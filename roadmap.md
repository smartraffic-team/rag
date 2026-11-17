# Roadmap SmartTraffic — prompt mestre de implementação

Use este documento como contexto e plano de trabalho para evoluir o SmartTraffic até uma demonstração ponta a ponta na maquete. Trabalhe por etapas, mantendo cada entrega pequena, verificável e compatível com o código que existe neste workspace.

**Prioridade revisada em 12/11/2026:** o funcionamento com uma camera foi validado pelo usuario. A expansao para duas cameras esta autorizada e em andamento; uma captura independente e atribuida a cada via, com processamento, estado e envio de leituras separados.

**Escopo desta etapa:** conferir a captura isolada A/B com indices distintos; exibir os dois streams no Flask e na plataforma; manter contagens, estimativas de espera e falhas separadas; controlar as duas vias pelo mesmo Arduino; enviar leituras de cada camera para a via cadastrada correspondente. Se uma camera falhar, a outra continua capturando e a via sem medicao nao gera demanda ficticia.


## Objetivo final

Construir um protótipo acadêmico para um cruzamento com duas vias, A e B, e dois semáforos simulados:

1. Na primeira entrega, uma webcam observa uma via configurada, inicialmente A. A outra via continua existindo no cruzamento, mas não possui medição por câmera.
2. Um programa Python local conta carros e pessoas visíveis na via observada. Na expansão, duas webcams produzem medidas independentes de A e B.
3. Um controlador local opera os dois semáforos pelo Arduino. Na primeira entrega, adapta o atendimento da via observada e garante atendimento temporizado à via sem câmera. Na expansão, considera a demanda das duas vias.
4. Leituras de tráfego, estados informados, eventos e saúde dos dispositivos são enviados à API e persistidos no MySQL.
5. A plataforma SmartTraffic e um aplicativo mobile consultam os dados reais pela API.

O controle físico deve continuar no computador local e no Arduino. A operação dos semáforos não pode depender da conexão com API, banco ou internet. API e banco registram e distribuem informações; não decidem nem acionam diretamente os LEDs.

**Premissa da primeira entrega:** uma câmera observa somente uma via; não há divisão da imagem em duas vias nesta fase. Não copiar suas contagens para B, não gerar leituras artificiais de zero para a via sem câmera e não interpretar ausência de medição como ausência de veículos. O banco mantém as vias e os dois semáforos cadastrados. Os estados dos dois semáforos podem ser registrados independentemente da cobertura das câmeras.

## Estado observado no workspace

Antes de implementar cada etapa, reexamine os arquivos atuais. Este resumo reúne a inspeção de 05/11/2026 e a revisão de visão computacional e plataforma em 09/11/2026; código existente não comprova execução integrada.

- `computer-vision/smarttraffic_app.py` é a entrada principal de uma câmera, configurável por índice e via observada (A por padrão). `vision.py` separa captura e detecção em threads, delimita uma região de contagem, tenta reconectar a câmera e invalida leituras antigas ou produzidas antes de uma desconexão. Um rastreador simples de centros das caixas estima veículos parados na região configurada; não identifica veículos únicos nem estima comprimento de fila.
- `computer-vision/Smartrafic_web.py` é um atalho para a mesma aplicação, sem abrir outra câmera. A configuração e a execução estão documentadas no README.
- A captura da Logitech BRIO (índice 1, DirectShow) foi validada pelo usuário. A precisão das contagens e os ensaios de falha da maquete ainda precisam de evidências próprias.
- `computer-vision/traffic.py` implementa o controlador local com mediacoes independentes A/B, filtros por via e prioridade automatica para a unica aproximacao com carro parado. O verde da via que cede pode ser 0, sempre passando por amarelo e ambos vermelhos. Falhas de cameras nao viram demanda nem zero.
- `arduino/arduino_code.ino` implementa fases sem `delay()`, amarelo de 3 segundos, ambos vermelhos por 1 segundo, limites de verde inclusive no manual e watchdog de comunicação de 4 segundos. Inicia em ambos vermelhos e passa ao ciclo fixo. O firmware informa fase e aceitação de comandos; não há confirmação elétrica dos LEDs. O novo Python exige esse firmware; o protocolo antigo A/B não é compatível.
- `database/` contém schema MySQL V2.1, seed, Compose, documentação e scripts SQL de validação. O modelo prevê leituras por via, fases, alertas, eventos e telemetria. A existência dos arquivos não comprova banco em execução ou conexão com a API.
- `api/` contém API NestJS/Prisma com autenticação, chave de ingestão e reenvios idempotentes de leituras, fases e telemetria. `computer-vision/ingestion.py` coleta snapshots em thread própria e envia por outra thread, com fila SQLite persistente, timeout e tentativas limitadas. No ambiente local, câmera, computador e Arduino foram cadastrados; dados reais já foram persistidos e consultados pela API em 10/11/2026.
- `SmartTraffic-platform/src/services/provider.ts` já consulta a API por HTTP, incluindo `/leituras`. `src/services/http.ts` trata endereço da API e sessão JWT. Reutilizar essa implementação e validar a exibição das leituras reais da câmera, incluindo a ausência de medição na outra via.
- Não há código de aplicativo mobile neste workspace. Implementar o cliente mobile depois de estabilizar os contratos da API.
- `web/` é um site separado da plataforma operacional em `SmartTraffic-platform/`.
- `rag/docs/` contém contexto valioso, mas algumas páginas descrevem arquivos ou integrações que não aparecem nesta cópia local. Trate o código presente no workspace como evidência do estado atual e reconcilie a documentação quando encontrar divergências.

