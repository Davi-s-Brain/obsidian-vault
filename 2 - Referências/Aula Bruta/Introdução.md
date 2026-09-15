# Introdução

> Nota refinada em: [[Introdução]]

- Máquina base: 1 núcleo, busca e executa instruções

## Processador

- Busca, executa e processa dados e instruções
- Trabalho: Prova
- Pendente: revisar Organização do processador e Unidade de controle

## Organização

- Interconexão dos componentes com certas instruções

## Arquitetura

- Conjunto de instruções
- Atributos visíveis ao programador
- Coisa de baixo nível

Programa -> Sequência de instruções

Ciclo de instrução

---

## Busca instrução → Decodifica → Busca operandos → Executa → Guarda

Program Counter (PC) → Endereço das instruções

Ciclo de busca ocupa quando a instrução chega no IR (Instruction Register)

Tempo de ciclo: quanto tempo por ciclo, quanto maior, mais lento

Desempenho = Tempo de execução

Muita coisa influencia

Armazenamento Comum: pensar que só 1 elemento influencia o desempenho.

Como saber quem tem um melhor desempenho?
CPI x Tempo

Monta:
Foco o caso comum mais rápido!

Lei de Amdahl:
Lei que quantifica otimizações

Speedup = T_execução do tarefa sem melhora / T_execução do tarefa com melhora

Lo ganho = Tem que ser maior que 1, aí a mudança é válida.

---

## Speedup

Speedup = 1 / (1 - Fraction_enhanced) + Fraction_enhanced / Speed_up_enhanced

> ✏️ Fórmula corrigida na nota refinada: Speedup = 1 / ((1 − F) + F / S) — o denominador é inteiro; exemplo correto dá 1,56 (não 1,66).

### Exemplo de Amdahl

Considera uma melhoria que roda 10 vezes mais rápido do que a máquina original, mas só pode ser utilizada 40% do tempo. Qual o Speedup?
Fraction_en = 0,4 e Speedup_en = 10

Speedup_overall = 1 / (1 - 0,40) + 0,40 / 10 = 1,66

## Desempenho

O desempenho é específico a um programa, configurações, meio que tudo.
Taxa_execução = NInstruções x CPI x Velocidade

Como otimizar a máquina (a luta)?

### Prefetch

- Enquanto executa uma instrução, antecipa a busca da próxima
- "Instruction prefetch"

### Pipeline

Não é uma máquina paralela
Busca -> Executa -> Busca -> Executa : Lento
Busca -> Executa -> Executa : Rápido

---

Busca

Em condições ideais, DOBRA o número de instruções executadas!

No mundo real, o tempo de execução é maior que o de busca. Ainda tem um monte de pontos etapas a serem feitos.

### Múltiplos estágios:
- Busca instruções
- Decodificação
- Calcula operandos
- Busca operandos
- Executa instrução
- Escreve resultado