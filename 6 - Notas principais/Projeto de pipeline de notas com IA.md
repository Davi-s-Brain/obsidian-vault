---
date: 2026-09-09
time: 21:27
tags:
  - projetos
  - tecnologia
status: rascunho
source:
aliases: []
---

# Projeto de pipeline de notas com IA

## Resumo
Este projeto consiste basicamente numa pipeline que pega um arquivo PDF escrito à mão e o integra automaticamente ao Obsidian, de forma bruta (num primeiro momento).

## Conteúdo
---

### Lista de passos da pipeline

- Criar a nota manualemente no Samsung Notes
- Enviar o PDF para uma pasta específica no computador
- Usar um script em Python, rodando em background, para monitorar a pasta do PDF.
- Usar uma IA multimodal, disparada pelo script python, para analisar e transcrever o PDF, gerendo um Markdown
- Mover o Markdown gerado para uma nova pasta do Obsidian
- A pasta será usada para refinar a nota e integrá-la ao grafo de conhecimento 

## Conexões
[[Projetos]]

## Ação
---
- [x] Criar uma nota no Samsung notes e compartilhar automaticamente com o PC
- [ ] Criar um script em python que pega automaticamente as notas que foram adicionadas, lê as mesmas, e gera um arquivo markdown, adicionando eles na pasta do obsidian


# Referências
