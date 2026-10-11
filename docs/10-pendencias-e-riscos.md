# 10 — Pendências, riscos e perguntas abertas

## Decisões necessárias com maior prioridade
| ID | Assunto | Pergunta/decisão | Impacto |
|---|---|---|---|
| P01 | Captura por via | Qual ROI identifica filas A e B? Como lidar com oclusão? | Algoritmo, validade dos dados |
| P02 | Contagem | Contar ocupação por quadro, entradas únicas ou veículos **em espera**? | Métrica e controle |
| P03 | YOLO e rastreador | Há tracking realmente funcional no cenário Hot Wheels? | Duplicações |
| P04 | Tempo verde | O que representam 19 s? Quais mínimo e máximo por sinal? | Lógica e segurança |
| P05 | Pedestres/PCD | Como solicitar/detectar travessia e atender acessibilidade sem inferir deficiência por aparência? | MVP e ética |
| P06 | Fallback | Quem inicia ciclo fixo se PC/câmera/serial falham? Qual watchdog? | Segurança funcional |
| P07 | Estado físico | Como o Arduino confirma execução de fase à aplicação? | Consistência do monitoramento |
| P08 | Híbrido/AWS | Qual carga roda localmente e qual na nuvem? | Infraestrutura e artigo |
| P09 | Medição científica | Como medir espera individual, repetições, cenários e variância? | Validação |
| P10 | Backend | Qual contrato de ingestão, periodicidade, autenticação e retenção? | Integração com web |
| P11 | Frontend | Onde está o repo do dashboard, o que de fato foi entregue pelo Codex? | Planejamento e onboarding |
| P12 | Hardware | Modelo de Arduino, webcams, circuito e esquema atualizado? | Reprodução e confiabilidade |
| P13 | Pesquisa | Questionários originais, data exata, método de amostragem e termo de consentimento? | Rigor acadêmico |
| P14 | Bibliografia | URLs, DOIs e BibTeX das afirmações do Estado da Arte? | Publicação |
| P15 | Custos | Qual orçamento/limite para a maquete e nuvem? | Viabilidade |

## Riscos identificados
- **Alto — sinais conflitantes ou transição insegura:** firmware atual não especifica intervalos de vermelho geral nem invariantes formais. Limitar uso à maquete; revisar estados e testes.
- **Alto — perda de supervisão:** fallback de inatividade por ausência de carros **não** cobre falha de processo/câmera. Requer comportamento autônomo no controlador.
- **Alto — métricas inválidas:** contagens repetidas de frame não representam chegadas únicas; não usar como medida de fluxo total/espera.
- **Médio/alto — comando e interface divergentes:** script atual muda estado sem ACK físico; sleeps bloqueantes podem duplicar atraso.
- **Médio — Flask exposto:** socket sem autenticação e CORS liberado; não disponibilizar fora de laboratório.
- **Médio — inconsistência documental:** Readmes de ambos os repositórios apontam arquivos/funcionalidades incorretos.
- **Médio — inferência de PCD:** não presumir inferência de deficiência por câmera. Desenhar acessibilidade com princípios de privacidade e segurança.
- **Médio — privacidade:** captura de pessoas em vídeo pode exigir políticas de acesso, consentimento/base legal e retenção compatíveis com LGPD.
- **Médio — expectativa acadêmica:** não confundir planos, métricas alvo e evidências observadas.

## Checklist antes de publicar qualquer decisão técnica
- [ ] Responsável designado.
- [ ] Evidência/link no issue ou PR.
- [ ] Implementação e versão identificadas.
- [ ] Teste reproduzível.
- [ ] Impacto em `computer-vision`, `arduino`, frontend/API e artigo analisado.
- [ ] Documentação e ADR atualizados.
