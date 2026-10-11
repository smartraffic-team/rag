# 01 — Visão do produto e escopo

## Problema e objetivo
- **Nome oficial:** SmartTraffic (evitar `SmarTraffic` e `Smartrafic` em documentação nova; nomes legados de arquivos podem permanecer até migração).
- **Objetivo principal (DECIDIDO):** reduzir o tempo de espera de veículos no trânsito.
- **Pesquisa:** investigar eficiência e viabilidade técnica de controle semafórico adaptativo em um cruzamento simulado.
- **Entregas acadêmicas declaradas:** MVP e artigo, destinados a evento e publicação científica. Prazo exato não informado.
- **Público do painel administrativo:** funcionários responsáveis pelo trânsito; não é aplicativo para cidadãos.
- **ODS adotados:** ODS 9 (Indústria, Inovação e Infraestrutura) e ODS 11 (Cidades e Comunidades Sustentáveis). Benefícios ainda são objetivos, não resultados comprovados.

## Limites do MVP (DECIDIDO / PLANEJADO)
- **Um único cruzamento** e maquete acadêmica, sem intervenção no trânsito real.
- Veículos **e pedestres**, incluindo atenção à acessibilidade de PCD, como escopo desejado. O mecanismo específico de acessibilidade **não foi definido**.
- Câmera/webcam observa modelos Hot Wheels; notebook executa software Python; Arduino aciona sinalização da maquete.
- Priorizar a via conforme **quantidade de veículos em espera**; algoritmo detalhado ainda não está implementado na forma desejada.
- Tempo de verde **máximo configurável por sinal**. Valor de **19 segundos** citado como padrão provisório, mas ainda não está claro se representa verde padrão, máximo ou configuração inicial.
- Se câmera/software falharem, **retornar a um fluxo padrão**. Exigência registrada, implementação segura de failover ainda pendente.
- Validação: comparar, na maquete, o **tempo de espera** com temporização fixa versus adaptativa.

## Fora do escopo confirmado
- Coordenação operacional de diversos cruzamentos.
- Operação em vias públicas reais.
- Reconhecimento de deficiência de uma pessoa a partir de imagem — **não foi demonstrado**, nem definido como abordagem técnica.
- Detecção e alerta médico automático: menção em texto antigo do artigo, tratada como ideia futura, não requisito aprovado.
- Redução quantificada de emissões, gasto de combustível ou acidentes — **não medida**.

## Vocabulário
- **Detecção por quadro:** número de objetos detectados em uma imagem específica; não significa veículos únicos.
- **Rastreamento:** manutenção de identidade do objeto ao longo do vídeo.
- **Contagem por via/fila:** veículos em região de interesse específica aguardando passagem; ainda precisa especificação.
- **Controle fixo:** algoritmo que alterna fases segundo tempos predefinidos.
- **Controle adaptativo:** algoritmo que altera decisões/tempos conforme demanda e regras de segurança.
- **Edge:** parte do processamento próxima à câmera/controlador.
- **Nuvem:** serviços externos para histórico, administração e eventual processamento; divisão pendente.
