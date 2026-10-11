# 11 — Fontes e rastreabilidade

## Código-fonte (verificado em 10/10/2026)
- [smartraffic-team/computer-vision](https://github.com/smartraffic-team/computer-vision), branch `main`:
  - [`Teste_camera.py`](https://github.com/smartraffic-team/computer-vision/blob/main/Teste_camera.py) — blob `9877dd5256aa23fb65c5e8052eca03c1cbe78d77`.
  - [`Smartrafic_web.py`](https://github.com/smartraffic-team/computer-vision/blob/main/Smartrafic_web.py) — aplicativo simples de vídeo; **não** é a versão mais atual da lógica integrada.
  - [`requirements.txt`](https://github.com/smartraffic-team/computer-vision/blob/main/requirements.txt) — dependências declaradas.
  - [`README.md`](https://github.com/smartraffic-team/computer-vision/blob/main/README.md) — possui instruções desatualizadas.
  - `yolov8n.pt`, `templates/` — presentes no repositório (templates não auditados neste pacote).
- [smartraffic-team/arduino](https://github.com/smartraffic-team/arduino), branch `main`:
  - [`arduino_code.ino`](https://github.com/smartraffic-team/arduino/blob/main/arduino_code.ino) — blob `98272a5c11b12e14fa6d8ada34ded7d9cf4d21d5`.
  - [`README.md`](https://github.com/smartraffic-team/arduino/blob/main/README.md) — menção divergente a nome de arquivo.

**Importante:** esses hashes são de arquivos (blobs), não necessariamente o SHA do commit completo. Revalidar branch atual após mudanças.

## Materiais compartilhados anteriormente com o assistente
1. Artigo SmartTraffic em LaTeX, versão do usuário com Abstract e Resumo atualizados e demais seções antigas (arquivo de texto apresentado em 09/10/2026).
2. Artigo antigo BUBATAG — modelo de estilo/estrutura científica; não é evidência sobre o SmartTraffic.
3. Relatório da pesquisa exploratória com 25 pessoas.
4. Relatório de entrevistas com 2 profissionais de trânsito.
5. Documento de artefatos SmartTraffic — SWOT, CANVAS, casos de uso, diagramas e inventário; parte do material contém ideias aspiracionais, não entrega validada.
6. Diagramas de arquitetura híbrida e AWS, ainda sem versão arquitetural homologada.
7. Questionário de contexto respondido pela equipe em 09/10/2026.
8. Instruções de geração de frontend executadas no Codex, sem auditoria do produto resultante.

Os itens 1–6 **não foram anexados automaticamente ao repositório de contexto**; solicitar à equipe que adicione originais aprovados em pasta própria, respeitando privacidade e direitos de terceiros. Esta documentação registra somente informações consolidadas, sem reproduzir integralmente fontes não fornecidas para publicação.

## Conflitos conhecidos e prioridade de evidência
- **Nome:** nome oficial declarado `SmartTraffic`; nomes legados `SmarTraffic`/`Smartrafic` persistem em arquivos/código antigo.
- **Contagem:** equipe relatou ausência; código confirma contagem por frame, mas não fluxo único nem fila.
- **Ciclo fixo:** script Python contém alternância por inatividade; Arduino não contém ciclo autônomo após falha do Python.
- **Controle adaptativo:** implementado por limiar global de imagem; o modelo objetivo por demanda das vias ainda é futuro.
- **Arquitetura:** materiais anteriores propõem edge/cloud em versões distintas; somente orientação híbrida está declarada.
- **Frontend:** prompt foi executado, mas nenhuma implementação web foi inspecionada.

## Padrão de citação interna
Ao acrescentar afirmações, preferir: `[ORIGEM: arquivo ou declaração | DATA: AAAA-MM-DD | ESTADO: ... | EVIDÊNCIA: URL/PR/teste]`.
