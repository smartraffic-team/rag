# 04 — Computer Vision: implementação de referência

**Fonte de código:** [`computer-vision/Teste_camera.py`](https://github.com/smartraffic-team/computer-vision/blob/main/Teste_camera.py) (branch `main`, SHA de blob `9877dd5256aa23fb65c5e8052eca03c1cbe78d77`). A equipe confirmou esse arquivo como a versão mais atual.

## Stack e parâmetros encontrados
- Python; `ultralytics.YOLO`, `cv2`, `serial`, Flask, Flask-SocketIO, `threading`.
- Modelo: `MODEL_PATH='yolov8n.pt'` (arquivo no repositório).
- Câmera: `CAMERA_INDEX=1`; dimensões 640×480; `FRAME_RATE=30`; inferência a cada **3 quadros** (`DETECTION_FRAME_SKIP=3`); JPEG qualidade 80.
- Arduino: `PORTA_ARDUINO='/dev/ttyUSB0'`; serial 9600 baud; comentário do script menciona COM3/COM4 no Windows.
- Limiares: `LIMIAR_TROCA_B=3`, `LIMIAR_VOLTA_A=1`.
- Inatividade: 15 s; alternância do ciclo padrão: 10 s. **Não confundir com o verde provisório de 19 s para o futuro MVP.**
- Flask: porta 5000, `host='0.0.0.0'`, página `/`, MJPEG `/video_feed`.
- Socket.IO com eventos `status_inicial`, `status_update`, `contadores_update`, `yellow_blink_web`, `ativar_manual`, `ativar_auto`, `alternar_semaforo_manual`.

## Funções e o que fazem
- `connect_arduino()`: abre serial; falha gera log e retorna False.
- `initialize_camera()`: inicializa webcam e solicita configurações de resolução/FPS.
- `load_model()`: carrega `yolov8n.pt`.
- `detect_objects()`: redimensiona frame, roda inferência, soma rótulos `car` e `person` detectados no frame.
- `update_traffic_state()`: atualiza contadores, registra atividade, executa regras de alternância e publica Socket.IO.
- `check_inactivity_and_cycle()`: após inatividade de detecção de carros, ativa ciclo A/B periódico.
- `change_light()`: envia comando A/B e publica eventos para simulação visual da transição.
- `detection_loop()`: obtém quadros; processa a cada 3 quadros; dorme aproximadamente `1/FRAME_RATE` por iteração.
- `generate_stream()`: gera fluxo MJPEG com frames armazenados.
- `activate_manual()` / `activate_auto()` / `set_manual_light()`: controles locais via eventos da UI.

## Restrições conhecidas do código
1. **Não há rastreamento persistente identificado:** usa `self.model(frame_resized, verbose=False)` e soma caixas. O mesmo objeto pode reaparecer em vários quadros; a contagem é instantânea, não cumulativa de veículos únicos.
2. **Ausência de ROI por via:** `cars` agrega o quadro inteiro; a regra não mede filas A/B separadamente.
3. **Classes restritas:** somente `car` e `person` incrementam contadores; motos/caminhões/ônibus não entram nesses contadores como categorias próprias.
4. **Comandos não confirmados:** `change_light` altera `current_light` após espera mesmo sem confirmação do Arduino; `send_command` pode falhar.
5. **Temporização:** a simulação web de amarelo bloqueia com sleeps, seguida por espera adicional, podendo divergir do tempo físico real.
6. **Sem detecção de falha como contingência efetiva:** ausência/erro de câmera ou inferência não equivale ao failover seguro desejado.
7. **Segurança da interface:** chave Flask de exemplo e `cors_allowed_origins='*'`; eventos de comando manual não exibem autenticação/autorização no script. Não expor em rede pública.
8. **Documentação divergente:** README chama `Smartrafic_web.py` de principal, mas ele implementa apenas streaming Flask simples; o programa abrangente está em `Teste_camera.py`.
9. **Nenhuma medida de acurácia, mAP, FPS observado ou redução de espera foi verificada** a partir deste código.

## Próximas tarefas indicadas (PROPOSTAS)
- Definir região de interesse para cada aproximação e convenção de contagem de veículos **em espera**.
- Avaliar rastreamento de objetos e IDs com cenas específicas da maquete; não presumir qualidade suficiente do modelo sem testes.
- Separar captura, inferência, decisão, estado semafórico, serial e telemetria em módulos testáveis.
- Usar máquina de estados e confirmação do controlador; não usar sleeps bloqueantes como substituto de protocolo.
- Instrumentar entrada/saída de fila e tempo de espera para experimentos reprodutíveis.
- Criar testes com clipes gravados e cenários determinísticos.

## Dependências declaradas
`flask==3.0.0`, `flask-socketio>=5.3.0`, `opencv-python-headless==4.8.1.78`, `pyserial==3.5`, `ultralytics>=8.0.0`; validar compatibilidades do ambiente da equipe. O repositório também inclui `Smartrafic_web.py` para streaming simples, `templates/`, pesos e README.
