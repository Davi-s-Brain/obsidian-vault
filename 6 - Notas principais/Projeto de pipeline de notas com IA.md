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

## Implementação

### Stack
- **Python 3 (stdlib pura)** — sem dependências pip
- **Ollama local** — GPU AMD RX 9060 XT 16GB (ROCm), sem custo
- **Modelo:** `qwen2.5vl:7b` (multimodal) — melhor OCR/transcrição de escrita à mão

### Arquivos e pastas
| Caminho                                                                  | Função                                   |
| ------------------------------------------------------------------------ | ---------------------------------------- |
| `/home/davib/Documents/Projetinhos/pipeline de notas/notas-brutas/`      | pasta de entrada (PDFs do Samsung Notes) |
| `/home/davib/Documents/Projetinhos/pipeline de notas/notas-processadas/` | PDFs migrados após sucesso               |
| `~/notas-pipeline/pipeline.py`                                           | script                                   |
| `~/notas-pipeline/history.json`                                          | histórico de processamento               |
| `1 - Notas brutas/<nome>.md`                                             | saída (transcrição crua)                 |

### Fluxo do script
1. Monitora a inbox (loop em background; `--once` executa e sai)
2. SHA-256 do arquivo → se já processado (history.json), pula — cada arquivo é processado 1x só, mesmo se renomeado
3. Renderiza páginas como PNG (`pdftoppm -r 150`) e envia em base64 ao Ollama
4. Prompt PT-BR pede transcrição fiel em Markdown, sem resumir/interpretar
5. Grava em `1 - Notas brutas/`, move o PDF e registra no histórico
6. Falha → marca `status: erro`, não move o arquivo

### Decisão
- Formato de saída: **transcrição crua** (bruto num primeiro momento; refinar depois)

## Conexões
[[Projetos]]

## Ação
---
- [x] Criar uma nota no Samsung notes e compartilhar automaticamente com o PC
- [ ] Criar um script em python que pega automaticamente as notas que foram adicionadas, lê as mesmas, e gera um arquivo markdown, adicionando eles na pasta do obsidian


# Referências
