---
date: 2026-09-15
time: 19:54
tags:
  - arquitetura
  - computadores
  - superescalar
  - ilp
status: rascunho
source:
matéria: OAC II
aliases:
  - superescalares
  - superescalar
  - ILP
  - paralelismo no nível de instrução
cards-deck: OAC II
flashcards:
  q-wgxn: { nid: 1789513246239, hash: kcfuy9e5, sync: bahgqxgw }
  q-b4z6: { nid: 1789513246264, hash: 8uskiiaz, sync: 2w9bf7wy }
  q-sa2m: { nid: 1789513246289, hash: ku6bizft, sync: d66vp7k4 }
  q-t85m: { nid: 1789513246314, hash: vfg25hmw, sync: vmtg8cit }
  q-iujt: { nid: 1789513246338, hash: 22iqxj77, sync: shyi7f5z }
  q-42uw: { nid: 1789513246364, hash: 2b3x7jbz, sync: 763gcbei }
  q-7gpe: { nid: 1789513246389, hash: cczrmcgu, sync: ujkfs8mw }
---

# Superescalares

## Resumo
Processador que executa múltiplas instruções independentes simultaneamente usando 2+ pipelines/unidades de execução em paralelo. Aproveita o paralelismo no nível de instrução (ILP) do programa, limitado por hazards, conflitos de recurso e dependências além das clássicas.

## Conteúdo

### Conceito
- Instruções usuais (load, store, desvio) são independentes → podem ser executadas simultaneamente
- Aplicável a CISC e RISC (mais comum em RISC)
- Nome vem de executar sobre "grandezas escalares" (dados individuais)
- Todo programa tem um potencial de paralelismo → **paralelismo no nível de instrução**

### Superescalar × Superpipeline

|             | Superescalar                               | Superpipeline                                      |
| ----------- | ------------------------------------------ | -------------------------------------------------- |
| Estrutura   | 2+ pipelines em paralelo                   | 1 pipeline "normal", mais profundo (mais estágios) |
| Ganho       | mais instruções por ciclo                  | clock mais alto (estágios mais finos)              |
| Paralelismo | no nível de instrução (múltiplas unidades) | temporal (estágios menores)                        |

### Paralelismo no nível de instrução (ILP)
- Potencial de paralelismo do programa, explorado com otimização do compilador
- Limitado por:
  - Hazards de dados
  - Hazards de desvio — ver [[Branch desvio]]
  - Conflito de recurso
  - **Dependência de saída** (novo)
  - Antidependência
- Execução pode ser sobreposta; depende da ISA

### Paralelismo de máquina (nível 2)
- Definido pelo número de pipelines
- Depende do mecanismo de identificação de instruções independentes
- Mede a capacidade do processador aproveitar o ILP do programa

### Política de Iniciação de Instruções
O processador precisa conhecer as próximas instruções e a ordem delas:
- Ordem de **busca**
- Ordem de **execução**
- Ordem de **atualização** de registradores e memória

Não dá para jogar todas as instruções no pipeline de uma vez — causa muitos hazards.

### Iniciação em ordem com terminação fora de ordem
- Instruções iniciam em ordem, mas terminam fora de ordem
- Melhora o desempenho com instruções de vários ciclos
- Passo seguinte: iniciação fora de ordem

### Dependência de saída
- Gerada pela iniciação de novas instruções (fora de ordem)
- Uma instrução depende de outra, mas a ordem de execução é alterada
- Dependendo da ordem dos escritos (writeback), pode dar problema

## Conexões
- [[Pipeline]] — base: superescalar usa 2+ pipelines em paralelo
- [[Risc X Cisc]] — mais comum em RISC; otimização hardware × software
- [[Branch desvio]] — hazard de desvio limita o ILP

## Ação

O que é um processador superescalar?::Executa múltiplas instruções independentes simultaneamente usando 2+ pipelines/unidades de execução em paralelo.
^q-wgxn

Qual a diferença entre superescalar e superpipeline?::Superescalar usa 2+ pipelines em paralelo (mais instruções por ciclo); superpipeline aprofunda os estágios de uma pipeline (clock mais alto).
^q-b4z6

O que é paralelismo no nível de instrução (ILP)?::O potencial de paralelismo de um programa — instruções independentes executadas simultaneamente, limitado por hazards e dependências.
^q-sa2m

Quais fatores limitam o paralelismo no nível de instrução?::Hazards de dados e de desvio, conflito de recurso, dependência de saída e antidependência.
^q-t85m

O que é dependência de saída?::Quando a ordem de execução das instruções é alterada e a ordem dos escritos no registrador/memória pode dar problema.
^q-iujt

O que significa iniciação em ordem com terminação fora de ordem?::Instruções entram no pipeline em ordem, mas terminam fora de ordem — melhora o desempenho com instruções de vários ciclos.
^q-42uw

Quais ordens o processador precisa conhecer na política de iniciação?::Ordem de busca, ordem de execução e ordem de atualização de registradores e memória.
^q-7gpe

---

# Referências
- Aula: OAC II - Organização e Arquitetura de Computadores