# 07 — Dados e telemetria

**Fonte de verdade:** [database/sql/01_schema.sql](https://github.com/smartraffic-team/database/blob/main/sql/01_schema.sql), **V2.1**, MySQL 8.4. Dez tabelas: `cruzamentos`, `vias`, `usuarios`, `dispositivos`, `semaforos`, `leituras_trafego`, `historico_fases`, `alertas`, `telemetria_dispositivos`, `eventos_sistema`.

Escopo rígido: CRZ-001, vias A/B maiúsculas, dois semáforos. Defaults de verde mínimo/padrão/máximo 8/10/20 s, amarelo 3 s, vermelho simultâneo 1 s; **parâmetros experimentais**, não segurança física comprovada.

Leitura `QUADRO`: carros visíveis no frame, sem veículos únicos. `AGREGADA`: período obrigatório. Origem distinta `VISAO_COMPUTACIONAL`, `SIMULACAO`, `MANUAL`. UUID de evento obrigatório e único por dispositivo, usado para deduplicar reenvios; via A/B obrigatória. Histórico limita fase aberta por semáforo; API valida cronologia adicional. Alertas com ciclo de reconhecimento/resolução e datas.

Database tem Docker, seed, scripts de backup/migração e CI; README declara 144 testes SQL. A atualização não reexecutou esses testes. A API usa Prisma por introspecção, não deve sobrescrever constraints com `db push`.
