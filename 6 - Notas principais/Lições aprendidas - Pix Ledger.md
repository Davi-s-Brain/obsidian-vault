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
  - lições
  - lessons learned
---

# Lições aprendidas - Pix Ledger

## Resumo
O que o [[Pix-ledger-api]] ensinou na prática — bugs reais, pegadinhas de framework e decisões que só o teste de carga/falha revele seriam caras em produção. Cada lição tem o mecanismo que a gerou e o porquê.

## Conteúdo

### 1. Cache nunca guarda entidade JPA 🥇
- O sintoma: teste de carga estourou `Cannot serialize value of type Account` — o `@Cacheable` tentava mandar a **entidade** para o Redis.
- A causa: entidade carrega proxy/lazy/contexto de persistência; serializar isso fora do banco quebra.
- A correção: cache de **records DTO** (`AccountResponse`, `LedgerEntryResponse`) com o mesmo contrato JSON. → [[Cache e invalidação]]

### 2. Self-invocation ignora @Transactional (proxy do Spring)
- Retry do [[Lock otimista]] chamando `this.createEntryTx()` não passava pelo proxy → sem transação nova → reler saldo não servia.
- Correção: auto-referência `@Autowired @Lazy private AccountService self` e chamar `self.createEntryTx(...)`.
- Lição geral: **no Spring, anotação só funciona quando o método é chamado de fora do bean**.

### 3. @CacheEvict não suporta 2 chaves dinâmicas
- Transferência invalida o cache de **duas** contas; anotação não cobre → evict programático via `CacheManager`.
- Lição: anotação resolve o caso comum; a API programática (`cacheManager.getCache("accounts").evict(id)`) existe pra quando a anotação não expressa o caso.

### 4. A página do extrato é parte da chave do cache
- `ledgers` com chave `#id + ':' + #pageable` — muda a página, muda a chave; invalidação = limpar todas as páginas (`allEntries` ou `clear()`).

### 5. Métrica de negócio ≠ erro de sistema
- 409 sob contenção é **comportamento esperado** no teste de carga (thresholds contam 5xx, não 4xx de negócio). Duplicidade de evento vira **métrica** (`ledger.event.duplicated`), não log de erro. → [[Métricas e observabilidade]]

### 6. Ordering de locks previne deadlock
- Locks pessimistas adquiridos em **ordem de UUID** → sem espera circular. Teste de concorrência prova: 160 transferências, zero deadlock. → [[Lock pessimista]]

### 7. Higiene de repo é parte do trabalho
- O `AGENTS.md` da raiz carrega conteúdo copiado de outro repo (skills de design web — irrelevante pro projeto). O `memory.md` marcou como pendência: "AGENTS.md deve refletir o projeto". Um repositório de portfólio com AGENTS errado passa uma imagem ruim — e qualquer agente/IA que leia o repo vai se confundir.

### 8. Infra real nos testes vale a pena
- Testcontainers com Postgres e Kafka **reais** (sem mock) foi o que pegou a classe de bugs de serialização/integração. Mock de Kafka nunca teria pego o problema de realismo de delivery/redelivery. → [[Testes de integração com Testcontainers]]

## Conexões
- [[Cache e invalidação]] · [[Lock otimista]] · [[Lock pessimista]] · [[Idempotência]]
- [[Métricas e observabilidade]] · [[Teste de carga com k6]] · [[Testes de integração com Testcontainers]]
- [[Pix-ledger-api]] — nota índice do projeto

## Ação
- As lições 2, 3 e 5 são **perguntas clássicas de entrevista Java/Spring** — ter os exemplos do repo na ponta da língua.
- Pendência real do repo: substituir o AGENTS.md da raiz (lição 7).

---

# Referências
- README do repo (seção "Teste de carga" — onde o bug real foi pego)
- `memory.md` do repo (pontos de atenção)
- Git log: 9 commits entre 2026-08-30 e 2026-09-13