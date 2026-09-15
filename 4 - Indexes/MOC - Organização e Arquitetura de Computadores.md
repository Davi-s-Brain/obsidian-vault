---
tags:
  - index
  - moc
date: 2026-09-10
atualizado: 2026-09-15
matéria: OAC II
cards-deck: OAC II
---

# Organização e Arquitetura de Computadores — OAC II

> [!info] MOC
> Mapa de estudo da matéria. Cada nota é um tópico atômico com conteúdo, flashcards (baralho `OAC II`) e links internos.

---

## Fluxo de estudo

Siga a ordem para construir o conhecimento incrementalmente. Cada nota aponta para a que veio antes e a que vem depois.

| #   | Tópico            | Resumo                                                              | Cards                           |     |
| --- | ----------------- | ------------------------------------------------------------------- | ------------------------------- | --- |
| 1   | [[Introdução]]    | Arquitetura vs organização, ciclo de instrução, Lei de Amdahl       | [[Introdução#Ação\|7 cards]]    |     |
| 2   | [[Pipeline]]      | Sobreposição de estágios, hazards e soluções (forwarding, previsão) | [[Pipeline#Ação\|7 cards]]      |     |
| 3   | [[Branch desvio]] | Hazard de controle aprofundado: mitigações, loop buffer, previsão   | [[Branch desvio#Ação\|7 cards]] |     |
| 4   | [[Risc X Cisc]]   | CISC vs RISC: janelas de registradores, hardware puro, NoOp         | [[Risc X Cisc#Ação\|7 cards]]   |     |
| 5   | [[Superescalares]] | Execução paralela de instruções independentes (ILP)               | [[Superescalares#Ação\|7 cards]] |     |

---

## Mapa conceitual

```
Introdução
├── Ciclo de instrução / desempenho
└── Pipeline
    ├── Hazards estruturais
    ├── Hazards de dados → Forwarding
    └── Hazards de controle → Branch desvio
                            ├── Múltiplos fluxos
                            ├── Busca antecipada
                            ├── Previsão (nunca/sempre)
                            └── Loop buffer
└── Risc X Cisc
    ├── CISC → UC microprogramada
    └── RISC → registradores, pipeline, NoOp
        └── Superescalares
            ├── 2+ pipelines em paralelo
            ├── ILP (paralelismo no nível de instrução)
            ├── Dependência de saída / antidependência
            └── Política de iniciação (ordem/f.d.ordem)
```

---

## Conexões entre notas

- [[Introdução]] introduz o ciclo de instrução → [[Pipeline]] sobrepe os estágios
- [[Pipeline]] define o hazard de controle → [[Branch desvio]] detalha mitigações
- [[Pipeline]] usa branch prediction como solução → [[Branch desvio]] é o aprofundamento
- [[Risc X Cisc]] assume pipeline e branch prediction como pressupostos → [[Pipeline]] e [[Branch desvio]] são pré-requisitos
- [[Risc X Cisc]] contrasta com o conceito de arquitetura/organização de [[Introdução]]
- [[Superescalares]] estende [[Pipeline]] (2+ pipelines) e [[Risc X Cisc]] (mais comum em RISC), limitado pelos hazards de [[Branch desvio]]

---

## Aulas-fonte (arquivo bruto)

> [!example] Origens das notas — rascunhos preservados em `2 - Referências/Aula Bruta/`

| Aula | Nota bruta | Nota principal |
|------|-----------|----------------|
| AULA 1 | [[2 - Referências/Aula Bruta/Introdução\|Introdução]] | [[Introdução]] |
| AULA 2 | [[2 - Referências/Aula Bruta/Branch desvio\|Branch desvio]] | [[Branch desvio]] |
| AULA 3 | [[2 - Referências/Aula Bruta/Risc X Cisc\|Risc X Cisc]] | [[Risc X Cisc]] |
| Pipeline | [[2 - Referências/Aula Bruta/Pipeline\|Pipeline]] | [[Pipeline]] |
| AULA 4 | [[2 - Referências/Aula Bruta/RISC Superescalares\|RISC Superescalares]] | [[Risc X Cisc]] + [[Superescalares]] |

---

## Pendências

- [ ] Criar nota [[Cache e invalidação de hardware]] —_loop buffer e cache de instruções (conceito de hardware vs o Redis da nota existente)
- [ ] Revisar `[[Introdução]]`: pendência de estudo "organização do processador e unidade de controle"
- [ ] Sincronizar cards do baralho OAC II (35 cards: 5 × 7)