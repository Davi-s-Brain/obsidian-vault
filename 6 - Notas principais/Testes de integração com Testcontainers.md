---
date: 2026-09-13
time: 18:35
tags:
  - tecnologia
  - aprendizado
  - projetos
  - testes
  - pix-ledger-api
status: rascunho
source:
aliases:
  - testcontainers
  - testes de integração
---

# Testes de integração com Testcontainers

## Resumo
O [[Pix-ledger-api]] roda **28 testes de integração** contra Postgres e Kafka **reais** (subidos via Testcontainers) — sem mock de infra. Foi essa escolha que pegou a classe de bugs que mock nunca pegaria, incluindo os de serialização e o fluxo assíncrono Kafka.

## Conteúdo

### A cobertura (28 testes)
| Grupo | O que cobre |
|---|---|
| 13 controller/contas | criar conta, saldo, extrato paginado... |
| 7 transferências | atomicidade, conta inativa, 404, mesma conta, e o **teste de concorrência** |
| 3 Kafka | E2E produtor→consumer, **duplicidade vira 1 linha só**, transferência gera 2 eventos |
| 4 auth | login JWT |
| 1 context | o contexto Spring sobe |

### O teste de concorrência (o mais foda)
`transfer_concurrent_serializesWithoutLostUpdate`: 2 threads com `CountDownLatch` (barreira pra largada simultânea), cada uma transferindo **80 reais de uma conta com saldo 100**:
- Asserta `[201, 422]` em qualquer ordem — exatamente um venceu, outro viu saldo insuficiente;
- Saldo final **20,00** — sem duplo débito;
- Exatamente **1 DEBIT** saiu da origem.

### Teste assíncrono (o jeito certo)
O consumer é assíncrono, então o teste **espera a projeção aparecer** com poll + deadline (15s) — não fica em sleep cego, nem acorda do Kafka em memória. `awaitProjections(n)` relê até chegar no número esperado.

### Por que Testcontainers e não mock
- Mock de Kafka nunca teria validado a serialização JSON (JsonSerializer/JsonDeserializer), o `trusted.packages` nem o redelivery.
- Mock de JPA nunca teria pego o `Cannot serialize value of type Account` do cache (ver [[Lições aprendidas - Pix Ledger]]).
- A infra de teste é **declarada via `@Import(TestcontainersConfiguration.class)`** — Postgres + Kafka reais sobem pro teste e caem depois.

### O trade-off honesto
Teste de infra real é mais lento e mais pesado que teste unitário. A compensação: uma suíte pequena de integração (28) cobre o que importa, e o resto fica no unitário. Foco no que funciona: **simular o mínimo, integrar o crítico**.

## Conexões
- [[Transações atômicas]] — onde a atomicidade é provada (rollback sem gravar nada)
- [[Lock pessimista]] / [[Lock otimista]] — os testes de corrida de saldo
- [[Idempotência]] — teste de duplicidade com Kafka real
- [[Lições aprendidas - Pix Ledger]] — lição 8: infra real nos testes
- [[Pix-ledger-api]] — nota índice do projeto

## Ação
- Para entrevista: história pronta — "28 testes de integração com Postgres e Kafka reais via Testcontainers, incluindo corrida de saldo com CountDownLatch".
- Próximo passo do plano: cobertura JaCoCo > 70% + CI no GitHub Actions.

---

# Referências
- Repo: `pix_ledger_api/src/test/java/com/pixledgerapi/` (TransferControllerTest, LedgerEventKafkaTest, TestcontainersConfiguration)
- README do repo (seção "Testes")