# Pipeline
- Uma forma de aumentar a performance

## Lavanderia em Pipeline
- 6 7 8 9 10 11 Meia noite
- Tempo
- 30 40 40 40 40 40 20
- ordem
- A
- B
- C
- D
- Lavanderia em Pipeline leva 3.5 horas

## Margem: Fazer isso ao invés de
## Margem: todos os dias todos os
## Margem: serviços de uma vez

- A ideia é usar recursos ociosos em tópicos diferentes e ter um ganho na execução
- Speedup: Nº de estágios
- MIPS Pipeline
- IF - Busca da instrução
- ID - Decodificação da instrução e leitura dos registradores
- EX - Executa a operação ou calcula endereços
- MEM - Acesso a operando na memória
- WB - Escreve resultado no registrador

---

## Custos do pipeline: (Hazards)

- Conflitos estruturais
  - Um recurso necessário está em uso

- Conflitos de dados
  - Instrução anterior completa leitura/escrita de dados

- Conflito de controle
  - Decisão de controle depende de instrução prévia (branch)

---

## Solução do conflito de dados
- Encaminhamento (forwarding)
  - Jogar o resultado de uma CLA adentro de outra
  - Nem sempre resolve, se o valor não tá pronto na hora.

---

## Solução do conflito de controle
- Adiantar a resolução e fazer o fetch da instrução
- Previsão de desvio
  - Para da se previsão estiver errada!
  - Só perco caso eu use a previsão

---

A imagem mostra uma página preta com um padrão de pontos brancos distribuídos uniformemente. Não há texto, gráficos ou outros elementos visíveis na página.