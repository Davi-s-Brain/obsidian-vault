---
date: 2026-09-08
time: 21:26
tags:
  - DDIA
  - tecnologia
status: concluído
source:
aliases: []
---

# Sistemas com uso intensivo de dados

## Conteúdo

_Definição:_ Dizemos que um sistema que faz o uso intensivo de dados quando o principal desafio em seu desenvolvimento é o gerenciamento de dados. 
Ex: 
- O programa precisa paralelizar um cálculo numérico muito grande;
- Se preocupa com aspectos como armazenar e processar grandes volumes de dados;
- Garantir consistência diante de falhas e concorrência;
- Assegurar que os serviços se mantenham no ar mesmo com falhas.

Concomitantemente a isso, diversas aplicações precisam:
- Armazenar dados para serem recuperados rapidamente posteriormente
- Lembrar de uma operação custosa para acelerar as leituras (_caches_)
- Permitir que usuários pesquisem por dados por palavra-chave (_índices de busca_)
- Tratar eventos e mudanças nos dados assim que ocorrem (_processamento de fluxos_)
- Processar periodicamente grandes volumes de dados acumulados (_Aplicações de batch_)

---
# Referências
- [[Conceitos-chave]]