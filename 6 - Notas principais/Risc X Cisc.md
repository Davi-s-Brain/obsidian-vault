---
date: 2026-09-15
time: 11:45
tags:
  - arquitetura
  - computadores
  - risc
  - cisc
status: rascunho
source:
matéria: OAC II
aliases:
  - risc
  - cisc
  - risc vs cisc
cards-deck: OAC II
flashcards:
  q-z66w: { nid: 1789483547822, hash: uy6e2isg, sync: 3fw2ezfj }
  q-9z3t: { nid: 1789483547848, hash: 8uy4jad3, sync: u3hbf53h }
  q-68e3: { nid: 1789483547873, hash: 2v936ybz, sync: te5z3pej }
  q-ayhu: { nid: 1789483547897, hash: g8pu9wqn, sync: myfgs2su }
  q-wprs: { nid: 1789483547923, hash: fws7n829, sync: htiwvgta }
  q-76fr: { nid: 1789483547947, hash: 8nmmz3bz, sync: qqsgqmgh }
  q-ymqe: { nid: 1789483547972, hash: xtd6zwib, sync: 7ykpwi6p }
---

# Risc X Cisc

## Resumo
CISC usa instruções complexas com alto poder semântico; RISC simplifica o conjunto de instruções para otimizar o desempenho — mais registradores, operações registrador→registrador e 1 instrução por ciclo. Hoje o RISC é o mais utilizado em processadores comerciais.

## Conteúdo

### CISC
- Instruções complexas que fazem várias coisas
- Mais poder semântico → diminui o trabalho do programador
- Usa múltiplos recursos de uma vez

### Unidade de controle
- **Microprogramada (firmware + hardware):** revolução — adicionar novas instruções = atualizar o firmware, sem redesenhar o hardware
- Avanço sobre a UC hardwired → processadores mais complexos e mais baratos de desenvolver
- A separação arquitetura/organização criou famílias de computadores e reutilização de programas → viabilizou a indústria de software — ver [[Introdução]]

### Por que o RISC?
- Criado para otimizar/melhorar o que já existia — evolução não é linear
- Estudos de casos comuns mostraram: pouca passagem de parâmetros, muitas variáveis locais, poucos níveis de chamadas, programas simples e não recursivos
- Chamada de procedimentos era demorada

### Simplificações do RISC
1. Grande número de registradores
2. Branch prediction — ver [[Branch desvio]]
3. Conjunto de instruções reduzido
4. Projeto cuidadoso de pipeline — ver [[Pipeline]]

### Grande banco de registradores
- Exige que o **compilador** aloque os registradores (alocação Most-Recently-Used, com análise sofisticada do programa)
- Mais variáveis alocadas nos registradores → menos acesso à memória
- Parâmetros devem ser passados e resultados retornados, mas o custo cai
- **Janelas de registradores (buffer circular):** a saída de uma chamada é a entrada da próxima → ganho gigantesco de desempenho

### Características RISC
- 1 instrução por ciclo
- Operações registrador → registrador
- Poucos e simples modos de endereçamento
- Formatos de instrução simples

### Projeto em hardware (sem microcódigo)
- Formato fixo de instrução
- Mais esforço/tempo de compilação (o compilador faz o trabalho que o hardware não faz)
- **NoOp** (No Operation) = instrução que não faz nada, funciona como um "stall de software"

### Otimização hardware × software
- **Hardware:** mais registradores
- **Software:** compilador aloca registradores — abordagem de **coloração de grafos**; variáveis globais podem permanecer em registradores
- Pipelining RISC: instruções de registrador para registrador, em 5 estágios (IF, ID, EX, MEM, WB) — ver [[Pipeline]]
- **Hotness:** o uso de cache em tempo de execução mantém os dados "quentes" (frequentemente acessados) no cache

### Microprocessadores (contexto)
- A microeletrônica reduziu e encapsulou os componentes: mais rápidos, menores, mais eficientes energeticamente e na dissipação de calor, mais robustos a condições ambientais

## Conexões
- [[Pipeline]] — simplificação RISC: pipeline cuidadoso
- [[Branch desvio]] — branch prediction e NoOp como stall
- [[Introdução]] — arquitetura vs organização e a indústria de software
- [[Superescalares]] — evolução do RISC: múltiplas pipelines em paralelo

## Ação

O que caracteriza um processador CISC?::Instruções complexas com alto poder semântico que usam múltiplos recursos — diminuem o trabalho do programador.
^q-z66w

Qual a vantagem da unidade de controle microprogramada?::Adicionar novas instruções apenas atualizando o firmware, sem redesenhar o hardware.
^q-9z3t

Por que o RISC foi criado?::Para otimizar o que existia: simplificar o conjunto de instruções, já que programas reais usam poucas operações e chamadas de procedimento eram demoradas.
^q-68e3

Quais as 4 simplificações do RISC?::Grande número de registradores, branch prediction, conjunto reduzido de instruções e projeto cuidadoso da pipeline.
^q-ayhu

O que são janelas de registradores?::Buffer circular onde a saída de uma chamada de procedimento é a entrada da próxima — facilita chamadas de função com ganho gigantesco de desempenho.
^q-wprs

Quais as características de uma instrução RISC?::Uma instrução por ciclo, operações registrador → registrador, poucos modos de endereçamento e formatos de instrução simples.
^q-76fr

O que é NoOp?::No Operation — uma instrução que não faz nada, funcionando como um "stall de software".
^q-ymqe

---

# Referências
- Aula: OAC II - Organização e Arquitetura de Computadores