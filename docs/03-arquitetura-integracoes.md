# 03 — Arquitetura e integração entre sistemas

## Contexto aprovado
- Objetivo de arquitetura **híbrida** (DECIDIDO).
- Python/YOLO/OpenCV e Arduino na maquete (IMPLEMENTADO NO CÓDIGO).
- AWS desejada para **histórico e processamento** (PLANEJADO; modalidade e fronteiras não definidas).
- Painel web de operadores e backend futuro em React, NestJS, Prisma e MySQL.
- Comunicação notebook ↔ Arduino: **USB**, porta serial pelo software (conector físico referido pela equipe como USB-C; modelo/cabo não especificados).

## Fluxo existente no repositório Python
```
Webcam → OpenCV → YOLOv8n → contagens por quadro
                                  ↓
                  regras locais no Python (A/B)
                                  ↓ serial 9600
                  Arduino → LEDs dos semáforos A/B
                                  ↓
           Flask / Socket.IO: status e vídeo local
```
**Observação:** ilustra o código encontrado, não uma topologia de hardware certificada nem a arquitetura híbrida final.

## Arquitetura-alvo de referência — PROPOSTA, NÃO DECISÃO FINAL
```
Maquete: webcam → aplicação Edge Python → controlador Arduino
                       │
                       └─ telemetria autenticada ─→ API NestJS
                                                      ├─ MySQL
                                                      └─ WebSocket → React Dashboard

                              AWS: hospedagem/histórico e processamento a definir
```
- A operação local da sinalização **não deve depender** da disponibilidade do dashboard ou da internet.
- O backend pode receber telemetria por HTTPS inicialmente; MQTT é opção futura, **não requisito aprovado**.
- Os dados e contratos de interface ainda precisam ser definidos e versionados.
- Não enviar comandos de troca semafórica do painel web na primeira versão.

## Questões em aberto
1. Quais funções serão processadas na nuvem vs notebook?
2. Como haverá sincronização eventual e fila de telemetria sem rede?
3. Qual sistema será autoridade do estado de fase: Arduino, Python ou ambos?
4. Qual será o protocolo versionado entre Edge e API? Autenticação mTLS/token por dispositivo?
5. Quais métricas brutas vs agregadas serão armazenadas e por quanto tempo?
6. AWS: quais serviços, custos, requisitos e disponibilidade?
7. Como garantir estados seguros se processador, câmera, cabo ou comunicação falharem?

## Restrições essenciais
Este é um protótipo didático. Não confundir status publicado por aplicação Python com confirmação do estado físico dos LEDs. Operação real exigiria projeto específico e independente de segurança funcional e normas aplicáveis.
