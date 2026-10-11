# 08 — Pesquisa de campo e artigo científico

## Estado do artigo
- Escrito em LaTeX/Overleaf, português do Brasil (`babel[brazil]`).
- Título atual: **Controle de Tráfego Utilizando Câmeras Inteligentes, Orientado por Visão Computacional**.
- Nome padronizado: **SmartTraffic**.
- Ordem adotada: **Abstract (inglês) → Resumo (português) → Introdução → Objetivo → Estado da Arte → Metodologia → Conclusão → Agradecimentos → Referências**.
- Resumo/Abstract atuais mencionam visão computacional, YOLO/OpenCV, Registro-SP, levantamento de 25 + 2 pessoas e ODS 9/11.
- As seções **Introdução, Objetivos, Estado da Arte, Metodologia e Conclusão** do arquivo examinado ainda exigem revisão e alinhamento com a implementação real.
- Referências bibliográficas do `.bib` ainda precisam ser conferidas; não inferir correção só porque existe `\cite{...}`.

## Pesquisa exploratória — público geral
- **25 respondentes**, pesquisa **presencial**, aproximadamente em **abril de 2026**, conforme relato da equipe (data exata pendente).
- **92%** dos respondentes afirmaram já parar no sinal vermelho com via transversal vazia.
- **44%** estimaram perder **10 minutos ou mais por dia** em paradas em semáforos.
- **84%** atribuíram nota **3, 4 ou 5** (em escala de 1–5) a percepção de má sincronização/perda de tempo; **isso NÃO significa 84% concordando fortemente** (notas 4/5 eram 56%).
- **80%** prefeririam trajeto mais longo que evitasse sinais; **68%** sentem insegurança no semáforo à noite; **72%** não conheciam sinalização adaptativa.
- Foram mapeados **seis pontos semafóricos** na área observada em Registro-SP; o relatório identifica quatro em sequência na Avenida José Antônio de Campos. **Por decisão editorial, o nome da avenida foi removido do Resumo**. Pode ser usado na metodologia/localização, se aprovado.
- Para um dos pontos (#5), 40% dos participantes o apontaram como pior para ficar parado e 84% como mais perigoso. Trata-se de **percepção da amostra**, não comprovação objetiva de risco ou pior desempenho.

## Pesquisa exploratória — profissionais de trânsito
- **Dois entrevistados**, pesquisa **presencial**, aproximadamente abril/2026.
- Ambos apontaram 7h–9h como janela crítica e assinalaram um ponto (#4) como gargalo; ambos indicaram custo de implantação/manutenção como barreira.
- Com `n=2`, descrever como **ambos** ou **um dos dois**, evitando percentuais pseudoprecisos.
- Percepções de público (#5) e técnicos (#4) não são necessariamente contraditórias: fizeram perguntas/pontos de vista diferentes.

## Limitações metodológicas
- Não foram realizadas **medições objetivas em cruzamentos reais**.
- Não há resultados experimentais demonstrando redução de espera ou emissões.
- Pesquisa exploratória e autorrelatada, com amostras pequenas; **não generalizar percentuais à população de Registro**.
- Formulários originais estão disponíveis com a equipe, mas não foram incorporados a este pacote.

## Objetivo científico consolidado
Desenvolver e avaliar protótipo acadêmico de controle semafórico adaptativo, em um único cruzamento simulado, usando visão computacional para caracterizar demanda e comparando **tempo médio de espera** com temporização fixa.

## Regras de redação
1. Distinguir *já foi implementado* de *será avaliado*.
2. Resultados da pesquisa de percepção são dados de campo, **não medições de eficiência**.
3. Não inserir números de mAP, FPS, latência, economia de combustível, redução de espera ou emissões sem ensaios.
4. ODS 9 e ODS 11 são **alinhamentos**, não impactos medidos.
5. Escrever `YOLOv8n` ao especificar modelo, se comprovado pelo script e arquivo de pesos; evitar prometer inferência totalmente em Edge ou AWS antes de decisão.
6. Não tratar detecção de pessoas como detecção validada de condição de deficiência.
7. Auditar fontes ONU/CNT/IEEE/MDPI e chaves BibTeX antes da publicação.

## Componentes do artigo anterior (BUBATAG) aproveitáveis como referência de forma
- Contextualização progressiva (global→Brasil→região/local); levantamento de campo; Estado da Arte comparativo; metodologia documentada com arquitetura e artefatos.
- O BUBATAG é **referência editorial**, não evidência sobre SmartTraffic.
