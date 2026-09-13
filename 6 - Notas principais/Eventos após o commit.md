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
  - AFTER_COMMIT
  - TransactionalEventListener
  - transacional outbox
---

# Eventos após o commit

## Resumo
A regra de ouro do padrão **transactional outbox** aplicada de forma simples: o evento só vai pro Kafka **depois que o banco confirmou o commit**. Publicar dentro da transação vazaria eventos de operações que deram rollback — o `@TransactionalEventListener(phase = AFTER_COMMIT)` impede isso no [[Pix-ledger-api]].

## Conteúdo

### O fluxo no repo
```
POST /entries ou /transfers
  └─ @Transactional (banco)
       ├─ grava lançamento no Postgres
       └─ applicationEventPublisher.publishEvent(EntryCreatedEvent)  ← dentro da tx
            └─ @TransactionalEventListener(AFTER_COMMIT)  [LedgerEventPublisher]
                 └─ kafkaTemplate.send("ledger.entry.created", key=entryId, event)
                      └─ [Kafka] consumer idempotente grava a projeção  ← ver [[Idempotência]]
```

### Por que o listener separado (e não publicar direto)
- `publishEvent` dentro da transação é **só um registro do evento em memória** — o `TransactionalEventListener` segura e só dispara no commit.
- Commit falhou → evento nem chega ao Kafka. **Nunca existe "evento de transação que não existiu"** (frase do próprio código).
- A transferência gera **2 eventos no mesmo commit** (débito + crédito) — o teste Kafka prova que os dois chegam.

### Limitação conhecida (e por que ainda assim é ok)
- O publish pós-commit não é atômico com o commit de verdade: se o Kafka cair no exato momento, o evento se perde (sem outbox table). Para o porte do projeto, o AFTER_COMMIT + consumer idempotente já resolve o caso real de duplicidade — o único "buraco" é a perda rara.
- Se um dia precisar de garantia forte de entrega, o caminho é uma tabela `outbox` gravada **dentro** da transação + relay para o Kafka (o padrão completo do Cap. 11 do DDIA).

## Conexões
- [[Transações atômicas]] — o evento herda o destino do commit
- [[Idempotência]] — o outro lado do contrato (consumer tolera duplicado)
- [[Métricas e observabilidade]] — saúde da projeção contada por métrica
- [[Lições aprendidas - Pix Ledger]] — a ordem publish/commit é lição pra entrevista
- [[Pix-ledger-api]] — nota índice do projeto

## Ação
- Para entrevista: explicar por que publicar dentro de `@Transactional` vaza eventos órfãos (o texto do README: "nunca existe evento de transação que não ocorreu").
- Evolução documentada: tabela outbox completa se precisar de entrega garantida.

---

# Referências
- Repo: `pix_ledger_api/src/main/java/com/pixledgerapi/event/LedgerEventPublisher.java`
- Repo: `pix_ledger_api/src/main/java/com/pixledgerapi/event/EntryCreatedEvent.java`
- DDIA Cap. 11 (Streams de eventos) — o contexto do padrão outbox