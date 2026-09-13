---
date: 2026-09-13
time: 18:35
tags:
  - tecnologia
  - aprendizado
  - projetos
  - dinheiro
  - pix-ledger-api
status: rascunho
source:
aliases:
  - BigDecimal
  - dinheiro
---

# Dinheiro e BigDecimal

## Resumo
Dinheiro é **exato** — float/double são aproximados. Todo valor monetário do [[Pix-ledger-api]] é `BigDecimal` com `precision = 19, scale = 2`, e toda comparação usa `compareTo` (nunca `equals`). Parece detalhe; é a diferença entre extrato certo e extrato com centavos fantasmas.

## Conteúdo

### O que o repo faz
- `Account.balance`: `@Column(nullable = false, precision = 19, scale = 2)` + `BigDecimal.ZERO` como default.
- Lançamentos e transferências: montantes como `BigDecimal` vindos de `EntryDTO` / `TransferDTO`.
- Validação de saldo: `balance.compareTo(amount) < 0` → 422 — **`compareTo`, nunca `equals`** (`equals` compara escala: `1.0 != 1.00`).
- Saldo: `balance.subtract(amount)` / `balance.add(amount)` — operações imutáveis que retornam novo valor.

### Por quê
- `double`/`float`: 0.1 + 0.2 = 0.30000000000000004 — centavos se perdem em aritmética binária.
- `BigDecimal` guarda o valor decimal exato; `scale = 2` fixa centavos; `precision = 19` dá trilionários de sobra.
- Para somar valores de coluna no banco (se um dia): `SUM` numérico no Postgres mantém a exatidão — a validação de saldo no repo é em memória (dentro do lock), então o BigDecimal é o guardião.

### O teste que blinda
As asserções de saldo usam `isEqualByComparingTo("80.00")` — o AssertJ compara o **valor numérico**, não a escala. O teste de concorrência confirma saldo final exato `20,00` após 160 transferências concorrentes.

## Conexões
- [[Transações atômicas]] — money path é o motivo de atomicidade (nunca débito sem crédito)
- [[Lock pessimista]] / [[Lock otimista]] — validação de saldo exato dentro do lock
- [[Testes de integração com Testcontainers]] — asserções `compareTo` os testes
- [[Pix-ledger-api]] — nota índice do projeto

## Ação
- Para entrevista: pergunta clássica de banco/fintech — "por que BigDecimal e não double?" com o exemplo 0.1 + 0.2 e a pegadinha do `equals` x `compareTo`.

---

# Referências
- Repo: `pix_ledger_api/src/main/java/com/pixledgerapi/model/Account.java`
- Repo: `pix_ledger_api/src/main/java/com/pixledgerapi/service/AccountService.java`