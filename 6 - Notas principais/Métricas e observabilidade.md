---
date: 2026-09-13
time: 18:35
tags:
  - tecnologia
  - aprendizado
  - projetos
  - observabilidade
  - pix-ledger-api
status: rascunho
source:
aliases:
  - métricas
  - prometheus
  - grafana
  - observabilidade
---

# Métricas e observabilidade

## Resumo
O [[Pix-ledger-api]] expõe **métricas de negócio** (não só de sistema) em `/actuator/prometheus`, escovadas pelo Micrometer e desenhadas num dashboard Grafana **provisionado como código** (zero clique). O diferencial: timers taggeados **por resultado**, para saber não só *quão rápido* mas *em qual desfecho*.

## Conteúdo

### Métricas do negócio
| Métrica | O que conta |
|---|---|
| `ledger.transfer.duration{result}` | tempo de transferência **por desfecho**: `success`, `insufficient`, `inactive`, `not_found`, `same_account`, `http_*` |
| `ledger.entry.duration{result}` | tempo de lançamento por desfecho: `success`, `insufficient`, `conflict`, `not_found` |
| `ledger.event.consumed{type}` | projeções gravadas, por tipo de lançamento (CREDIT/DEBIT) — saúde do pipeline |
| `ledger.event.duplicated` | eventos duplicados que a [[Idempotência]] barrou — quão frequente é a redelivery |

### Como foram feitas
- `Timer.start(meterRegistry)` + `sample.stop(timer(result))` em `try/finally` no `AccountService` — o resultado é mapeado do status HTTP pro rótulo de negócio (422 → `insufficient`, 409 → `conflict`/`inactive`…).
- Trocar **fluxo de controle por métrica**: introduzir um desfecho novo é um `case` no switch, sem tocar nas regras.

### Percentis precisos
`management.metrics.distribution.percentiles-histogram.*` habilitado pra HTTP e pros dois timers de negócio → histogramas com **p95/p99 confiáveis** no Prometheus.

### Sistema + Grafana
- `http.server.requests` (throughput/erros 4xx/5xx), `jvm_memory_used`, `kafka_consumer_fetch_manager_records_lag` (o clássico: degradação lenta do consumer aparece no lag antes do usuário reclamar).
- Grafana **provisionado como código** (`grafana/provisioning/` + JSON do dashboard no git) — a stack inteira nasce com `docker compose up`, sem configuração manual.

### Números de referência (carga 30s, máquina dev)
~1.012 req/s totais · p95 leitura 4,5 ms · p95 transferência 6,9 ms · 0,03% de erro (só os 409 esperados). Ver [[Teste de carga com k6]].

## Conexões
- [[Teste de carga com k6]] — a carga que alimenta o dashboard
- [[Idempotência]] — `ledger.event.duplicated` mede a barreira de duplicidade
- [[Lock otimista]] — `result=conflict` = retry exaurido sob contenção
- [[Lições aprendidas - Pix Ledger]] — "métrica de negócio ≠ erro" (lição 5)
- [[Pix-ledger-api]] — nota índice do projeto

## Ação
- Para entrevista: saber defender o dashboard "como código" e os timers por resultado (história: "sabemos quantos 422 são saldo insuficiente e quão lentos eles são").
- Próximo passo do plano do projeto: traceId/OpenTelemetry e logs JSON (Fase 4).

---

# Referências
- Repo: `pix_ledger_api/src/main/resources/application.properties` (config de métricas)
- Repo: `grafana/provisioning/dashboards/ledger-dashboard.json` · `prometheus/prometheus.yml`
- README do repo (seção "Observabilidade")