## Arquitetura da primeira entrega

```text
Webcam da Via A ─> aplicação Python local ─> controlador local ─> Arduino ─> semáforos A/B
                              │
                              └─> API NestJS <─> MySQL
                                       ↑
                              plataforma e mobile consultam a API
```

O stream Flask local pode permanecer como ferramenta de configuração e diagnóstico das câmeras. A plataforma web e o mobile devem obter dados operacionais pela API. Não exponha a chave de ingestão no navegador ou no aplicativo mobile.

A segunda camera esta sendo integrada em uma captura independente por via; indices OpenCV, ROIs, estados e streams devem permanecer separados.

## Plano por etapas

### Etapa 1 — Consolidar a aplicação com uma câmera

- Revisar `smarttraffic_app.py`, `Smartrafic_web.py`, template e README.
- Escolher um único programa principal e retirar ambiguidades na forma de iniciar o sistema.
- Configurar uma câmera ativa: índice, via observada, resolução e taxa de captura. A inicialização não deve exigir uma segunda webcam.
- Separar captura, detecção e controle para que as transições dos semáforos não congelem o vídeo.
- Mostrar um feed na interface local e comunicar câmera conectada, sem frame ou desconectada.
- Preservar a associação entre câmera e via para permitir a expansão posterior, sem implementar agora a operação com dois dispositivos.

**Critério de conclusão:** o sistema inicia com apenas uma webcam conectada, mantém o vídeo durante trocas de sinal, informa falhas e possui uma instrução única de execução no README.

### Etapa 2 — Obter contagens da via observada

- Processar frames da única câmera e associar suas medidas à via configurada.
- Definir uma região de interesse configurável para a área observada.
- Contar carros e pessoas visíveis por quadro. Uma detecção de pessoa não equivale a uma solicitação de travessia.
- Usar contagem instantânea como medida inicial, com horário da captura e validade do dado. Rastreamento, veículos únicos e estimativa de fila ficam para uma evolução específica, caso necessários.
- Representar falha ou baixa confiança como dado indisponível, nunca como contagem zero.

**Critério de conclusão:** a interface local mostra contagens da via observada, carimbo de atualização e estado da câmera; a outra via aparece sem medição. Uma desconexão não produz uma contagem válida de zero.

### Etapa 3 — Controlar os dois semáforos com uma via observada

- Definir regras antes de codificar: verde mínimo e máximo, amarelo, intervalo de segurança, atendimento da via observada, atendimento temporizado da via sem câmera e comportamento quando a única câmera falhar.
- Adaptar o verde da via observada conforme sua contagem válida, respeitando os limites e a alternância. A via sem câmera deve receber atendimento periódico mesmo que não exista informação sobre sua demanda.
- Usar ciclo fixo local quando a câmera estiver indisponível. Documentar que essa versão adapta o controle com observação parcial do cruzamento e ainda não compara filas A/B.
- Evitar alternância rápida com filtragem temporal ou histerese definida e documentada.
- Evitar que uma via fique indefinidamente sem atendimento.
- Manter a decisão local; perda de rede não pode parar nem comandar as fases.
- Evoluir o Arduino para uma máquina de estados explícita, com transições previsíveis, estado seguro de inicialização, confirmação dos comandos e watchdog/fallback apropriado à maquete.
- Evitar `delay()` longo que impeça leitura de comandos e monitoramento durante uma fase.
- Definir claramente quais sinais e estados o controlador consegue confirmar. Um estado registrado no banco não prova o estado elétrico real de um LED.

