---
date: 2026-09-09
time: 09:18
tags:
  - tecnologia
  - DDIA
status: rascunho
source:
aliases: []
---

# Data Lake

## Conteúdo
---

_Definição:_ Um data lake é um repositório centralizado que armazena grandes volumes de dados em seu formato bruto e nativo — estruturados, semiestruturados e não estruturados — a um custo de armazenamento baixo. Diferente do data warehouse, os dados não são transformados nem modelados na ingestão: o esquema é aplicado apenas no momento da leitura (_schema-on-read_), o que dá máxima flexibilidade para usos futuros.

_Por que é usada:_ 
- Armazena qualquer tipo de dado sem modelagem prévia: logs, imagens, vídeos, documentos, JSON, dados de sensores etc.
- Custo reduzido de armazenamento (em geral, objetos em nuvem, como Amazon S3).
- Flexibilidade para descobrir novos usos e análises depois que os dados já estão armazenados.
- Base para ciência de dados, machine learning e análises exploratórias sobre dados variados.
- Ingestão rápida e contínua de grandes volumes de dados brutos.

_Exemplos:_
- Logs de aplicação e eventos de telemetria (cliques, dados de sensores IoT).
- Dados brutos de APIs e redes sociais, retidos para análises futuras.
- Arquivos de mídia (imagens, vídeos, áudio) e documentos.
- Pipelines de big data (Spark, Flink) para treinamento de modelos de ML.

### Diferenças: Data Lake × Data Warehouse

| Critério | Data Warehouse | Data Lake |
| --- | --- | --- |
| Tipo de dados | Estruturados, curados e prontos para análise | Brutos, em formato nativo (estruturados ou não) |
| Esquema | Definido na escrita (_schema-on-write_) | Aplicado na leitura (_schema-on-read_) |
| Processamento | ETL (extrai, transforma, carrega) | ELT (carrega e transforma quando necessário) |
| Modelagem | Desnormalizada (estrela, floco de neve) | Sem modelagem prévia |
| Custo | Alto (armazenamento + processamento) | Baixo (armazenamento barato) |
| Usuários | Analistas, BI, gestores | Cientistas de dados, engenheiros de dados |
| Governança | Alta: dados confiáveis e padronizados | Depende de governança; sem ela vira um "pântano de dados" |
| Finalidade | Relatórios e BI estruturados | Exploração, analytics avançado e machine learning |

### Vantagens
- **Flexibilidade:** aceita qualquer formato de dado, sem definição prévia de esquema.
- **Custo baixo:** armazenamento barato em comparação com o data warehouse.
- **Escala:** armazena volumes massivos de dados (petabytes).
- **Velocidade de ingestão:** dados são carregados rapidamente, sem transformação na entrada.
- **Usos futuros:** habilita data science, ML e análises que ainda não foram imaginadas.

### Desvantagens
- **Risco de "data swamp":** sem governança e catalogação, torna-se um depósito caótico e difícil de navegar.
- **Menor confiabilidade:** dados brutos podem ter baixa qualidade, duplicações ou inconsistências.
- **Dificuldade de integração:** dados de fontes diferentes não são padronizados nem relacionados.
- **Segurança e privacidade:** proteção e controle de acesso são mais difíceis de aplicar sobre dados heterogêneos.
- **Performance:** consultas tradicionais de BI são mais lentas sem transformação e indexação prévias.

## Conexões
- [[Data warehouse]]

