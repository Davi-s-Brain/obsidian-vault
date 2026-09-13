---
date: 2026-09-13
time: 18:35
tags:
  - tecnologia
  - aprendizado
  - projetos
  - mensageria
  - pix-ledger-api
status: rascunho
source:
aliases:
  - idempotência
  - at-least-once
---

# Idempotência

## Resumo
Fazer a mesma operação duas vezes ter o **mesmo efeito de uma vez só**. O Kafka entrega "pelo menos uma vez" (redelivery, restart), então o mesmo evento pode chegar 2× — no [[Pix-ledger-api]] o consumer usa a **PK = entry_id** para que o duplicado vire no-op e seja contado numa métrica.

## Conteúdo

### O problema
No padrão at-least-once, reprocessar uma mensagem é o comportamento normal. Sem proteção, um restart do consumer duplica a projeção: a auditoria ficaria com 2 linhas para 1 lançamento.

### A solução no repo (`LedgerEventConsumer`)
```java
if (entryEventRepository.existsById(event.entryId())) {
    meterRegistry.counter("ledger.event.duplicated").increment();
    return;                    // ← duplicado vira no-op
}
// grava a projeção (EntryEvent) com entry_id como PK
```
- A tabela `entry_events` (projeção/auditoria) usa o **`entry_id` do lançamento original como chave primária** — o banco é a barreira de duplicidade.
- Duplicado não é erro: é **métrica** — `ledger.event.duplicated` (ver [[Métricas e observabilidade]]).

### O teste que prova
`LedgerEventKafkaTest.duplicateEvent_isNotPersistedTwice`: envia o MESMO evento 2× no tópico → `assertThat(awaitProjections(1)).hasSize(1)` — uma linha só.

### Idempotência em outras camadas (relação)
- No Kafka: PK natural = chave de idempotência.
- No retry de [[Lock otimista]]: reler e tentar não duplica porque o lance é o mesmo `LedgerEntry`.
- Na transferência: o mesmo commit grava 1 débito + 1 crédito → 2 eventos com entry_ids distintos → 2 projeções (teste cobre).

## Conexões
- [[Eventos após o commit]] — o produtor do evento (publisher AFTER_COMMIT)
- [[Métricas e observabilidade]] — `ledger.event.duplicated` = saudade da idempotência em números
- [[Testes de integração com Testcontainers]] — o teste de duplicidade roda com Kafka real
- [[Lições aprendidas - Pix Ledger]] — "sem idempotência, restart duplica" é lição clássica
- [[Pix-ledger-api]] — nota índice do projeto

## Ação
- Para entrevista: unir os conceitos — Kafka at-least-once → consumer deve ser idempotente → PK natural/negocial é a barreira.
- Leitura: DDIA Cap. 11 (Streams) e Cap. 12 (o capítulo dos problemas de duplicidade).

---

# Referências
- Repo: `pix_ledger_api/src/main/java/com/pixledgerapi/event/LedgerEventConsumer.java`
- Repo: `pix_ledger_api/src/test/java/com/pixledgerapi/LedgerEventKafkaTest.java`