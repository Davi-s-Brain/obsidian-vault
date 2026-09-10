# Introdução

- Máquina base: 1 núcleo, busca e executa instruções

## Processador

- Busca, executa e processa dados e instruções
- Trabalho: Prova
- Memória: Dados, Instruções

## Média final: 0,3 * MT + 0,7 * MR

5 no mínimo nos dois

## Revisão organização do processador e unidade de controle

## Organização

- Interconexão dos componentes com certas instruções

## Arquitetura

- Conjunto de instruções
- Atributos visíveis ao programador
- Coisa de baixo nível

Programa -> Sequência de instruções

Ciclo de instrução

---

## Busca instrução -> Decodifica -> Busca operando -> Executa -> Guarda

Program Counter (PC) -> Endereço da instrução

Ciclo de busca acaba quando chega quando a instrução chega no IR (Instruction Register)

Tempo do ciclo: quanto tempo por ciclo, quanto maior, mais lento

Desempenho = Tempo de execução

Muita coisa influencia

Armadilha comum: pensar que só 1 elemento influencia o desempenho.

Como saber quem tem um melhor desempenho?
CPI x Tempo

Mentira: Faça o caso comum mais rápido!

Lei de Amdahl:
- Lei que quantifica otimizações

Speedup = T execução do tarefa sem melhoria
T execução do tarefa com melhoria

Lo ganho -> Tem que ser maior que 1, aí a mudança é válida.

---

## Speedup = 1 / (C - Fraction_enhanced) + Fraction_enhanced / Speedup_enhanced

### Exemplo de Amdahl
Considera uma melhoria que roda 10 vezes mais rápido do que a máquina original, mas só pode ser utilizada 40% do tempo. Qual o Speedup?
Fraction_en = 0,4 e Speedup_en = 10

Speedup_overall = 1 / (1 - 0,40) + 0,40 / 10 = 1,66

## 1 / (1 - 0,2) + 0,2 / 20 = 5,25
## 1 / (1 - 0,5) + 0,5 / 2 = 1,33

O desempenho é específico a um programa, configurações, meio que tudo.
Taxa_execução = NInstruções x CPI x Veloc.
Como otimizar a máquina (a lata)?
Prefetch
- Enquanto executa uma instrução, antecipa a busca da próxima
- "Instruction prefetch"

Pipeline não é uma máquina paralela
Busca -> Executa -> Busca -> Executa : Lento
Busca -> Executa -> Executa : Rápido

---

Busca

Em condições ideais, DOBRA o número de instruções executadas!

No mundo real, o tempo de execução é maior que o de busca. Ainda tem um monte de outros etapas a serem feitos.

Multiplos estágios:
- Busca instrução
- Decodificação
- Calcula operandos
- Busca operandos
- Executa instrução
- Escreve resultado

[imagem: Tabela com colunas de 1 a 14 e linhas de Instrução 1 a Instrução 9. Cada linha tem setas indicando o fluxo de etapas: F1, DI, CO, FO, EI, WO.]

---

A imagem mostra uma página preta com um padrão de pontos brancos distribuídos uniformemente. Não há texto, gráficos, ou qualquer outro conteúdo visível na página.