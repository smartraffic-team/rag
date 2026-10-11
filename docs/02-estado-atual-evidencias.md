# 02 — Estado atual: fato, relato e plano

**Referência temporal:** 10/10/2026; leitura estática dos repositórios GitHub na branch `main` e informações prestadas pela equipe em 09/10/2026.

| Área | Estado | Evidência / cautela |
|---|---|---|
| Maquete com veículos Hot Wheels | RELATADO PELA EQUIPE | Protótipo acadêmico físico; não há laudo de teste anexado. |
| Webcams e notebook | RELATADO PELA EQUIPE | Equipamentos disponíveis, modelos não informados. |
| YOLOv8n | IMPLEMENTADO NO CÓDIGO | `YOLO('yolov8n.pt')` no Python; pesos versionados no repositório. |
| Detecção de carros e pessoas | IMPLEMENTADO NO CÓDIGO e RELATADO FUNCIONAL | `detect_objects`: classes `car` e `person`. |
| Contadores exibidos na interface | IMPLEMENTADO NO CÓDIGO | Contagem de caixas por quadro, **não** veículos distintos ou fila por via. |
| Rastreamento/identificação única | NÃO IDENTIFICADO | `model(frame, verbose=False)` sem `track()` no arquivo inspecionado. |
| Arduino com dois grupos A/B | IMPLEMENTADO NO CÓDIGO | `arduino_code.ino`, seis saídas de LEDs e comandos seriais A/B. |
| Comunicação serial Python→Arduino | IMPLEMENTADO NO CÓDIGO | `serial.Serial(...,9600)` e `write(command.encode())`; sucesso da execução não foi auditado. |
| Controle preliminar por limiares | IMPLEMENTADO NO CÓDIGO | `LIMIAR_TROCA_B=3`, `LIMIAR_VOLTA_A=1`; baseado em contagem total na imagem. |
| Alternância por inatividade | IMPLEMENTADO NO CÓDIGO | `INACTIVITY_TIMEOUT=15`, `NORMAL_CYCLE_TIME=10`. Não é o failover por perda de software. |
| Interface Flask e Socket.IO | IMPLEMENTADO NO CÓDIGO | `/`, `/video_feed`; eventos de status, contadores e modo manual. |
| Priorização conforme filas separadas | PLANEJADO | Requer contagem por aproximação/ROI; lógica atual não separa A e B. |
| Tempo máximo configurável por sinal | PLANEJADO | 19 segundos provisórios; sem configuração encontrada no script. |
| Travessia de pedestres e acessibilidade | PLANEJADO | Detecção de pessoas existe; lógica de atendimento não foi identificada. |
| Retorno seguro ao modo fixo em caso de falha | PLANEJADO | Falta watchdog local e máquina de estados segura. |
| Medição de espera e comparação experimental | PLANEJADO | Ainda não há métricas experimentais registradas. |
| AWS e banco em nuvem | DECIDIDO COMO DIREÇÃO | Histórico/processamento, sem desenho final nem implementação comprovada. |
| Dashboard React com mocks | RELATADO EM DESENVOLVIMENTO | Prompt executado no Codex; **código gerado não foi analisado**. |
| API NestJS + Prisma + MySQL | DECIDIDO COMO STACK FUTURA | Não há evidência de API entregue ou banco provisionado. |

## Interpretação crítica
A equipe relatou inicialmente que contagem e controle ainda não existiam, mas `Teste_camera.py` contém **contagem de detecções por frame** e **regras preliminares de alternância**. É correto descrever isso no artigo como implementação inicial **não validada experimentalmente**; é incorreto afirmar contagem de veículos únicos, comparação de filas, redução comprovada de espera ou funcionamento seguro de contingência.

## Atualização obrigatória
Antes de mudar um estado para IMPLEMENTADO/VALIDADO: inserir link do PR/commit, cenário testado, data do teste e resultado verificável. Evitar que status de documentação substitua evidência.
