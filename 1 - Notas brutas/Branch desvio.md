# Branch desvio

- Desvios impedem a pipeline de encher e causa penalidade de performance
- O desvio otimiza porque a unidade de controle sempre aposta no caso comum de que a execução continua e a próxima instrução

## Contos de uma máquina c/ pipeline:
- É mais complexo, principalmente na lógica de controle de estágios e dependências
- Tem penalidades em custos na movimentação de dados entre a memória e o processador

- Desempenho: ganho = T1 = N.K.t = N.K
  T1 = [K1(n-1)].t = K1(n-1)

- Número típico de estágios é de 6 a 9

## Abordagem p/ mitigação de contação de desvio
- Múltiplos fluxos: Usar dois pipelines (mas só 1 executa os instruções por vez) um buscando as próximas instruções de um rumo e o outro de outro
  - Essa abordagem causa muitos conflitos no uso dos registradores, lançamentos e isso precisa ser coordenado pela unidade de controle
  - Aumenta mais ainda a complexidade
  - Caso hajam 2 ou mais desvios sequenciais, voltamos a ter penalidade

- Busca antecipada da instrução alvo do desvio: Deixar a instrução alvo já carregada, menos com. Caso o desvio seja tomado, podemos executá-la imediatamente

---

## Memória em loop de repetição
- Para loops, ao invés de começar lendo as instruções, olhamos se elas já estão no cache do processador.
  - Funciona por localidade de referência
  - Bom p/ loops ou pequenos jumps.

## Buffers
- Temporário e geralmente os dados só existem nele durante uma transferência.

## Caching
- Cópia de dados, de uma memória mais lenta, para uma mais rápida.

## Previsão de acessos
- Prevermos o resultado da avaliação como, nunca ocorrerá (sempre carrega a próxima) e sempre ocorrerá (sempre carrega a instrução olho.

---

### RISC & CISC

- A depuração de arquitetura e organização cria famílias de computadores e permite a reutilização de código/programas entre máquinas.
  - Viabiliza a indústria de software

- Unidade de controle microprogramada (um avanço sobre a UC hardwired) permite que processadores mais complexos e mais caros para desenvolver mais rápido.

- Memória cache que "acelera" a comunicação memória-processador para garantir menos tempo ocioso.

---

Microprocessadores: Avanços em microeletrônica permitiram reduzir o tamanho dos componentes e encapsulá-los, tornando-os mais rápidos, menores, mais eficientes energeticamente, mais eficientes na dissipação de calor, mais robustos a condições ambientais (não sai sujeira).

RISC: (Reduced Instruction Set Computer)

---

A imagem mostra uma página preta com um padrão de pontos brancos distribuídos uniformemente. Não há texto, gráficos ou outros elementos visíveis na página.