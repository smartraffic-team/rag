# 04 — Visão Computacional

**Entrada atual:** [`smarttraffic_app.py`](https://github.com/smartraffic-team/computer-vision/blob/main/smarttraffic_app.py) e [`smarttraffic_config.py`](https://github.com/smartraffic-team/computer-vision/blob/main/smarttraffic_config.py), não o legado `Teste_camera.py`.

**Implementado no código:** webcam única, YOLOv8n detectando `car` e `person`, contagem de caixas por quadro, stream MJPEG, Flask/Socket.IO, modos manual/automático, regras preliminares por limiares de três e um automóvel, comandos seriais A/B. Configuração por variáveis SMARTTRAFFIC_*; `.env.example` não é carregado automaticamente.

**Não comprovado:** tracking persistente, detecção de filas independentes A/B, estimativa de ordem de chegada, duas câmeras ao mesmo tempo, solicitação segura de pedestre, confirmação de estado Arduino, ingestão HTTP na API e ensaio quantitativo da maquete.

**Separação:** pasta `simulation/sumo` existe na branch `feat-seguindo-com-o-projeto-para-2-webcam`. Seu algoritmo lê detectores virtuais e nunca deverá ser descrito como teste de precisão YOLO.
