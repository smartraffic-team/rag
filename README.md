# SmartTraffic — Repositório de contexto compartilhado

> **Snapshot de contexto:** 10/10/2026. **Idioma:** pt-BR. **Nome oficial:** SmartTraffic.  
> Este repositório é uma **base de conhecimento**, não o código-fonte dos sistemas. Informações oriundas de conversas e materiais acadêmicos devem ser confrontadas com o código e validadas pela equipe antes de uso operacional.

## O que é
Projeto acadêmico de protótipo semafórico inteligente que utiliza visão computacional para detectar veículos e pedestres em uma **maquete representando um único cruzamento**. A finalidade de pesquisa é avaliar se um controle adaptativo baseado na demanda consegue reduzir o **tempo médio de espera dos veículos**, comparado a um controle de tempo fixo.

A equipe usa Python, OpenCV, **YOLOv8n** e Arduino no protótipo. Uma plataforma web está sendo iniciada com React + TypeScript e dados mockados; para o backend futuro foram escolhidos NestJS + Prisma + **MySQL**. A arquitetura desejada é híbrida, mas a divisão detalhada entre processamento local e AWS permanece pendente.

## Leia nesta ordem
1. [`docs/01-visao-produto.md`](docs/01-visao-produto.md) — problema, escopo, objetivos, atores e requisitos.
2. [`docs/02-estado-atual-evidencias.md`](docs/02-estado-atual-evidencias.md) — **implementado × relatado × planejado**.
3. [`docs/03-arquitetura-integracoes.md`](docs/03-arquitetura-integracoes.md) — sistemas, comunicação e decisões ainda abertas.
4. [`docs/04-computer-vision.md`](docs/04-computer-vision.md) — leitura do código Python de referência.
5. [`docs/05-arduino.md`](docs/05-arduino.md) — firmware, sinais, limitações de segurança.
6. [`docs/06-plataforma-web.md`](docs/06-plataforma-web.md) — stack, telas, contratos e dados mockados.
7. [`docs/07-dados-e-telemetria.md`](docs/07-dados-e-telemetria.md) — entidades e proposta de payload (não implementado).
8. [`docs/08-pesquisa-e-artigo.md`](docs/08-pesquisa-e-artigo.md) — pesquisas de campo e diretrizes científicas.
9. [`docs/09-roadmap-e-validacao.md`](docs/09-roadmap-e-validacao.md) — fases e desenho dos experimentos.
10. [`docs/10-pendencias-e-riscos.md`](docs/10-pendencias-e-riscos.md) — decisões, riscos e dúvidas.
11. [`docs/11-fontes-e-rastreabilidade.md`](docs/11-fontes-e-rastreabilidade.md) — inventário e procedência.
12. [`adr/README.md`](adr/README.md) — registro de decisões técnicas.

## Semântica dos estados
- **IMPLEMENTADO NO CÓDIGO:** rotina presente em arquivo inspecionado; **não equivale** a ensaio bem-sucedido.
- **RELATADO PELA EQUIPE:** informação declarada por integrante, sem verificação independente.
- **DECIDIDO:** escolha de escopo ou tecnologia explicitamente comunicada.
- **PLANEJADO:** funcionalidade desejada, ainda não confirmada como implementada.
- **PENDENTE:** necessita definição, teste, documentação ou aprovação.
- **PROPOSTO NESTA DOCUMENTAÇÃO:** sugestão de arquitetura/contrato, não decisão da equipe.

## Repositórios de software confirmados
- [Arduino](https://github.com/smartraffic-team/arduino) — arquivo `arduino_code.ino` na branch `main`.
- [Computer Vision](https://github.com/smartraffic-team/computer-vision) — **`Teste_camera.py` é a versão mais atual, conforme confirmado pela equipe**.

## Regra de atualização
Toda alteração material deve indicar **data, responsável, origem, sistemas impactados e estado**. Para decisões novas, registre um ADR; para funcionalidades concluídas, atualize o quadro de status e linke commit/PR/teste. **Não edite o histórico para converter planos em fatos**.

## Limites de aplicação
O firmware e scripts descritos são de **maquete acadêmica** e **não devem ser usados para controlar tráfego em via pública** sem projeto formal, redundância, intertravamento, conformidade e validação por profissionais competentes.
