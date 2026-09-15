# RISC / Superescalares

- Simplicidade do Hardware, otimizado entre Hardware e software
- Muito mais registradores
- Instruções menores e mais simples

- Compilador otimiza o uso dos registradores
  - Abordagem de coloração de grafos
  - Variáveis globais

- Otimização de Hardware: ter mais registradores
- Otimização de software: compilador mais eficiente nos alocações

## Risc Pipelining

Como ter mais desempenho? Aumentar a vazão
Instruções são de registrador para registrador
(mais aqui ao mais do pipeline) [OpenCook]

Não é todo dia que uma otimização melhora 100% do processo, como faz o pipeline.

Máquina de OAC I, não tem pipeline, não tem sobreposição
(margem: O uso de cache no tempo de execução, isso é "hotness")

---

## Superescalares

[Open Code, crie uma nova nota a partir daqui)

- Instruções usuais, load, store, desvio, são independentes e podem ser executadas simultaneamente.
- Aplicável a CISC, RISC
  (mais comum em RISC)

Todo programa tem um potencial de paralelismo.
- Paralelismo no nível de instrução.

O que é Superescalares?
- Se chama assim porque é usado sobre grandezas escalares

Superescalares X SuperPipelino (Faz uma tabela aqui)

- 2 pipelines em paralelo
- Pipeline "normal"

O computador consegue a execução de mais instruções de forma independente

---

## Paralelismo no nível de instrução
- Precisa da otimização baseada no compilador
- Técnicas de hardware limitado por:
  - Hazards de dados
  - Hazards de desvio
  - Conflito de recurso
  - Dependência de saída [Novos!]
  - Antidependência
- Execução pode ser sobreposta
- Depende da ISA.

## Paralelismo de máquina (nível 2)
- Definido pelo número de pipelines
- Dependente do mecanismo de identificação de instruções independentes
- Medida da capacidade do processador aproveitar o paralelismo no nível da instrução.

## Política de Iniciação de Instruções
- O processador precisa saber as próximas instruções
- Precisa saber a ordem delas
  - Ordem que busca
  - Ordem que executa
  - Ordem que são atualizados os registradores e memória
- Não dá pra jogar todas as instruções no pipeline, pois causa muitos Hazards.

## Iniciação em ordem com terminação fora de ordem
- Melhora desempenho com instruções de vários ciclos
- Iniciação de instruções fora de ordem

---

## Iniciação de novos instruções
- Gera dependência de saída.

### Dependência de saída
- Quando uma instrução depende de outra, mas a ordem de execução é alterada.
- Dependendo da ordem dos escritos, pode dar problema.

---

A imagem mostra uma página preta com um padrão de pontos brancos distribuídos uniformemente. Não há texto, gráficos ou outros elementos visíveis na página.