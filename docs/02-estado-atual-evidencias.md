# 02 — Estado atual e evidências (05/11/2026)

| Elemento | Status | Fonte/ressalva |
|---|---|---|
| Câmera, YOLOv8n, detecções de car/person | IMPLEMENTADO EM CÓDIGO | computer-vision/main/smarttraffic_app.py; por quadro |
| Duas câmeras + filas independentes | PENDENTE | README do computer-vision |
| Python/Arduino serial A/B | IMPLEMENTADO EM CÓDIGO | computer-vision e arduino/main, sem certificação física |
| Pedestres | DETECÇÃO DE PESSOA; CONTROLE PENDENTE | nenhuma fase de travessia comprovada |
| Fallback/watchdog seguro | PENDENTE | código didático sem verificação de segurança |
| SUMO e comparação fixa/adaptativa | TESTADO EM SIMULAÇÃO | 10 pares, 20 execuções, 125 carros cada |
| Espera 4,44→3,84 s (−13,5%) | RESULTADO VIRTUAL | não é maquete nem cidade |
| Banco MySQL V2.1 | CÓDIGO PRESENTE | database/main, 10 tabelas, Docker/CI/testes descritos |
| API NestJS/Prisma/JWT | CÓDIGO PRESENTE | api/main, Swagger/rotas/CI |
| Web React/HTTP | CÓDIGO PRESENTE | SmartTraffic-platform/main; não mais exclusivamente mocks |
| Mobile | INICIAL | README + LICENSE |
| Integração física completa com banco/API/web | NÃO COMPROVADA | exige testes ponta a ponta |

Não usar status "implementado" como sinônimo de "validado em hardware". Não exibir métricas da simulação como telemetria real.
