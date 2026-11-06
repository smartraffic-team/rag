# 01 — Visão e escopo

SmartTraffic é um protótipo acadêmico para **um único cruzamento**, com vias A/B, dois conjuntos semafóricos, miniaturas Hot Wheels, processamento local Python + OpenCV + YOLOv8n e Arduino. Hipótese: adaptar o tempo de sinalização à quantidade de veículos aguardando pode reduzir espera em relação à temporização fixa.

**Maquete física:** detecção de carros/pessoas e atuação preliminar por serial já possuem código; duas câmeras simultâneas, contagem distinta das filas, desempate pelo veículo mais antigo, atendimento aos pedestres e lógica segura ainda são metas. Acessibilidade/identificação de PCD não é objetivo obrigatório v1; não inferir deficiência pela aparência.

**SUMO:** simula isoladamente a estratégia adaptativa, com sensores E2 em vez de visão computacional. Melhora virtual de 13,5%, não física.

**IoT:** dados de dispositivos/semáforos chegam à API e MySQL, consumidos por React e mobile futuro; o controle físico não pode depender da rede.

**ODS 9/11:** alinhamento potencial à inovação e mobilidade sustentável, sem benefício urbano mensurado.

**Termos:** detecção por quadro ≠ veículo único ≠ fila em espera; lógica fuzzy (difusa) é citada no Estado da Arte, não constitui algoritmo SmartTraffic.
