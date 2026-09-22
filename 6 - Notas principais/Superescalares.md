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
- Introduz complexidade no tratamento de interrupções e dependências de saída
- Próximo passo: iniciação fora de ordem

### Paralelismo de Hardware
Para maximizar o ILP, o hardware utiliza:
- **Duplicação de recursos:** Mais unidades funcionais para evitar conflitos.
- **Iniciação fora de ordem:** Executa instruções assim que os operandos estão disponíveis.
- **Renomeação de registradores:** Resolve dependências de saída (WAW) e anti-dependências (WAR).
- **Janela de instruções:** Buffer (geralmente > 8 instruções) que permite ao processador buscar instruções independentes adiante no fluxo.

### Predição de Desvio em Superescalares
Devido ao alto custo de esvaziar múltiplas pipelines:
- Técnicas estáticas são ineficientes; utilizam-se **técnicas dinâmicas e estatísticas**.
- Busca de múltiplas instruções simultaneamente para alimentar as pipelines.
- Implementação de mecanismos de confirmação (commit) para garantir que os resultados sejam aplicados na ordem correta, mesmo executados fora de ordem.

## Conexões
- [[Pipeline]] — base: superescalar usa 2+ pipelines em paralelo
- [[Risc X Cisc]] — mais comum em RISC; otimização hardware × software
- [[Branch desvio]] — hazard de desvio limita o ILP
- [[Superescalares#Paralelismo de Hardware]] — aprofundamento em janelas e renomeação

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

Para que serve a renomeação de registradores em superescalares?::Técnica para resolver dependências de saída (WAW) e anti-dependências (WAR) ao mapear registradores lógicos para físicos.

Como a janela de instruções melhora o desempenho?::Permite que o processador busque instruções independentes mais adiante no código, aumentando a probabilidade de preencher as pipelines.

Por que a predição de desvio dinâmica é essencial em superescalares?::Porque o custo de esvaziar múltiplas pipelines em um erro de predição é muito maior do que em uma pipeline única.

---

# Referências
- Aula: OAC II - Organização e Arquitetura de Computadores