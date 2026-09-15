---
date: 2026-09-15
time: 11:27
tags:
  - arquitetura
  - computadores
  - desempenho
  - amdahl
status: rascunho
source:
matéria: OAC II
aliases:
  - introdução
  - arquitetura
  - amdahl
  - desempenho
cards-deck: OAC II
flashcards:
  q-bs3u: { nid: 1789482768999, hash: 6gjhrcgi, sync: bi7bt56h }
  q-vivm: { nid: 1789482769023, hash: 9wtjvgrm, sync: zt2cvbdx }
  q-jjtm: { nid: 1789482769047, hash: 2zek43nt, sync: h4qh4qd8 }
  q-rnim: { nid: 1789482769073, hash: py8wdj8r, sync: 8bxjx7kp }
  q-qjq8: { nid: 1789482769097, hash: 8553fkc7, sync: v2wygm43 }
  q-7nyw: { nid: 1789482769122, hash: t5hshkny, sync: einmh9t6 }
  q-k9s9: { nid: 1789482769147, hash: 7yacvjcm, sync: 4vx6e2e5 }
---

# Introdução

## Resumo
A máquina base tem um núcleo e executa um ciclo contínuo de busca e execução de instruções. O desempenho é medido pelo tempo de execução (influenciado pelo nº de instruções, CPI e clock), e otimizações são avaliadas pela Lei de Amdahl.

## Conteúdo

### Arquitetura vs Organização
- **Arquitetura:** o conjunto de instruções — atributos visíveis ao programador
- **Organização:** a interconexão dos componentes que implementa a arquitetura

### Ciclo de instrução

**Busca → Decodifica → Busca operandos → Executa → Guarda**

- **Program Counter (PC):** guarda o endereço da próxima instrução
- **Instruction Register (IR):** recebe a instrução ao final do ciclo de busca

### Desempenho

- Desempenho = tempo de execução = **nº de instruções × CPI × tempo de ciclo**
- Tempo de ciclo: quanto maior, mais lento o processador
- Regra de ouro: **focar o caso comum e torná-lo mais rápido**

### Lei de Amdahl

Quantifica o ganho de uma otimização, limitado pela fração do tempo em que ela é usada.

**Speedup = 1 / ((1 − F) + F / S)**

- F = fração do tempo em que a melhoria é utilizada
- S = aceleração da melhoria
- A mudança só vale a pena se Speedup > 1

**Exemplo:** melhoria 10× mais rápida, usada 40% do tempo → F = 0,4, S = 10 → Speedup = 1 / (0,6 + 0,4/10) = 1 / 0,64 ≈ **1,56**

### Prefetch

- Antecipa a busca da próxima instrução enquanto a atual é executada ("instruction prefetch")
- Em condições ideais, dobra o nº de instruções executadas; no mundo real a busca é mais rápida que a execução, então o ganho é menor

### Pipeline

- Sobreposição de estágios de execução — ver [[Pipeline]]
- Não é uma máquina paralela

## Conexões
- [[Pipeline]] — pipeline como evolução do prefetch (múltiplos estágios)
- [[Risc X Cisc]] — decisões de projeto do conjunto de instruções (arquitetura)

## Ação

Qual a diferença entre arquitetura e organização de um processador?::Arquitetura é o conjunto de instruções visível ao programador; organização é a interconexão dos componentes que a implementa.
^q-bs3u

O ciclo de instrução segue a sequência: ==Busca → Decodifica → Busca operandos → Executa → Guarda==.
^q-vivm

O Program Counter (PC) guarda o ==endereço da próxima instrução==.
^q-jjtm

O que diz a Lei de Amdahl?::O ganho de uma otimização é limitado pela fração do tempo em que ela pode ser utilizada.
^q-rnim

Qual a fórmula do Speedup da Lei de Amdahl?::Speedup = 1 / ((1 − F) + F / S), com F = fração de uso da melhoria e S = aceleração dela.
^q-qjq8

O que é prefetch?::Antecipar a busca da próxima instrução enquanto a atual ainda está sendo executada.
^q-7nyw

Qual a regra de ouro para otimizar desempenho?::Focar o caso comum e torná-lo mais rápido.
^q-k9s9

Pendente: revisar organização do processador e unidade de controle.

---

# Referências
- Aula: OAC II - Organização e Arquitetura de Computadores