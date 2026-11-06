# 12 — SUMO: experimento realizado

**Fonte:** [relatório técnico](https://github.com/smartraffic-team/computer-vision/blob/feat-seguindo-com-o-projeto-para-2-webcam/simulation/sumo/reports/adaptive_vs_fixed.md), datado de 26/10/2026, e [configuração](https://github.com/smartraffic-team/computer-vision/blob/feat-seguindo-com-o-projeto-para-2-webcam/simulation/sumo/reports/sumo_configuration_reference.md).

**Simulador:** SUMO 1.28.0. Cruzamento duas vias perpendiculares A/B, sentido único, 2 faixas de aproximação, aproximadamente 100 m antes/depois; velocidade máxima 13,89 m/s (~50 km/h). Sem conversões, ciclistas ou pedestres. Quatro sensores virtuais E2 nos 50 m anteriores ao sinal, não câmeras.

**Demanda:** 75 carros via A (a cada 4 s) + 50 via B (a cada 6 s); inserção 0–300 s, simulação 360 s passo 1 s. Dez repetições pareadas por estratégia, valores de inicialização 1–10, total 20 execuções.

**Fixo:** 10 s verde + 3 amarelo + 1 vermelho simultâneo antes da outra via; ciclo 28 s. **Adaptativo:** verde mínimo 8 s, limite 20 s quando há veículos aguardando na via conflitante, critério de 30 s monitorados; mesmas transições. Entrada em detector não equivale necessariamente ao instante de parada.

| Indicador | Fixo | Adaptativo | Variação |
|---|---:|---:|---:|
| Espera média/veículo | 4,44 s | 3,84 s | −13,5% |
| Via A | 4,73 s | 3,95 s | −16,4% |
| Via B | 4,02 s | 3,68 s | −8,4% |
| Duração média da viagem | 24,82 s | 24,06 s | −3,1% |
| Fila média | 1,54 | 1,33 | −13,5% |
| Média das maiores filas | 4,50 | 4,50 | sem redução |

Menor espera global em **todos os dez pares**, porém Via B piorou em duas execuções; diferença média pareada 0,60 s (IC 95% da redução 0,50–0,71 s). Todos 125 veículos de cada execução concluíram. Nenhum verde conflitante nas verificações **lógicas** de simulação.

**Limitações:** um cenário regular e desigual, tempo verde fixo igual por via, sem YOLO/Arduino, pedestres ou condições reais. Não usar como evidência de melhoria física ou eficácia em cruzamentos urbanos reais.
