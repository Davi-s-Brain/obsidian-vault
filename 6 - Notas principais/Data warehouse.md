---
date: "2026-09-09"
time: "09:17"
tags: []
status: rascunho
source: 
aliases: []
---

# Data warehouse
---
## Conteúdo

_Definição:_ Um data warehouse é um sistema de banco de dados voltado para análise e elaboração de relatórios, projetado para armazenar grandes volumes de dados históricos e consolidados originados de diversas fontes (bancos transacionais, arquivos, APIs etc.). Os dados passam por um processo de extração, transformação e carga (ETL), são organizados de forma desnormalizada — normalmente em modelos estrela ou floco de neve (snowflake) — e indexados para consultas analíticas (OLAP).

_Por que é usada:_ 
- Separa a carga de trabalho analítica da operacional: consultas pesadas de análise não competem com as transações do dia a dia dos sistemas OLTP.
- Consolida dados de múltiplas fontes em uma única visão confiável e padronizada da organização.
- Melhora o desempenho das consultas: dados desnormalizados, agregados e pré-indexados.
- Preserva o histórico de dados ao longo do tempo, permitindo análises de tendências e comparações de períodos.
- Dá suporte à tomada de decisão e a ferramentas de business intelligence (BI).

_Exemplos:_
- Análise de vendas por região, produto e período.
- Dashboards e relatórios gerenciais em ferramentas como Power BI, Tableau e Looker.
- Segmentação de clientes e análise de comportamento para campanhas de marketing.
- Análises financeiras consolidadas (demonstrativos, custos, receitas).

## Conexões

