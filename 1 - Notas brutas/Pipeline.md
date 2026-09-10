# Pipeline
- Uma forma de aumentar a performance

## Lavanderia em Pipeline
- 6 7 8 9 10 11 Meia noite
- Tempo
- Lavanderia em Pipeline leva 3.5 horas
- 1998 Morgan Kaufmann Publishers 10

## A ideia é usar recursos ociosos em tarefas diferentes e ter um ganho no execução
- Speedup: N° de estágios
- MIPS Pipeline
- IF - Busca da instrução
- ID - Decodificação da instrução e leitura dos registradores
- EX - Executa a operação ou calcula endereços
- MEM - Acesso a operando na memória
- WB - Escreve resultado no registrador

---

## Custos do pipeline: (Hazards)

- Conflitos estruturais
  * Um recurso necessário está em uso
- Conflitos de dados
  * Instrução anterior completa leitura/escrita de dados
- Conflito de controle
  * Decisão de controle depende de instrução prévia (branch)

---

## Solução do conflito de dados
- Encaminhamento (forwarding)
  * Jogar o resultado de uma CLA dentro de outra
  * Nem sempre resolve, se o valor não tá pronto na hora.

---

## Solução do conflito de controle
- Adiantar a resolução e fazer o fetch da instrução
- Previsão de branch
  * Parada momentânea se precisão estiver errada!
  * Só perco caso eu erre a previsão

---

## Página 3

[margem: Observação: Verificar dados]