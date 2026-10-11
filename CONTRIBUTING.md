# Como contribuir para o contexto SmartTraffic

1. Abra issue para fato novo, dúvida ou discrepância.
2. Verifique código, fonte ou confirmação do responsável.
3. Atualize documentos correspondentes e classifique o estado do conteúdo.
4. Para escolha arquitetural, registre um ADR.
5. Envie PR com `Contexto`, `O que mudou`, `Evidências`, `Sistemas impactados` e `Pendências`.
6. Revise se existem afirmações convertendo expectativas em resultados comprovados.

## Definição de pronto de documentação
- Fonte ou origem descrita.
- Data e responsável quando disponíveis.
- Status coerente com a evidência.
- Nenhuma métrica fictícia indicada como resultado experimental.
- Sem dados pessoais de respondentes, credenciais, imagens sensíveis ou chaves de dispositivos.
- Terminologia `SmartTraffic` padronizada.
- Links para código e materiais atualizados.

## Fluxo entre times
- Mudança em firmware que altera comandos exige revisão do contrato Python ↔ Arduino.
- Mudança no Python que altera telemetria exige revisão do contrato Edge ↔ API e mocks web.
- Mudança no banco/API exige revisão de tipos/contracts do frontend.
- Novos experimentos exigem atualização em `docs/08-pesquisa-e-artigo.md` e `docs/09-roadmap-e-validacao.md`.
