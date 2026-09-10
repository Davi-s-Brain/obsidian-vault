# Branch desvio

- Desvios impedem a pipeline de encher e causa penalidade de performance
- O desvio otimiza porque a unidade de controle sempre aponta no caso comum de que a execução continua e a próxima instrução

## Contos de uma máquina C/ pipeline:
- É mais complexo, principalmente na lógica de controle de estágios e dependência
- Tem penalidades em custos na movimentação de dados entre a memória e o processador

- Desempenho: ganho = Ts = N.K.t = N.K
  [Ki(n-1)]·t Ki(n-1)

- Número típico de estágios é de 6 a 9.

## Alordagem p/ mitigação de contação do desvio
- Múltiplos fluxos: Usar dois pipelines (mas só executa as instruções por vez) um buscando as próximas instruções de um rumo e o outro de outro
- Essa alordagem causa muitos conflitos no uso dos registradores, lançamentos e isso precisa ser coordenado pela unidade de controle
- Aumenta mais ainda a complexidade
- Caso hajam 2 ou mais desvios seguidos, voltamos a ter penalidade

- Busca antecipada da instrução alvo do desvio: Deixam a instrução alvo já concedida, mesmo com. Caso o desvio seja tomado, podemos executá-la imediatamente

---

## Memória em loop de repetição
- Para loops, ao invés de começar buscando as instruções, olhamos se elas já estão no cache do processador.
  - Função na localidade de referência
  - Bom p/ loops ou pequenos jumps.

## Buffer
- É temporário e geralmente os dados só existem nel durante uma transferência.

## Coching
- Cópia de dados, de uma memória mais lenta, para uma mais rápida.

## Previsão de desvio
- Preservamos o resultado da análise como, nunca ocorrerá (sempre carrega a próxima) e sempre ocorrerá (sempre carrega a instrução alvo).

---

### RISC & CISC
- A depuração de arquitetura e organização cria famílias de computadores e permite a reutilização de código/programas entre máquinas. (Vitaliza a indústria de software)
- Unidade de controle microprogramada (um avanço sobre a UC tradicional) permite criar processadores mais complexos e mais rápidos para desenvolver mais rápido.
- Memória cache que "acelera" a comunicação memória-processador para garantir menos tempo ocioso.

---

- Microprocessadores: Avanços em microeletrônica permitiram reduzir o tamanho dos componentes e encapsulá-los, tornando-os mais rápidos, menores, mais eficientes energeticamente, mais eficientes na dissipação de calor, mais robustos a condições ambientais (não caí- raízera)
- RISC: (Reduced Instruction Set Computer)

---

A imagem mostra uma página preta com um padrão de pontos brancos distribuídos uniformemente. Não há texto ou conteúdo visível na página.