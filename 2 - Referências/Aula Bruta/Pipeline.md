# Pipeline

> Nota refinada em: [[Pipeline]]

Técnica que aumenta a performance sobrepondo a execução de múltiplas instruções em diferentes estágios.

**Analogia (Lavanderia):** Em vez de esperar terminar todo o serviço para começar o próximo, cada etapa (lavar, secar, dobrar) começa assim que o item anterior avança para a próxima estação. O ganho de tempo vem da **execução em paralelo**.

**Speedup:** Ganho de desempenho proporcional ao número de estágios da pipeline.

## Estágios MIPS (5 estágios)

1. **IF** (Instruction Fetch) - Busca a instrução na memória
2. **ID** (Instruction Decode) - Decodifica e lê registradores
3. **EX** (Execute) - Executa operação ou calcula endereço
4. **MEM** (Memory Access) - Acessa operando na memória
5. **WB** (Writeback) - Escreve resultado no registrador

## Hazards (Conflitos)

Problemas que impedem a execução paralela ideal:

- **Estrutural** - Dois estágios disputam o mesmo recurso de hardware
- **Dados** - Instrução depende do resultado de outra ainda não completada
- **Controle** - Decisão de um branch afeta instruções seguintes ainda não executadas

## Soluções

**Hazards de dados:**
- **Forwarding** (encaminhamento) - Redireciona o resultado da ULA diretamente para onde é necessário, sem esperar o WB. Nem sempre resolve se o valor não estiver pronto a tempo.

**Hazards de controle:**
- **Adiantamento** da resolução - Executa o fetch da próxima instrução antes da decisão final
- **Previsão de desvio** - Supõe o caminho do branch; se errar, descarta instruções e refaz