---
date: 2026-09-13
time: 18:35
tags:
  - tecnologia
  - aprendizado
  - projetos
  - performance
  - pix-ledger-api
status: rascunho
source:
aliases:
  - k6
  - teste de carga
  - load test
---

# Teste de carga com k6

## Resumo
Três cenários simultâneos no [[Pix-ledger-api]] que provam cada mecanismo de concorrência sob pressão real: leitura com cache, lançamento com [[Lock otimista]] (contenção) e transferência com [[Lock pessimista]] (serialização). O teste de carga tem crédito de ter **pego o bug de produção mais caro do projeto** — ver [[Lições aprendidas - Pix Ledger]].

## Conteúdo

### Os cenários (cada um prova um pilar)
| Cenário | Executor k6 | O que prova |
|---|---|---|
| `reads` | constant-vus (20 VUs) | caminho cacheado — p95 4,5 ms, 0% erro |
| `entries` | constant-arrival-rate 100/s | [[Lock otimista]] + retry sob contenção — 201 ou 409 (esperado), nunca 5xx |
| `transfers` | constant-arrival-rate 200/s | [[Lock pessimista]] serializando por par — p95 6,9 ms, 0% erro |

### Design inteligente do dataset
- Pares de contas A/B **fundidos** (`credit(1e6)` nos dois lados) → transferência alterna A→B / B→A e **o saldo do par nunca drena**, sustentando carga infinita.
- Uma conta "sink" com saldo gigante concentra todos os débitos do cenário `entries` → **contenção máxima** no lock otimista.
- `setup()` roda antes: login + criação do dataset via API real (sem seed no banco).

### Thresholds (a sutileza)
```
'http_req_failed{scenario:reads}':    ['rate<0.01']   // 5xx/timeout
'http_req_failed{scenario:transfers}':['rate<0.01']
'checks{scenario:entries}':           ['rate>0.95']   // 201 OU 409 contam como ok
'http_req_duration{scenario:reads}':  ['p(95)<200']
'http_req_duration{scenario:transfers}': ['p(95)<2000'] // teto generoso: lock serializa
```
- **4xx de negócio não são erro**: o check de entries aceita 409 (retry exaurido sob contenção). Erro de verdade é 5xx/timeout.
- O `p(95)<2000` da transferência reconhece a fila do lock: latência de cauda é o preço do serial.

### Resultado medido (máquina dev, 30s, 10 pares)
| Cenário | Carga | p95 | Erros |
|---|---|---|---|
| Leituras (cache) | 20 VUs | 4,5 ms | 0% |
| Transferências | 200/s | 6,9 ms | 0% |
| Lançamentos | 100/s | — | 0% (409 esperados) |
| **Total** | **~1.012 req/s** | — | **0,03%** (só 409) |

### O teto é didático
`transferências máx = PAIRS / tempo de lock` — o README é explícito: "o throughput de transferência é limitado pelo lock pessimista por par de contas. Escale PAIRS para subir o teto — é o experimento didático do lock." Ótima frase para defender a escolha em entrevista.

### Como rodar
```bash
docker run --network host -i grafana/k6 run - < k6/load-test.js
# variações: -e TRANSFERS_TPS=400 -e PAIRS=50 -e DURATION=60s
```

## Conexões
- [[Métricas e observabilidade]] — a carga alimenta o dashboard (timers por resultado)
- [[Lock pessimista]] — a fila serializada vira latência de cauda
- [[Lock otimista]] — os 409 esperados sob contenção
- [[Lições aprendidas - Pix Ledger]] — o bug do cache que o k6 pegou
- [[Pix-ledger-api]] — nota índice do projeto

## Ação
- Para entrevista: conseguir explicar o `constant-arrival-rate` (carga em req/s, não VUs) e a filosofia "4xx de negócio ≠ erro".
- Evolução do plano: rodar com PAIRS maior pra medir o teto real da máquina.

---

# Referências
- Repo: `k6/load-test.js`
- README do repo (seção "Teste de carga (k6)")