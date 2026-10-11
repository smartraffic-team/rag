# 07 — Modelo de dados e contratos de telemetria

> **STATUS: PROPOSTO NESTA DOCUMENTAÇÃO.** Não é o esquema MySQL existente nem o formato já transmitido pelo Python.

## Entidades candidatas
| Entidade | Propósito | Campos indicativos |
|---|---|---|
| `users` | Operadores do painel | id, nome, email, role, status |
| `intersections` | Cruzamento simulado; expandível | id, codigo, nome, descricao, status |
| `devices` | Webcam, Edge, Arduino, controladores | id, intersection_id, tipo, identificação, firmware, online, last_seen_at |
| `approaches` | Aproximações/vias A e B | id, intersection_id, code, direction |
| `traffic_measurements` | Medição agregada por via | id, intersection_id, approach_id, captured_at, window_seconds, car_count, pedestrian_count, source |
| `signal_events` | Mudanças e estado das fases | id, intersection_id, phase, timestamp, origin, acknowledged |
| `device_heartbeats` | Saúde/comunicação | id, device_id, received_at, status, latency_ms |
| `alerts` | Anomalias e reconhecimento | id, device_id, severity, type, status, opened_at, resolved_at |
| `experiment_runs` | Ensaios científicos | id, scenario_id, strategy, started_at, ended_at, notes |
| `wait_observations` | Espera medida nos ensaios | id, run_id, vehicle_ref, approach_id, wait_seconds |
| `audit_logs` | Alterações administrativas | id, user_id, action, entity, created_at |

**Cuidado:** `car_count` por frame não equivale a veículos únicos e `wait_seconds` ainda não tem coleta implementada. O esquema deve separar medições observadas e resultados simulados.

## Contrato ilustrativo de telemetria v0 — PROPOSTA
```json
{
  "schemaVersion": "0.1-draft",
  "messageId": "evt-demo-001",
  "deviceId": "edge-demo-01",
  "intersectionId": "crossing-demo-01",
  "capturedAt": "2026-10-10T12:00:00Z",
  "measurements": [
    {"approachId": "A", "detectedCarsInFrame": 2},
    {"approachId": "B", "detectedCarsInFrame": 1}
  ],
  "signalState": {"currentGreenApproach": "A", "mode": "adaptive-demo"},
  "health": {"cameraOnline": true, "controllerConnected": true}
}
```
**Valores e campos acima são ilustrativos;** o código Python atual não envia esse JSON nem identifica contagens por via. Não tratar como evento real.

## Recomendações de integração
- Incluir versão de esquema e `messageId` para idempotência.
- Distinguir `capturedAt` de `receivedAt`; usar timestamps UTC e exibir no fuso adequado.
- Credenciais individuais de dispositivo, HTTPS, autenticação no ingestion endpoint.
- Armazenar aggregates com periodicidade definida; evitar frame/vídeo integral no MySQL.
- Definir política de retenção, minimização e controle de acesso; imagens de trânsito podem incluir informações pessoais.
- Definir sinal de qualidade/ausência para dados indisponíveis; **não apresentar zero como ausência de medição**.

## Exemplo de API futura — PROPOSTO
`POST /telemetry`, `GET /dashboard/overview`, `GET /intersections`, `GET /intersections/:id`, `GET /devices`, `GET /traffic/history`, `GET /alerts`. Nenhuma dessas rotas NestJS está confirmada como implementada.
