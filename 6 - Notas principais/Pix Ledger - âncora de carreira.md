---
date: 2026-09-13
time: 18:35
tags:
  - carreira
  - aprendizado
  - projetos
  - pix-ledger-api
status: rascunho
source:
aliases:
  - projeto âncora
  - portfólio
---

# Pix Ledger - âncora de carreira

## Resumo
O pix-ledger-api é o **projeto âncora** do plano de evoluir de **Jr alto → Pleno até Mar/2027**, com apelo direto a recrutadores de banco/fintech. Ele foi desenhado pra falar a língua do mercado: concorrência, consistência, mensageria e observabilidade — os temas que separam um backend Jr de um Pleno.

## Conteúdo

### Por que esse projeto fala com recrutador de banco/fintech
- **É um domínio financeiro**: extrato, débito/crédito, transferência, saldo — vocabulário que o entrevistador de banco entende de imediato.
- **São os temas de Pleno**: [[Lock pessimista]], [[Lock otimista]], [[Transações atômicas]], [[Eventos após o commit]], [[Idempotência]], [[Cache e invalidação]].
- **Tem prova, não só código**: [[Testes de integração com Testcontainers]] (28 testes, concorrência real) + [[Teste de carga com k6]] (~1.012 req/s, p95 em ms) + [[Métricas e observabilidade]] (dashboard como código).

### Stack que o mercado pede (e o repo tem)
Java 26 · Spring Boot 4.1.1 · Postgres 16 · Liquibase · Kafka (KRaft) · Redis · Spring Security JWT · Micrometer + Prometheus + Grafana · k6 · Testcontainers · Docker Compose.

### Histórias prontas pra entrevista (narrativa do repo)
1. "O teste de carga pegou um bug de produção real" → [[Lições aprendidas - Pix Ledger]] (cache de entidade JPA no Redis).
2. "Descobri a pegadinha do proxy do Spring" → self-invocation ignorava `@Transactional`; correção com auto-referência `@Lazy`.
3. "Sei quantos 422 são saldo insuficiente e quão lentos eles são" → timers por resultado.
4. "Deadlock não existe na minha transferência" → locks em ordem de UUID, provado por teste de corrida.

### Estado e próximos passos (Fase 4 do plano)
- ✅ Projeto completo e funcional (2026-09-13): auth JWT, contas, lançamentos, transferências, Kafka, cache, métricas, dashboard, k6, README.
- ⬜ **AGENTS.md da raiz errado** (não reflete o projeto) — pendência de higiene.
- ⬜ JaCoCo > 70% + CI GitHub Actions.
- ⬜ LinkedIn: Featured com o link do repo + About citando o projeto.
- ⬜ Treino de entrevista Pleno: 22 perguntas do diagnóstico + mock interviews.

## Conexões
- [[Pix-ledger-api]] — nota índice técnica do projeto (todas as notas técnicas linkam daqui)
- [[Metas Profissionais]] · [[Dicas de entrevista]] — contexto de carreira do vault
- [[Lições aprendidas - Pix Ledger]] — o material das histórias de entrevista

## Ação
- Agenda: zerar as pendências da Fase 4 (começar pelo AGENTS.md e CI).
- Treinar as 4 histórias acima em voz alta até saírem em 2 minutos.

---

# Referências
- Plano mestre: `evolucao_carreira/docs/plans/2026-08-27-plano-pleno-pix-ledger.md`
- `memory.md` do repo (memória de sessão — perfil: Davi Batista · backend Java · SI-USP)