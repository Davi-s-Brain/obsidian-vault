> Nota refinada em: [[Superescalares]]

# Continuação Superescalares

## Funcionamento:
- Executa instruções independentes em diferentes pipelines
- Permite busca e execução paralela

## Tipos de ordenação:
- Ordem que as instruções são iniciadas
- Ordem que as instruções são terminadas

## Início em ordem, término fora de ordem:
- Melhora desempenho onde precisa de mais ciclos
- Complexidade de interrupções
- Dependência de saída

## Tem uma janela de instruções onde as que serão usadas ficam, ganhando desempenho.

## Renomeação de registradores:
- Técnica para resolver alguns dependências (saída/anti)

## Paralelismo de Hardware:
- Duplicação de recursos
- Início fora de ordem
- Renomeação de registradores
- Tamanho da janela de instruções > 8

## Predição de Desvio:
- Ativos de desvio não é eficiente em Superescalares
- Superescalares: Técnicas dinâmicas e estatísticas de previsão de desvio.
- Busca de múltiplas instruções ao mesmo tempo
- Só que não dependência de dados
- Inicia múltiplas instruções em paralelo
- Mecanismos para confirmar resultados da execução correta.
