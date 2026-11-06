# 06 — Plataforma e mobile

[SmartTraffic-platform](https://github.com/smartraffic-team/SmartTraffic-platform) evoluiu de mocks para **cliente HTTP REST**. `src/services/provider.ts` consome `src/services/http.ts`; `VITE_API_URL` aponta para `/api/v1`. JWT é obtido com `/auth/login`, validado em `/auth/me`, guardado no sessionStorage. Perfis `ADMINISTRADOR`, `OPERADOR`, `VISUALIZADOR` exigem validação na API.

Recursos no código: dashboard/resumo, cruzamento/vias, semáforos, dispositivos, leituras, alertas, telemetria, histórico de fases, relatórios. **Não comanda LEDs**. Gráficos e CSV podem usar apenas a página recebida; registros de estados não são confirmação física; algumas categorias de veículos não são medidas pelo banco atual.

**Implantação/conexão ponta a ponta não reexecutada nesta revisão.** Não chamar telemetria mockada ou simulada de medição física. Imagens da identidade visual estão em `imgs/` do repositório da plataforma.

[Mobile](https://github.com/smartraffic-team/Mobile) contém somente README e LICENSE na main inspecionada: integração ainda futura.
