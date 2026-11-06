# 13 — Database e API

## Database V2.1 (schema oficial)
[Repositório](https://github.com/smartraffic-team/database) usa MySQL 8.4, Docker local, documentação Aiven TLS, CI, migrations e backup. Tabelas: `cruzamentos`, `vias`, `usuarios`, `dispositivos`, `semaforos`, `leituras_trafego`, `historico_fases`, `alertas`, `telemetria_dispositivos`, `eventos_sistema`. Restrição para um cruzamento CRZ-001, vias A/B, seed com dois semáforos. Padrões 8/10/20/3/1 s. Migração `001_v2_0_para_v2_1.sql`. README declara **144 testes**; não reexecutados nesta atualização. SQL é fonte de verdade; não sobrescrever com Prisma `db push`.

## API
[Repositório](https://github.com/smartraffic-team/api) possui NestJS + Prisma, login JWT/bcrypt, perfis `ADMINISTRADOR`, `OPERADOR`, `VISUALIZADOR`, chave `x-ingestion-key` para ingestão, validação, Swagger, CI/testes. Prefixo `/api/v1`. Endpoints para `/auth/login`, `/auth/me`, `/cruzamento`, `/vias`, `/semaforos`, `/dispositivos`, `/leituras`, `/historico-fases`, `/alertas`, `/telemetria`, `/eventos`, `/dashboard/resumo`, `/health`. IDs BigInt serializados como string; datas UTC ISO, paginação `{dados,total,pagina,limite}`.

**Ingestão:** UUID `identificadorEvento` estável em reenvios, `dispositivoId`, `via` A/B confiável, `registradoEm`, `tipoLeitura` QUADRO/AGREGADA, contagens. Chave somente em Python/backend, nunca nos clientes. Sem câmera identificando a via, não fabricar contagens por A/B.

**Nuvem:** README da API cita database `smarttraffic_v21` para V2.1 na Aiven e database legado `smarttraffic` de oito tabelas. A presença dessas instruções não comprova implantação/conectividade atual.

**Clientes:** [React](https://github.com/smartraffic-team/SmartTraffic-platform) já consome rotas HTTP no código; Mobile ainda inicial. Falta demonstrar integração ponta a ponta com a maquete e testes reais.
