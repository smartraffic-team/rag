# 11 — Fontes e rastreabilidade

**Inspeção estática em 05/11/2026, sem reexecução dos pipelines:**
- [computer-vision/main](https://github.com/smartraffic-team/computer-vision) — `smarttraffic_app.py`, `smarttraffic_config.py`, README.
- [computer-vision — simulation/sumo](https://github.com/smartraffic-team/computer-vision/tree/feat-seguindo-com-o-projeto-para-2-webcam/simulation/sumo) — `run_experiment.py`, `adaptive_controller.py`, `reports/adaptive_vs_fixed.md`, `reports/sumo_configuration_reference.md`, CSV.
- [arduino/main](https://github.com/smartraffic-team/arduino) — `arduino_code.ino`.
- [database/main](https://github.com/smartraffic-team/database) — `sql/01_schema.sql` V2.1, `02_seed_maquete.sql`, `tests/03_testes_integridade.sql`, README, CI.
- [api/main](https://github.com/smartraffic-team/api) — `prisma/schema.prisma`, controllers, README, CI.
- [SmartTraffic-platform/main](https://github.com/smartraffic-team/SmartTraffic-platform) — `src/services/provider.ts`, `src/services/http.ts`, README.
- [Mobile/main](https://github.com/smartraffic-team/Mobile) — README e LICENSE.
- Artigo PDF SmartTraffic compartilhado em 29/10/2026 e decisões desta conversa: são **materiais de contexto**, não prova física.

Precedência: esquema vem do SQL database vigente; endpoints da API vigente; tela da plataforma; medições do relatório SUMO; status físico do firmware/código mais testes reais. Evitar perpetuar prompts e starters antigos como estado atual.
