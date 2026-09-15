# Risc × Cisc

> Nota refinada em: [[Risc X Cisc]]

## Cisc
- Instruções complexas que podem vários coisas
- Tem mais poder semântico
- Diminui o trabalho do programador
- Usa múltiplos recursos de uma vez

## Unidade de controle
- Firmware + Hardware (Revolução!)
- P/ adicionar novas instruções atualizar firmware!

## Criaram o Risc para otimizar, melhorar o que tinha
## Evolução não é linear!
## Operações do Risc:
- Atribuições
- Condicionais (IF, Loop)
- Chamada de procedimentos era demorada
## Grandeza escolar é o mais utilizado.

## Casos comuns:
- Sem muita passagem de parâmetros
- Variáveis locais
- Sem muitos níveis de chamadas
- Simples e não recursivo

---

Simplificação:
- Grande número de registradores
- Branch prediction
- Conjunto de instruções reduzido
- Projeto cuidadoso de pipeline

Grande banco de registradores:
- Requer que o compilador aloque os registradores
- Alocação Most-Recently-Used
- Requer análise sofisticada do programa
- Mais registradores
- Mais variáveis aloçadas nos registradores

Registradores p/ variáveis locais:
- Quando variáveis locais escolhão nos registradores
- Menos acesso à memória
- Parâmetros devem ser passados
- Resultados não retornados
- Facilita chamados de funções
- Buffer circular: a saída de um é a entrada do outro
- Solução de janelas de registradores
- Gigantesco ganho de desempenho

Características Risc:
- 1 instrução p/ ciclo
- Operações de registradores p/ registrador
- Pouco e simples modos de endereçamento
- Formatos de instrução

---

## Projeto em Hardware (Sem microcódigo)
- Formato fixo de instrução
- Mais esforço/tempo de compilação

NoOp = No Operation, é tipo um stall de Software

---

> Seção RISC & CISC da aula de [[Branch desvio]] (famílias de computadores, UC microprogramada, cache, microprocessadores) também incorporada na nota refinada [[Risc X Cisc]].