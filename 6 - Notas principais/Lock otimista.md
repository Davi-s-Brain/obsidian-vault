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
  - lock otimista
  - optimistic lock
  - @Version
---

# Lock otimista

## Resumo
Estratégia de concorrência que **não trava nada no banco**: cada registro carrega uma coluna `version`, e o `UPDATE` só vale se a versão que você leu ainda é a atual. Se outra transação committou antes, você recebe `ObjectOptimisticLockingFailureException` — relê e tenta de novo (retry). Usado no [[Pix-ledger-api]] para lançamentos individuais (`POST /entries`).

## Conteúdo

### Como foi implementado
- Coluna `@Version` na entidade `Account` — o JPA incrementa a versão a cada `UPDATE` e adiciona `WHERE version = ?` no SQL.
- Duas transações leem o mesmo saldo: a que committar primeiro vence; a outra falha com `ObjectOptimisticLockingFailureException`, **relê o saldo** e tenta de novo — até `MAX_RETRIES = 3`.
- Esgotou o retry? Resposta **409 Conflict** (`"Conflito de concorrência ao atualizar saldo, tente novamente"`).

### A pegadinha do Spring proxy (lição valiosa)
O retry funciona porque chama o método transacional **através do proxy do Spring**:
- `self.createEntryTx(...)` onde `self` é uma **auto-referência com `@Lazy`**.
- Chamar `this.createEntryTx(...)` (self-invocation puro) **não passa pelo proxy** → a anotação `@Transactional` é ignorada e o retry perde o "reler com transação nova".
- Reler o saldo na mesma transação não adianta: você veria a versão velha do snapshot.

### Quando usar (e quando não)
- Vantagem: sem lock de banco, mais throughput em leitura; custa retry em contenção.
- Funciona bem quando a contenção é baixa/média. Sob contenção alta o retry exaurido vira 409 — no teste de carga os 409 são **esperados e observados** (métrica de negócio, não erro).

## Conexões
- [[Lock pessimista]] — o contraste: travar vs. retry; transferência vs. lançamento
- [[Transações atômicas]] — o retry depende de reler em transação nova
- [[Métricas e observabilidade]] — o 409 vira timer `result=conflict`
- [[Lições aprendidas - Pix Ledger]] — a pegadinha do proxy entrou pra lista
- [[Pix-ledger-api]] — nota índice do projeto

## Ação
- Para entrevista: demonstrar o fluxo completo — versão lida, UPDATE condicional, exceção, retry, 409.
- Ler o Cap. 7 do DDIA — o lock otimista é o "detecting lost updates" na prática.

---

# Referências
- Repo: `pix_ledger_api/src/main/java/com/pixledgerapi/service/AccountService.java`
- Repo: `pix_ledger_api/src/main/java/com/pixledgerapi/model/Account.java` (`@Version`)