# 09 — Roadmap e validação

**Já documentado no código:** visão por webcam única, controle serial inicial; experimento virtual SUMO concluído; database SQL V2.1 + Docker + CI; API NestJS/Prisma/JWT; frontend HTTP/JWT. Isso não significa implantação validada.

**Próximos passos:** (1) testes locais integrados Database→API→React; (2) cliente Python de ingestão autenticada com UUID e buffering offline; (3) visão com ROIs independentes A/B, tracking e filas; (4) Arduino com máquina de estados, watchdog, transição amarela/vermelho simultâneo e segurança; (5) fase segura para pedestres; (6) mobile; (7) ensaios físicos e novos cenários SUMO.

**SUMO já realizado:** 10 pares, distribuição desigual 75 A / 50 B, 360 s, adaptativo melhor na espera global em todos pares. Limites: um cenário, sem câmera, sem Arduino, sem pedestres. Comparativo físico com medição de espera permanece pendente.
