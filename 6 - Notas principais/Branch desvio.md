---
date: 2026-09-15
time: 11:35
tags:
  - arquitetura
  - computadores
  - pipeline
  - branch
status: rascunho
source:
matéria: OAC II
aliases:
  - branch
  - desvio
  - branch prediction
cards-deck: OAC II
flashcards:
  q-7gst: { nid: 1789483541673, hash: pcuizt3r, sync: tbrmnrq5 }
  q-dxp4: { nid: 1789483541698, hash: s98yyjbd, sync: 3gedyikk }
  q-29zn: { nid: 1789483541722, hash: drtrwemd, sync: i8qffqgv }
  q-4dht: { nid: 1789483541748, hash: rtm8vnfg, sync: 3tyacq9u }
  q-szf5: { nid: 1789483541772, hash: i9tfg4ni, sync: 43few89s }
  q-usr2: { nid: 1789483541798, hash: g6zkbv9r, sync: c9vh3esq }
  q-zg5m: { nid: 1789483541822, hash: jubq9rux, sync: biveefuj }
---

# Branch desvio

## Resumo
Desvios (branches) impedem a pipeline de encher, causando penalidade de performance. As mitigações são múltiplos fluxos, busca antecipada da instrução alvo e previsão de desvio — sempre apostando no caso comum de que a execução continua.

## Conteúdo

### O problema
- Desvios impedem a pipeline de encher → penalidade de performance
- A unidade de controle aposta no caso comum: a execução continua (busca a próxima instrução sequencial)

### Contras de uma máquina com pipeline
- Mais complexa, principalmente na lógica de controle de estágios e dependências
- Penalidades de custo na movimentação de dados entre memória e processador
- Número típico de estágios: 6 a 9

### Desempenho
- Pipeline de K estágios com N instruções: tempo total = (K + N − 1) × t
- Sem pipeline: N × K × t
- Speedup tende a **K** (nº de estágios) para N grande — ver [[Pipeline]]

### Mitigações de desvio

1. **Múltiplos fluxos** — dois pipelines, mas só um executa por vez: um busca o fluxo do desvio tomado e o outro, o não tomado
   - Custo: conflitos de registradores e maior complexidade, coordenados pela unidade de controle
   - Com 2+ desvios sequenciais, a penalidade volta

2. **Busca antecipada da instrução alvo** — a instrução alvo já fica carregada; se o desvio for tomado, executa imediatamente

3. **Previsão de acessos** — aposta predefinida no resultado do desvio:
   - **Nunca ocorrerá:** sempre carrega a próxima instrução sequencial
   - **Sempre ocorrerá:** sempre carrega a instrução alvo

4. **Loop buffer (memória em loop de repetição)** — para loops, verifica se as instruções já estão no cache do processador antes de buscar na memória
   - Apoia-se na **localidade de referência**
   - Eficaz para loops e pequenos jumps

### Definições do contexto de memória

- **Buffer:** armazenamento temporário — os dados só existem nele durante uma transferência
- **Cache (hardware):** cópia de dados de uma memória mais lenta para uma mais rápida

## Conexões
- [[Pipeline]] — branch é o hazard de controle da pipeline (previsão de desvio)
- [[Introdução]] — desempenho e speedup (paralelo com a Lei de Amdahl)
- [[Risc X Cisc]] — seção RISC/CISC desta aula será processada naquela nota

## Ação

Por que desvios prejudicam a pipeline?::Impedem a pipeline de encher, causando penalidade de performance.
^q-7gst

Qual a aposta padrão da unidade de controle diante de um desvio?::Que a execução continua — sempre busca a próxima instrução sequencial (o caso comum).
^q-dxp4

O que é a previsão de desvio "nunca ocorrerá" e "sempre ocorrerá"?::Nunca: sempre carrega a próxima instrução sequencial. Sempre: sempre carrega a instrução alvo do desvio.
^q-29zn

O que é a técnica de múltiplos fluxos?::Usar dois pipelines onde apenas um executa por vez — um busca o fluxo tomado e o outro o não tomado; custa complexidade e conflitos de registradores.
^q-4dht

O que é a busca antecipada da instrução alvo?::Deixar a instrução alvo já carregada, permitindo executá-la imediatamente se o desvio for tomado.
^q-szf5

O que é um loop buffer?::Para loops, verificar se as instruções já estão no cache do processador antes de buscá-las na memória, aproveitando a localidade de referência.
^q-usr2

O tempo total de uma pipeline de K estágios executando N instruções é ==(K + N − 1) × t==, e o speedup tende a ==K== para N grande.
^q-zg5m

---

# Referências
- Aula: OAC II - Organização e Arquitetura de Computadores