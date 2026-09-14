---
date: 2026-09-13
time: 18:35
tags:
  - tecnologia
  - aprendizado
  - projetos
  - pix-ledger-api
status: Em andamento
source: https://github.com/Davi-s-Brain/pix-ledger-api
aliases:
  - Pix Ledger
  - pix-ledger-api
---

# Pix-ledger-api

## Resumo
Ledger de pagamentos estilo PIX com foco em **corretude sob concorrência** e **observabilidade de negócio**. Java 26 · Spring Boot 4.1.1 · Postgres 16 · Kafka · Redis · Prometheus/Grafana · k6 — cada mecanismo (lock pessimista, lock otimista com retry, eventos com idempotência, cache) validado por testes de integração e medido por teste de carga. Nota índice técnico do projeto — daqui nascem todas as notas de conceito e lições.

## Conteúdo

### Arquitetura (os 4 pilares)
| Pilar        | Mecanismo                                                               | Nota                                         |
| ------------ | ----------------------------------------------------------------------- | -------------------------------------------- |
| Atomicidade  | débito + crédito no mesmo `@Transactional`                              | [[Transações atômicas]]                      |
| Concorrência | lançamento com lock otimista + retry; transferência com lock pessimista | [[Lock otimista]] · [[Lock pessimista]]      |
| Eventos      | publicado após COMMIT, consumer idempotente                             | [[Eventos após o commit]] · [[Idempotência]] |
| Leitura      | cache de records no Redis                                               | [[Cache e invalidação]]                      |

### Conceitos
- [[Lock pessimista]] — `SELECT ... FOR UPDATE` em ordem de UUID (sem deadlock)
- [[Lock otimista]] — `@Version` + retry via proxy (self com `@Lazy`), 409 após esgotar
- [[Transações atômicas]] — transferência que falha sem gravar nada
- [[Eventos após o commit]] — `@TransactionalEventListener(AFTER_COMMIT)`; nunca evento de transação que não ocorreu
- [[Idempotência]] — PK = entry_id; duplicado vira no-op e métrica
- [[Cache e invalidação]] — records serializáveis (nunca entidade JPA!) e evict por escrita
- [[Dinheiro e BigDecimal]] — precisão 19, scale 2, `compareTo` nunca `equals`

### Medição e prova
- [[Testes de integração com Testcontainers]] — 28 testes, Postgres+Kafka reais, corrida de saldo com CountDownLatch
- [[Teste de carga com k6]] — ~1.012 req/s, p95 leitura 4,5 ms / transferência 6,9 ms, 0,03% de erro
- [[Métricas e observabilidade]] — timers por resultado, `ledger.event.duplicated`, dashboard Grafana como código

### Aprendizado e carreira
- [[Lições aprendidas - Pix Ledger]] — o bug real do cache de JPA no Redis e outras 7
- [[Pix Ledger - âncora de carreira]] — o projeto como alavanca Jr alto → Pleno
- [[Diagrama do pix]] — o desenho conceitual

### API em uma linha
`POST /auth/login` (JWT) · `POST /accounts` · `GET /accounts/{id}` (cache) · `GET /accounts/{id}/ledger` (cache paginado) · `POST /accounts/{id}/entries` (lock otimista) · `POST /transfers` (lock pessimista). Erros: 400 validação · 401 JWT · 404 conta · 409 conflito/inativa · 422 saldo insuficiente.

## Conexões
Todas as notas acima · [[Diagrama do pix]] · [[Conceitos-chave]] (tema DDIA: transações e streams)

## Ação
- Zerar pendências da Fase 4: AGENTS.md do repo, JaCoCo + CI, LinkedIn Featured.
- Readme do projeto já está sólido: as notas acima são o caminho de estudo pra entrevista Pleno.

---

# Referências
- Repo: https://github.com/Davi-s-Brain/pix-ledger-api (local: `~/Documents/Projetinhos/pix-ledger-api`)
- README do repo · `memory.md` do repo · DDIA Caps. 7 e 11