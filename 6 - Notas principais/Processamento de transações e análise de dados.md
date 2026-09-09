---
date: "2026-09-09"
time: "09:01"
tags: []
status: rascunho
source: 
aliases: []
---

## Processamento de transações e análise de dados

## Conteúdo

_OLTP_: Online Transaction Processing. 
- Processamento de transações on-line. Refere-se aos sistemas otimizados para lidar com um grande volume de transações em tempo real (inserções, atualizações e consultas simples e frequentes). Prioriza rapidez de escrita, integridade e consistência dos dados. Exemplos: sistemas bancários, e-commerce, reservas de passagens, sistemas de folha de pagamento.

_OLAP_: Online Analytical Processing
- Processamento analítico on-line. Refere-se aos sistemas otimizados para a análise de grandes volumes de dados históricos e consolidados, executando consultas complexas (agregações, drill-down, corte por dimensões) para apoio à tomada de decisão. Prioriza leitura e análise multidimensional. Exemplos: data warehouses, data marts, relatórios de business intelligence (BI).

### Comparação OLTP × OLAP

| Critério | OLTP | OLAP |
| --- | --- | --- |
| Finalidade | Transações operacionais do dia a dia | Análise de dados para tomada de decisão |
| Carga de trabalho | Muitas operações curtas (leitura/escrita) | Consultas complexas sobre grandes volumes |
| Tipo de dados | Dados atuais e detalhados (transações individuais) | Dados históricos e consolidados (agregados) |
| Modelagem | Altamente normalizado (evita redundância) | Desnormalizado (modelo estrela, floco de neve) |
| Tempo de resposta | Milissegundos a segundos | Segundos a minutos |
| Nº de usuários | Muitos usuários operacionais simultâneos | Menos usuários analíticos (analistas, gestores) |
| Objetivo principal | Integridade, consistência e velocidade de escrita | Velocidade de leitura e flexibilidade de análise |

### Propriedades de cada um

**OLTP — propriedades ACID:**
- **Atomicidade** (Atomicity): cada transação é executada por completo ou não é executada (tudo ou nada).
- **Consistência** (Consistency): as transações levam o banco de um estado válido a outro, respeitando regras de integridade.
- **Isolamento** (Isolation): transações concorrentes não interferem umas nas outras.
- **Durabilidade** (Durability): uma vez confirmada, a transação persiste mesmo após falhas ou quedas do sistema.

**OLAP — acrônimo FASMI:**
- **Fast** (Rápido): respostas rápidas para consultas, mesmo sobre grandes volumes.
- **Analysis** (Análise): suporte a análises e cálculos de negócio complexos.
- **Shared** (Compartilhado): múltiplos usuários acessando os dados simultaneamente.
- **Multidimensional** (Multidimensional): visão dos dados por várias dimensões (tempo, região, produto etc.).
- **Information** (Informação): acesso a dados históricos e consolidados de toda a organização.

## Conexões
<!-- Que outras notas se conectam aqui? Use [[]] -->

