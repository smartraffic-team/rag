# 09 — Roadmap e plano de validação

## Fases sugeridas (planejamento, não cronograma fechado)
1. **Estabilizar maquete** — conferir versão de firmware, conexões, vídeo, logs e comandos.
2. **Separar veículos por via** — definir ROIs e unidades de observação/contagem; validar em cenas gravadas.
3. **Controle adaptativo seguro** — decidir prioridade por fila, mínimo/máximo de verde, tempos de amarelo/vermelho geral, critério de justiça, pedestres, contingência.
4. **Instrumentar ensaio** — definir momento em que cada veículo começa e termina espera, persistir observações por veículo.
5. **Executar comparação** — cenários equivalentes com fase fixa e adaptativa, várias repetições e documentação das condições.
6. **Integrar telemetria** — definir contrato Edge/API, autenticação e armazenamento MySQL.
7. **Conectar dashboard** — substituir mocks por chamadas HTTP e atualizações autenticadas, mantendo UI de monitoramento.
8. **Documentar resultados** — tabelas e gráficos com medições reais de maquete, limitações e artigo.

## Teste científico prioritário — desenho proposto
**Pergunta:** o SmartTraffic reduz o tempo médio de espera em comparação com um ciclo fixo no mesmo cruzamento simulado?

**Variável manipulada:** tipo de controle (fixo/adaptativo).  
**Desfecho principal:** tempo de espera por veículo (segundos).  
**Controle experimental:** mesma sequência de chegada, quantidade e distribuição por vias, posicionamento da câmera, condições de iluminação e tempos de segurança.  
**Réplica:** número de repetições a definir antes dos testes.  
**Registro:** data, versão do algoritmo, configuração da maquete, cenário, estratégia, tempos brutos, falhas e observações.

Fórmula de comparação quando as medidas existirem:

`reducao_% = ((media_espera_fixo - media_espera_adaptativo) / media_espera_fixo) * 100`

**Atenção:** se `media_espera_fixo = 0`, fórmula não se aplica. Relatar também médias, dispersão e número de observações; não selecionar somente rodadas favoráveis.

## Critérios técnicos complementares — ainda não decididos
- Consistência da detecção e identificação única.
- Tempo de resposta do algoritmo e do comando.
- Convergência do estado reportado e LEDs reais.
- Recuperação após falha de câmera, cabo ou computador.
- Atendimento a pedestres e equidade entre vias.

## Marcos sugeridos
- M1: visão/serial demonstráveis com comportamento rastreável.
- M2: algoritmo por demanda implementado e reproduzível.
- M3: ensaios comparativos concluídos, com registros auditáveis.
- M4: dashboard conectado à telemetria real.
- M5: artigo com resultados e limitações descritas.
