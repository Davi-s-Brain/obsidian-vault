---
date: 2026-09-13
time: 19:42
tags:
  - arquitetura
  - computadores
  - pipeline
  - risc-v
status: rascunho
source:
matéria: OAC II
aliases:
  - pipeline
  - hazards
  - forwarding
cards-deck: OAC II
flashcards:
  q-c5ye: { nid: 1789478118923, hash: 9ev5gxrr, sync: 3kkvyjjh }
  q-zwm4: { nid: 1789478118947, hash: svj6uaaj, sync: 5vwgcet9 }
  q-teg7: { nid: 1789478118972, hash: bv5emmfu, sync: w2ihp278 }
  q-bvtm: { nid: 1789478118997, hash: qqynxr7s, sync: nbxq4t8s }
  q-8nb8: { nid: 1789478119022, hash: h49mnjs7, sync: gdw6n2uw }
  q-u2qp: { nid: 1789478119047, hash: 7rpma6h4, sync: 5x2rcn94 }
  q-2yci: { nid: 1789478119072, hash: 2qgx7cwx, sync: j87x6ncg }
  q-t4xv: { nid: 1789478119097, hash: enjcn58v, sync: exddrphb }
  q-vjhr: { nid: 1789478119122, hash: xng6mxdr, sync: 4n7zrck8 }
  q-cgy5: { nid: 1789478119147, hash: jkvdp2yu, sync: uyi8i4ae }
---

# Pipeline

## Resumo
Pipeline é uma técnica que aumenta a performance do processador sobrepondo a execução de múltiplas instruções em diferentes estágios. O desafio são os **hazards** (conflitos estruturais, de dados e de controle) que interrompem a execução paralela ideal, resolvidos com técnicas como **forwarding** e **previsão de desvio**.

## Conteúdo

### Conceito
- Técnica de execução sobreposta: cada instrução está em um estágio diferente simultaneamente
- Ganho de desempenho proporcional ao número de estágios

**Analogia (Lavanderia):** Em vez de esperar terminar todo o serviço para começar o próximo, cada etapa (lavar, secar, dobrar) começa assim que o item anterior avança para a próxima estação. O ganho de tempo vem da ==execução em paralelo==.
^q-c5ye

### Estágios MIPS (5 estágios)

| Estágio | Sigla | Função |
|---------|-------|--------|
| Busca | IF | Busca instrução na memória |
| Decodificação | ID | Decodifica e lê registradores |
| Execução | EX | Executa operação ou calcula endereço |
| Memória | MEM | Acessa operando na memória |
| Writeback | WB | Escreve resultado no registrador |

### Hazards (Conflitos)

Problemas que impedem a execução paralela ideal:

- **Estrutural** - Dois estágios disputam o mesmo recurso de hardware
- **Dados** - Instrução depende do resultado de outra ainda não completada
- **Controle** - Decisão de um branch afeta instruções seguintes ainda não executadas

### Soluções

**Hazards de dados:**
- ==Forwarding== (encaminhamento) - Redireciona o resultado da ULA diretamente para onde é necessário, sem esperar o WB. Nem sempre resolve se o valor não estiver pronto a tempo.
^q-zwm4

**Hazards de controle:**
- ==Adiantamento== da resolução - Executa o fetch da próxima instrução antes da decisão final
- ==Previsão de desvio== - Supõe o caminho do branch; se errar, descarta instruções e refaz
^q-teg7

## Conexões
- [[Cache e invalidação]] - conceito relacionado de otimização de desempenho
- [[Escalabilidade]] - pipeline como técnica de escalabilidade em hardware
- [[Conceitos-chave]] - lista de conceitos fundamentais de arquitetura

## Ação

Qual o objetivo principal de uma pipeline de processador?::Sobrepor a execução de múltiplas instruções em diferentes estágios para aumentar a performance.
^q-bvtm

Quais são os 5 estágios de uma pipeline MIPS?::IF (busca), ID (decodificação), EX (execução), MEM (acesso à memória), WB (writeback).
^q-8nb8

O que é um hazard estrutural?::Conflito onde dois estágios disputam o mesmo recurso de hardware ao mesmo tempo.
^q-u2qp

O que é um hazard de dados?::Uma instrução depende do resultado de outra que ainda não completou sua execução.
^q-2yci

O que é hazard de controle e qual instrução o causa?::Conflito causado por um branch cuja decisão afeta instruções seguintes ainda não executadas.
^q-t4xv

O que é forwarding e qual sua limitação?::Técnica que redireciona o resultado da ULA diretamente para estágios que precisam dele; sua limitação é quando o valor não está pronto a tempo.
^q-vjhr

Qual a ideia da previsão de desvio?::Supor o caminho correto do branch para executar instruções adiantadamente; se errar, descarta e refaz.
^q-cgy5

---

# Referências
- Aula: OAC II - Organização e Arquitetura de Computadores
