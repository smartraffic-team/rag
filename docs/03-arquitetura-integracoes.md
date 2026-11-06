# 03 — Arquitetura e integrações

## Local
`Webcam → Python/OpenCV/YOLOv8n → contagem por quadro → regras preliminares → serial USB 9600 → Arduino → LEDs A/B`.
Flask/Socket.IO disponibiliza visualização local. Ainda não há no controle físico contagem confiável separada por via, protocolo de ACK completo ou watchdog autônomo.

## Dados (arquitetura do software)
`Python/Edge --HTTPS + x-ingestion-key--> NestJS/Prisma --MySQL V2.1--> React/Mobile via API HTTPS + JWT`.
Banco não aciona semáforos; API não decide fases físicas. Operação local deve continuar sem internet. Cliente Python de ingestão e testes ponta a ponta precisam comprovação. Mobile ainda sem implementação funcional.

**Aiven:** banco gerenciado com TLS, configurações em secrets; Docker MySQL local para desenvolvimento. Não expor credenciais em VITE_* nem mobile.

**Telemetria:** distinguir `QUADRO` de `AGREGADA`, via A/B somente se reconhecida com confiança, UUID de evento reusado em reenvio, origem `VISAO_COMPUTACIONAL`, `SIMULACAO` ou `MANUAL`, horários UTC.
