# 06 — Plataforma web SmartTraffic

## Objetivo e usuários
Interface exclusiva para funcionários responsáveis pelo trânsito acompanharem um cruzamento do protótipo, equipamentos, histórico de tráfego, fases e alertas. A interface é um sistema **de monitoramento**, não deve controlar sinais físicos remotamente no MVP.

## Tecnologias escolhidas
- **Frontend:** React + Vite + TypeScript, Tailwind CSS, shadcn/ui/Radix UI, React Router, TanStack Query, Recharts, Lucide React; Zustand para estado global quando necessário, React Hook Form + Zod para validação.
- **Backend futuro:** NestJS + Prisma + **MySQL** (preferência expressa da equipe).
- **Tempo real futuro:** WebSocket entre API e navegador; MQTT opcional para dispositivos se necessário, **não decidido**.
- **Deploy:** AWS é direção desejada para histórico/processamento, serviços específicos ainda indefinidos.
- **Estado atual:** prompt completo do frontend foi executado no Codex; **não foi inspecionado o repositório de frontend nem confirmadas as telas concluídas**.

## Identidade visual
A equipe criará uma pasta **`imgs/` na raiz do repositório de frontend**, com logos, cores e referências de identidade. Agentes e desenvolvedores devem inspecioná-la antes de implementar a UI. **Paleta oficial ainda não foi fornecida**; não transformar cores temporárias em identidade oficial.

## Escopo de interface solicitado ao Codex (PLANEJADO, não entregue comprovadamente)
| Rota proposta | Função |
|---|---|
| `/dashboard` | KPIs, volume e categorias de veículos, status, alertas |
| `/intersections` e `/:id` | Cadastro/lista e detalhe do cruzamento |
| `/devices` e `/:id` | Inventário IoT, saúde, firmware, cadastro mockado |
| `/traffic` | Histórico, gráficos e filtros |
| `/monitoring` | Telemetria, fase, temporizador e cenários demonstrativos |
| `/alerts` | Listagem, severidades, reconhecimento e resolução simulada |
| `/reports` | Relatórios e exportação CSV; visualização para impressão |
| `/settings` | Tema, datas, atualização, preferências |
| `/login` | Autenticação **mockada** e papéis demonstrativos |

## Perfis demonstrativos
`ADMIN`, `OPERATOR`, `VIEWER`. Permissões de UI no frontend não substituem controle real de acesso na API. Não armazenar credenciais reais no browser como mocks.

## Regras obrigatórias para os dados mockados
- Marcar visivelmente os valores como **demonstrativos**.
- Criar referências coerentes entre dispositivos, cruzamentos, alertas e medições.
- Centralizar os dados em provider/mock service; não distribuir números aleatórios pelos componentes.
- Permitir filtros, gráficos, exportações e simulações funcionais; não apenas interfaces decorativas.
- Separar `services/contracts`, `services/mock` e futuro `services/http` para migração simples.
- Uma única fonte de verdade para estado da simulação; incluir cenários offline e reconexão.

## Integração futura
Navegador → API NestJS → MySQL; Python/Edge → API de ingestão autenticada → agregação → WebSocket. Os formatos de payload deste pacote são **propostas iniciais**, não contrato vigente.

## Questões pendentes
Repo/branch de frontend, identidade visual final, implantação, domínio, JWT/SSO, permissões detalhadas, retenção e vídeo ao vivo. Não misturar imagens brutas com metadados nem armazenar vídeo indiscriminadamente.