**Critério de conclusão:** presença, ausência e variação de carros na via observada, falha de câmera e modo manual têm resultados repetíveis; a via sem câmera recebe atendimento periódico e não há verde simultâneo nos ensaios da maquete.

### Etapa 4 — Integrar Python à API e ao banco

- Iniciar o MySQL local usando `database/` e configurar a API conforme `api/README.md` e os arquivos `.env.example`. Nunca reutilizar valores demonstrativos de ambiente como credenciais reais.
- Confirmar a versão do schema e executar os procedimentos de validação documentados em ambiente de teste/local antes de declarar a integração funcional.
- Cadastrar a câmera ativa e associá-la à via observada; identificar também computador de processamento e Arduino quando apropriado. Manter as duas vias e os dois semáforos no banco.
- Criar um cliente de ingestão Python para as rotas existentes, começando por leituras de tráfego e telemetria necessária.
- Enviar identificador de evento UUID estável em reenvios, dispositivo, via A/B confiável, timestamp UTC, origem e tipo de leitura coerentes com o schema.
- Começar com amostras periódicas de `QUADRO`, enviando apenas uma nova medida válida da via observada. Definir a frequência considerando os limites da API; não enviar cada frame nem somar amostras como veículos únicos. Usar `AGREGADA` somente quando houver uma regra de agregação e um período definidos.
- Registrar fases informadas dos dois semáforos e saúde dos dispositivos nas rotas correspondentes. Conferir a política de reenvio de cada recurso: a deduplicação de leituras não garante idempotência de fases ou telemetria.
- Implementar timeout, tentativas limitadas, deduplicação e fila local persistente para envio posterior. A fila da API nunca pode bloquear o controle local.
- Enviar somente campos efetivamente medidos. Não inventar fila, veículos únicos, estado do LED ou pessoas em travessia.
- Manter `x-ingestion-key` somente no processo/ambiente confiável do computador, nunca em código de frontend ou mobile.

**Critério de conclusão:** leituras reais da única câmera chegam ao banco e são consultáveis pela API; reenvios da mesma leitura não duplicam registros; fases dos dois sinais podem ser consultadas; o controle continua sem API. Não são produzidas leituras fictícias para a via sem câmera.

### Etapa 5 — Validar a plataforma com dados reais de uma câmera

- Reutilizar o provider HTTP existente; conferir os contratos e a configuração da API.
- Exibir as contagens da via observada e marcar a outra como sem medição, mantendo visíveis os estados informados dos dois semáforos.
- Validar login, sessão e permissões existentes. A autorização precisa ser aplicada no servidor, não apenas escondendo controles na interface.
- Exibir timestamp, via, origem e estado de disponibilidade junto aos dados; distinguir dado ausente de valor zero.
- Consultar leituras, dispositivos, alertas, histórico de fases e resumo sem apresentar estado reportado como confirmação elétrica.
- Configurar CORS e endereço da API por ambiente; não colocar segredos de ingestão no bundle do navegador.

**Critério de conclusão:** uma mudança de contagem da câmera aparece na plataforma após persistência e consulta à API; dados antigos, indisponibilidade e via sem medição ficam identificados.

### Etapa 6 — Criar aplicativo mobile

- Definir primeiro as telas e permissões: acompanhamento, histórico, alertas e eventual operação autorizada.
- Reutilizar a API e os mesmos contratos da plataforma, com autenticação adequada ao mobile.
- Nunca incluir a chave de ingestão ou credenciais administrativas no aplicativo.
- Distinguir informações reais, simuladas, desatualizadas e indisponíveis.
- Mostrar dados da única via observada e estados dos dois semáforos; identificar a via sem medição.

**Critério de conclusão:** o aplicativo compila, autentica usuários autorizados e consulta dados reais da API para o cruzamento A/B.

### Etapa 7 — Validar a primeira entrega com uma câmera

- Validar componentes isolados e depois o fluxo completo: câmera → Python → Arduino e leituras Python → API → MySQL, consultadas pela plataforma/mobile através da API.
- Preparar cenários reproduzíveis de variação de demanda na via observada, atendimento da via sem câmera, alternância, desconexão de câmera, falha de rede, indisponibilidade da API e reinício de componentes.
- Registrar versão do código, data, ambiente, configuração, resultado e evidência de cada ensaio.
- Medir a maquete separadamente de resultados SUMO e de dados fictícios. O resultado SUMO documentado no RAG é virtual e não prova o desempenho da maquete.
- Atualizar README e `rag/docs/` apenas após confirmar o comportamento correspondente.

