# SmartTraffic — RAG atualizado (05/11/2026)

> Base de conhecimento em português. **Um cruzamento, vias A/B, dois semáforos**, maquete acadêmica. Separar código existente, testes virtuais e validação física.

**SmartTraffic:** proposta de controle semafórico adaptativo com webcam(s), Python/OpenCV/YOLOv8n e Arduino via serial USB. A maquete é o projeto central. A implementação física de contagem independente por via, fase de pedestres, failover seguro e validação quantitativa ainda está pendente.

**Resultado científico:** simulação complementar no **SUMO 1.28.0**, 10 pares de repetições (20 rodadas), 125 veículos por rodada, espera média fixa **4,44 s** contra adaptativa **3,84 s** (**13,5% menor**) no cenário virtual. O SUMO utiliza sensores virtuais, não YOLO/Arduino; **não atribuir 13,5% à maquete**.

**Software:** banco [MySQL V2.1](https://github.com/smartraffic-team/database) com Docker/CI e scripts; [API NestJS/Prisma](https://github.com/smartraffic-team/api) com JWT e ingestão autenticada; [plataforma React](https://github.com/smartraffic-team/SmartTraffic-platform) com consumo HTTP da API. O repositório [Mobile](https://github.com/smartraffic-team/Mobile) ainda possui somente README/LICENSE em main. Código disponível não prova implantação ou integração ponta a ponta.

## Índice
- [01 Visão](docs/01-visao-produto.md)
- [02 Estado atual](docs/02-estado-atual-evidencias.md)
- [03 Arquitetura](docs/03-arquitetura-integracoes.md)
- [04 Visão computacional](docs/04-computer-vision.md)
- [05 Arduino](docs/05-arduino.md)
- [06 Web/mobile](docs/06-plataforma-web.md)
- [07 Banco e telemetria](docs/07-dados-e-telemetria.md)
- [08 Artigo e pesquisa](docs/08-pesquisa-e-artigo.md)
- [09 Roadmap](docs/09-roadmap-e-validacao.md)
- [10 Riscos](docs/10-pendencias-e-riscos.md)
- [11 Fontes](docs/11-fontes-e-rastreabilidade.md)
- [12 SUMO](docs/12-sumo-experimentos.md)
- [13 Banco/API](docs/13-database-api.md)

## Status
- **IMPLEMENTADO NO REPOSITÓRIO:** código inspecionado; funcionamento em campo não garantido.
- **TESTADO EM SIMULAÇÃO:** experimento SUMO reproduzível, não valida hardware.
- **RELATADO:** informado pela equipe em artigo/conversa.
- **PLANEJADO/PENDENTE:** ainda sem evidência de implementação concluída.
- **DOCUMENTADO COMO TESTADO:** relatório/testes existem, mas não reexecutados nesta revisão.

O banco e a API não devem comandar diretamente os LEDs; o controle seguro precisa continuar local. Projeto exclusivamente didático, não homologado para sinalização de via pública.
