---
date: 2026-09-13
time: 18:35
tags:
  - tecnologia
  - aprendizado
  - projetos
  - concorrência
  - pix-ledger-api
status: rascunho
source:
aliases:
  - lock pessimista
  - SELECT FOR UPDATE
---

# Lock pessimista

## Resumo
Estratégia de concorrência onde quem escreve **bloqueia o registro na leitura**, serializando o acesso ao recurso. No [[Pix-ledger-api]] é usado na transferência: as duas contas são travadas com `SELECT ... FOR UPDATE` **em ordem de UUID** antes de debitar/creditar.

## Conteúdo

### Quando usar
Quando a contenção é alta ou a operação precisa de mais de um registro consistente (débito + crédito). Paga com latência (quem chega depois espera) mas elimina o retry — diferente do [[Lock otimista]].

### Como foi implementado
- `AccountRepository.findAllByIdForUpdate`: `@Lock(LockModeType.PESSIMISTIC_WRITE)` + `@Query("select a from Account a where a.id in :ids order by a.id")`.
- A **ordem por UUID é determinística**: duas transferências concorrentes que envolvem as mesmas contas sempre adquirem os locks na mesma sequência → **nunca há deadlock** (deadlock acontece quando cada transação segura um lock e espera o outro).
- Todo o fluxo (validar saldo, débito na origem, crédito no destino) acontece dentro do mesmo `@Transactional` — ver [[Transações atômicas]].

### Limite físico do lock
O lock serializa **por par de contas**: transferências do mesmo par formam fila, e o throughput máximo ≈ `1 / tempo de lock`. No repo isso é tratado como experimento didático: o teste de carga escala o número de pares (`PAIRS`) para subir o teto — ver [[Teste de carga com k6]]. Em produção, é o trade-off clássico: consistência máxima × vazão por conta.

### Validação
Teste de concorrência com `CountDownLatch`: 2 × 80 transferências em paralelo de uma conta com saldo 100 → exatamente **um 201 e um 422**, saldo final 20, e **exatamente 1 débito** na origem (sem duplo débito nem saldo negativo). Ver [[Testes de integração com Testcontainers]].

## Conexões
- [[Lock otimista]] — o complemento: usado no lançamento individual, sem lock de banco
- [[Transações atômicas]] — o lock só faz sentido dentro da transação
- [[Teste de carga com k6]] — a fila serializada vira p95 e teta de vazão
- [[Testes de integração com Testcontainers]] — como a concorrência foi provada
- [[Pix-ledger-api]] — nota índice do projeto

## Ação
- Para entrevista: saber explicar por que ordenar o lock elimina deadlock e o trade-off com vazão.
- Ler o Cap. 7 do DDIA (Transações) — o lock pessimista é a seção "Two-phase locking" na prática.

---

# Referências
- Repo: `pix_ledger_api/src/main/java/com/pixledgerapi/repository/AccountRepository.java`
- README do repo (seção "Concorrência — como foi validada")