**Critério de conclusão:** o sistema completo funciona com somente uma webcam conectada, com evidências do controle dos dois sinais e das leituras reais no banco, na plataforma e no mobile. Este é o marco necessário antes da expansão para duas câmeras.

### Etapa 8 — Ampliar para duas câmeras

- Adicionar a segunda webcam com configuração, captura, região de interesse, contagens e disponibilidade independentes.
- Processar as duas entradas sem compartilhar indevidamente contagens ou IDs de rastreamento, caso este seja introduzido.
- Associar cada câmera ao dispositivo e à via corretos na API, reaproveitando o contrato de leituras.
- Atualizar Flask, plataforma e mobile para apresentar medidas reais separadas de A e B.
- Evoluir o controle para comparar a demanda das duas vias, com critérios de troca, desempate e espera máxima definidos.
- Definir o comportamento quando só uma câmera falhar e quando ambas falharem, aproveitando a política de operação com observação parcial da primeira entrega.

**Critério de conclusão:** contagens independentes A/B percorrem todo o fluxo até os clientes e participam do controle local; falha de uma câmera não interrompe a captura da outra nem gera demanda fictícia.

### Etapa 9 — Validar a expansão e atualizar as evidências

- Executar cenários A>B, B>A, empate, vias vazias e perda de uma ou das duas câmeras.
- Conferir desempenho no computador usado, atribuição correta de dados por via, atendimento de ambas as vias e continuidade durante falhas da API.
- Registrar testes da maquete e comparações entre ciclo fixo e adaptativo com método reproduzível, sem reutilizar métricas do SUMO como resultados físicos.
- Atualizar documentação e limitações da versão com duas câmeras.

**Critério de conclusão:** há evidências rastreáveis da operação com duas câmeras e do fluxo completo, preservando o funcionamento já validado com uma câmera.

## Próxima entrega

A captura e a operação física dos semáforos com os tempos definidos foram validadas pelo usuário. Não inferir desse relato a realização de todos os ensaios de falha do checklist.

A Etapa 4 foi implementada e exercitada no ambiente local em 10/11/2026. Evidências em `computer-vision/VALIDACAO_ETAPA4.md`: 45 testes Python, 4 testes unitários da API, 6 testes de integração com MySQL separado, build/lint, verificação do schema V2.1, leituras reais somente da Via A, fases recebidas do Arduino e recuperação após interrupção da API. A chave fica em arquivo local ignorado pelo Git; a fila mantém os eventos pendentes. Os tempos cadastrados no banco local foram alinhados aos 12/25 segundos da maquete.

A Etapa 5 está implementada no frontend: a Visão geral consulta leituras QUADRO da Via A e últimas leituras de ambas as vias, associa a câmera cadastrada à via, atualiza a tela periodicamente, mostra B sem medição e indica contagens/telemetria/fases desatualizadas. O gráfico apresenta amostras isoladas, sem somar um objeto repetido entre quadros. Falta conferir o login e essas telas no Vite local com a API em execução e observar as próximas atualizações. O aplicativo mobile pertence à Etapa 6, e a segunda câmera à Etapa 8.

## Regras de trabalho para cada etapa

1. Comece inspecionando os arquivos e o estado atual do repositório; não assuma que este roadmap ou o RAG ainda descrevem exatamente a implementação.
2. Informe a etapa escolhida, os arquivos que pretende alterar, o resultado esperado e como será verificado.
3. Implemente somente a etapa autorizada. Não avance automaticamente para a próxima etapa nem reescreva subsistemas não relacionados.
4. Preserve decisões existentes quando forem compatíveis com o objetivo e explique incompatibilidades encontradas antes de mudar contratos de dados.
5. Nunca coloque senhas, chaves, certificados ou tokens em código, logs, screenshots, fixtures públicas ou bundles de cliente.
6. Não confunda detecção por quadro, veículos únicos e estimativa de fila.
7. Não trate pessoas detectadas como pedidos de travessia; não tente inferir deficiência pela aparência.
8. Não descreva resultado SUMO, mock ou estado recebido do Arduino como resultado medido da maquete sem evidência.
9. Ao concluir, informe mudanças, arquivos, validações executadas, limitações e o critério que ainda falta para avançar.

## Fora do escopo

- Controle de semáforos de vias públicas reais. O projeto é uma maquete acadêmica, sem homologação para uso viário.
- Decisão de fases semafóricas pela API, pelo MySQL, pela plataforma ou pelo aplicativo mobile.
- Identificação de pessoas ou inferência de deficiência.
- Apresentar a melhoria percentual do SUMO como ganho físico ou benefício urbano comprovado.
