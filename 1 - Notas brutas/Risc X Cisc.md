# Risc X Cisc

Cisc → Instruções complexas que fazem vários coisas
- Tem mais poder semântico
- Diminui o trabalho do programador
- Usa múltiplos recursos de uma vez

Unidade de controle → Firmware + Hardware (Revolução!)
- P/ selecionar novas instruções → Atualiza firmware!

Criaram o RISC para otimizar, melhorar o que tenha
Evolução não é linear!

Operações do RISC:
- Atribuições
- Condicionais (IF, LOOP)
- Chamada de procedimentos em demanda

Grandez exata x o mais utilizado.

Casos comuns:
- Sem muito passagem de parâmetro
- Variáveis locais
- Sem muitos níveis de chamada
- Sintético e não recursivo

---

## Simplificações:
- Grande número de registradores
- Branch prediction
- Conjunto de instruções reduzido
- Projeto cuidadoso de pipelina

### Grande banco de registradores:
- Requer que o compilador aloque os registradores
- Alocação Most-Recently-Used
- Requer análise sofisticada do programa
- Mais registradores
- Mais variáveis aloçadas nos registradores

### Registradores p/ variáveis locais:
- Guarda variáveis locais exclusivo nos registradores
- Menos acesso à memória
- Parâmetros devem ser passados
- Resultados não retornados
- Facilita chamados de funções
- Buffer circular: a saída de um é a entrada do outro
- Solução de janelas de registradores
- Gigantesco ganho de desempenho.

### Características Risc:
- 1 instrução p/ ciclo
- Operações de registradores p/ registrador
- Poucos e simples modos de endereçamento
- "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "..." "

---

## Projeto em Hardware (Som microcódigo)
- Formato fixo de instrução
- Mais esforço/tempo de compilação

NoOp - No Operation, é tipo um stall de Software

---

A imagem mostra uma página preta com um padrão de pontos brancos distribuídos uniformemente. Não há texto ou conteúdo visível na página.