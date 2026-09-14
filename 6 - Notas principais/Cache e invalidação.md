---
date: 2026-09-13
time: 18:35
tags:
  - tecnologia
  - aprendizado
  - projetos
  - cache
  - pix-ledger-api
status: rascunho
source:
aliases:
  - cache
  - Redis
  - Cacheable
---

# Cache e invalidação

## Resumo
O [[Pix-ledger-api]] cacheia **leitura** (conta e extrato paginado) no Redis para segurar a carga de leitura. A regra de ouro aprendida na prática: **cache guarda records serializáveis, nunca entidade JPA** — e toda escrita precisa invalidar a leitura correspondente.

## Conteúdo

### O que é cacheado
- `GET /accounts/{id}` → cache `accounts`, chave `#id`.
- `GET /accounts/{id}/ledger?page&size` → cache `ledgers`, chave `#id + ':' + #pageable` — a página faz parte da chave; invalidar = limpar todas as páginas da conta.

### Invalidação em cada escrita
- **Lançamento** (`createEntryTx`): `@Caching(evict = { @CacheEvict(accounts, key=#accountId), @CacheEvict(ledgers, allEntries=true) })`.
- **Transferência**: as anotações **não suportam 2 chaves dinâmicas** → evict programático com `cacheManager.getCache(...).evict(id)` para as duas contas + `ledgers.clear()`.

### ⚠ A lição mais cara do projeto (bug real)
- O teste de carga pegou: `@Cacheable` tentava serializar **a entidade JPA** no Redis → `Cannot serialize value of type Account` (erro de produção).
- Correção: o cache passou a guardar **records DTO serializáveis com o mesmo contrato JSON** (`AccountResponse`, `LedgerEntryResponse`) — serialização estável, sem lazy loading nem proxies.
- Regra derivada: **nunca** colocar entidade no Redis. DTO/record para fora, entidade para dentro.

### Config que faz diferença
- `spring.cache.type=redis` + `spring.cache.cache-names=accounts,ledgers`: pre-criar os caches no startup evita cache "lazy" e faz o Boot registrar as métricas `cache_gets/cache_puts` desde o início.

### Medição
O cenário de leituras do k6 (20 VUs, tudo batendo no cache) rodou a **p95 = 4,5 ms, 0% de erro** — ver [[Teste de carga com k6]].

## Conexões
- [[Lições aprendidas - Pix Ledger]] — o bug do JPA no Redis é a lição nº 1
- [[Transações atômicas]] — pilar "Leitura" dos 4 pilares
- [[Métricas e observabilidade]] — cache_gets/puts no Grafana
- [[Teste de carga com k6]] — o cenário `reads` prova o caminho cacheado
- [[Pix-ledger-api]] — nota índice do projeto

## Ação
- Para entrevista: "o que aprendi com cache" = história do record vs. entidade + invalidação por escrita com evict de página.
- Próximo passo possível: TTL explícito e fallback Caffeine (citado no plano do projeto como melhoria).

---

# Referências
- Repo: `pix_ledger_api/src/main/java/com/pixledgerapi/service/AccountService.java`
- Repo: `pix_ledger_api/src/main/resources/application.properties`