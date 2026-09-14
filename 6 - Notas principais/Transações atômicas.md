---
date: 2026-09-13
time: 18:35
tags:
  - tecnologia
  - aprendizado
  - projetos
  - pix-ledger-api
status: rascunho
source:
aliases:
  - atomicity
  - Transactional
---

# Transações atômicas

## Resumo
A garantia de que **ou tudo acontece, ou nada acontece**. No [[Pix-ledger-api]], uma transferência é débito na origem + crédito no destino dentro de **um único commit** — se qualquer validade falhar, nada é gravado. É o pilar "Atomicidade" do projeto.

## Conteúdo

### O que o repo prova
- `transfer()` é um único `@Transactional`: debitou origem, creditou destino, salvou os dois com `saveAll`.
- O teste `transfer_insufficientFunds_isAtomic` prova: transferência de 100 com saldo 50 → `422` e o banco fica **exatamente como estava** (só o crédito de fundo permanece; nenhum lançamento da transferência foi gravado).
- Validações que abortam tudo: saldo insuficiente (`422`), conta inativa (`409`), conta ausente (`404`), origem = destino (`400`).

### Bloco de montagem
```
@Transactional
mensagem de commit
├── lock pessimista nas contas (ordem de UUID)   ← ver [[Lock pessimista]]
├── validar saldo
├── gravar lançamento débito + crédito
├── atualizar saldos
├── publicar eventos (só após o COMMIT)          ← ver [[Eventos após o commit]]
└── COMMIT (ou ROLLBACK de tudo)
```

### Erros de negócio × erros de infra
Os erros de negócio viram `ResponseStatusException` (rollback silencioso e previsível). A semântica de status é documentada: `400` validação, `401` JWT, `404` conta, `409` conflito/inativa, `422` saldo insuficiente.

### Conceito relacionado — os 4 pilares do projeto
| Pilar | Mecanismo |
|---|---|
| Atomicidade | transferência = débito + crédito no mesmo `@Transactional` |
| Concorrência | lançamentos com [[Lock otimista]] + retry |
| Eventos | publicado após COMMIT via `@TransactionalEventListener` |
| Leitura | [[Cache e invalidação]] de records no Redis |

## Conexões
- [[Lock pessimista]] — o lock só tem efeito dentro da transação
- [[Lock otimista]] — mesma base transacional, outra estratégia
- [[Eventos após o commit]] — o que acontece depois do commit
- [[Idempotência]] — projeção do que foi commitado
- [[Testes de integração com Testcontainers]] — onde a atomicidade é provada
- [[Pix-ledger-api]] — nota índice do projeto

## Ação
- Para entrevista: explicar a diferença entre rollback de exceção de negócio vs. de infra, e por que o lock pessimista precisa do `@Transactional`.
- Referência teórica: Cap. 7 do DDIA (Transações) — do que a atomicidade protege (falhas e concorrência).

---

# Referências
- Repo: `pix_ledger_api/src/main/java/com/pixledgerapi/service/AccountService.java`
- README do repo (tabela "Os quatro pilares")