# Roadmap SmartTraffic — prompt mestre de implementação

Use este documento como contexto e plano de trabalho para evoluir o SmartTraffic até uma demonstração ponta a ponta na maquete. Trabalhe por etapas, mantendo cada entrega pequena, verificável e compatível com o código que existe neste workspace.

## Objetivo final

Construir um protótipo acadêmico para um cruzamento com duas vias, A e B, e dois semáforos simulados:

1. Duas webcams capturam simultaneamente as vias A e B.
2. Um programa Python local processa cada câmera e estima a demanda de cada via.
3. Um controlador local decide as fases adaptativamente e envia comandos ao Arduino pela serial.
4. Leituras de tráfego, estados informados, eventos e saúde dos dispositivos são enviados à API e persistidos no MySQL.
5. A plataforma SmartTraffic e um aplicativo mobile consultam os dados reais pela API.

O controle físico deve continuar no computador local e no Arduino. A operação dos semáforos não pode depender da conexão com API, banco ou internet. API e banco registram e distribuem informações; não decidem nem acionam diretamente os LEDs.

## Estado observado no workspace

Antes de implementar cada etapa, reexamine os arquivos atuais. Este resumo descreve a inspeção feita em 05/11/2026 e pode ficar desatualizado.

- `computer-vision/Teste_camera.py` abre câmeras nos índices 0 e 1, mantém streams Flask separados para A e B e mostra ambos em `computer-vision/templates/index.html`. A detecção YOLO, os contadores e as regras de semáforo ainda usam somente a câmera A. A lógica atual conta caixas por quadro; não rastreia veículos únicos nem calcula filas confiáveis.
- `computer-vision/Smartrafic_web.py` é outro servidor, legado e de câmera única. Consolidar e documentar um único ponto de entrada para captura, visão e controle.
- `arduino/arduino_code.ino` recebe `A` ou `B` a 9600 baud, pisca amarelo com `delay()` e alterna os LEDs da maquete. Não há confirmação estruturada de conclusão, watchdog ou protocolo robusto de estado.
- `database/` contém schema MySQL V2.1, seed, Compose, documentação e scripts SQL de validação. O modelo prevê leituras por via, fases, alertas, eventos e telemetria. A existência dos arquivos não comprova banco em execução ou conexão com a API.
- `api/` contém API NestJS/Prisma com autenticação, leitura e escrita de recursos, chave de ingestão e deduplicação de leituras. A rota de leituras aceita dispositivo, via, UUID de evento, timestamp, tipo de leitura e contagens. A integração com Python ainda precisa ser implementada e demonstrada.
- `SmartTraffic-platform/src/services/provider.ts` aponta para `mockProvider`, e a interface descreve dados simulados. Substituir por integração HTTP real com a API e adaptar a interface ao schema de um cruzamento A/B.
- Não há código de aplicativo mobile neste workspace. Implementar o cliente mobile depois de estabilizar os contratos da API.
- `web/` é um site separado da plataforma operacional em `SmartTraffic-platform/`.
- `rag/docs/` contém contexto valioso, mas algumas páginas descrevem arquivos ou integrações que não aparecem nesta cópia local. Trate o código presente no workspace como evidência do estado atual e reconcilie a documentação quando encontrar divergências.

## Arquitetura desejada

```text
Webcam A ─┐
          ├─> aplicação Python local ─> controlador local ─> Arduino ─> semáforos da maquete
Webcam B ─┘               │
                          └─> API NestJS ─> MySQL
                                      ├─> SmartTraffic-platform
                                      └─> aplicativo mobile
```

O stream Flask local pode permanecer como ferramenta de configuração e diagnóstico das câmeras. A plataforma web e o mobile devem obter dados operacionais pela API. Não exponha a chave de ingestão no navegador ou no aplicativo mobile.

## Plano por etapas

### Etapa 1 — Consolidar o ponto de entrada local

- Revisar `Teste_camera.py`, `Smartrafic_web.py`, template e README.
- Escolher um único programa principal e retirar ambiguidades na forma de iniciar o sistema.
- Manter configuração explícita e independente para câmera A e câmera B, incluindo índices, resolução e taxa de captura.
- Capturar frames das duas câmeras de modo independente para que uma câmera lenta ou indisponível não congele a outra.
- Exibir ambas as imagens na página local e comunicar claramente câmera conectada, sem frame ou desconectada.

**Critério de conclusão:** as duas webcams podem ser iniciadas juntas; cada feed atualiza independentemente; o README ensina como identificar e configurar os índices.

### Etapa 2 — Obter medidas independentes por via

- Processar frames da câmera A e da câmera B, mantendo a associação de cada frame à sua via.
- Definir regiões de interesse configuráveis para a área de espera de cada aproximação.
- Separar carros e pessoas por via. Uma detecção de pessoa não deve ser descrita como identificação de pedestre em travessia ou solicitação segura de travessia.
- Definir se a primeira versão do controle usará presença/contagem instantânea, fila estimada ou veículos únicos. Não chamar contagem por quadro de contagem de veículos únicos.
- Se for necessário contar veículos únicos ou estimar ordem de chegada, especificar e implementar rastreamento persistente, tratamento de oclusões e limites conhecidos.
- Representar falha ou baixa confiança como dado indisponível, nunca como contagem zero.

**Critério de conclusão:** a interface local mostra medidas separadas de A e B, carimbo de atualização e estado de cada câmera; a lógica pode distinguir zero real de dado indisponível.

### Etapa 3 — Especificar e implementar controle adaptativo local

- Definir regras antes de codificar: verde mínimo, verde máximo, tempo de amarelo, intervalo de segurança, critério de demanda, desempate, prioridade por espera e comportamento com uma ou ambas as câmeras indisponíveis.
- Evitar alternância rápida com filtragem temporal ou histerese definida e documentada.
- Evitar que uma via fique indefinidamente sem atendimento.
- Manter a decisão local; perda de rede não pode parar nem comandar as fases.
- Evoluir o Arduino para uma máquina de estados explícita, com transições previsíveis, estado seguro de inicialização, confirmação dos comandos e watchdog/fallback apropriado à maquete.
- Evitar `delay()` longo que impeça leitura de comandos e monitoramento durante uma fase.
- Definir claramente quais sinais e estados o controlador consegue confirmar. Um estado registrado no banco não prova o estado elétrico real de um LED.

**Critério de conclusão:** cenários de demanda A>B, B>A, empate, ausência de demanda e falha de câmera têm resultados documentados e repetíveis; não há verde simultâneo nas vias conflitantes nos testes da maquete.

### Etapa 4 — Integrar Python à API e ao banco

- Iniciar o MySQL local usando `database/` e configurar a API conforme `api/README.md` e os arquivos `.env.example`. Nunca reutilizar valores demonstrativos de ambiente como credenciais reais.
- Confirmar a versão do schema e executar os procedimentos de validação documentados em ambiente de teste/local antes de declarar a integração funcional.
- Cadastrar dispositivos e associar cada câmera à via correta; identificar também computador de processamento e Arduino quando apropriado.
- Criar um cliente de ingestão Python para as rotas existentes, começando por leituras de tráfego e telemetria necessária.
- Enviar identificador de evento UUID estável em reenvios, dispositivo, via A/B confiável, timestamp UTC, origem e tipo de leitura coerentes com o schema.
- Preferir leituras agregadas em intervalo definido para evitar gravar cada frame indiscriminadamente. Preservar a semântica de `QUADRO` quando uma amostra instantânea for necessária.
- Implementar timeout, tentativas limitadas, deduplicação e fila local persistente para envio posterior. A fila da API nunca pode bloquear o controle local.
- Enviar somente campos efetivamente medidos. Não inventar fila, veículos únicos, estado do LED ou pessoas em travessia.
- Manter `x-ingestion-key` somente no processo/ambiente confiável do computador, nunca em código de frontend ou mobile.

**Critério de conclusão:** uma leitura produzida por cada via chega ao endpoint de ingestão, é armazenada uma única vez mesmo após reenvio e pode ser consultada pela API; o controle local continua operando sem API.

### Etapa 5 — Conectar a SmartTraffic-platform

- Substituir `mockProvider` por um provider HTTP que implemente os contratos existentes ou revisar os contratos para refletir a API real.
- Adaptar as telas que pressupõem múltiplos cruzamentos e categorias não presentes no modelo de um único cruzamento com vias A/B.
- Integrar login, sessão e permissões usando a API. A autorização precisa ser aplicada no servidor, não apenas escondendo controles na interface.
- Exibir timestamp, via, origem e estado de disponibilidade junto aos dados; distinguir dado ausente de valor zero.
- Consultar leituras, dispositivos, alertas, histórico de fases e resumo sem apresentar estado reportado como confirmação elétrica.
- Configurar CORS e endereço da API por ambiente; não colocar segredos de ingestão no bundle do navegador.

**Critério de conclusão:** a plataforma mostra dados reais consultados pela API e mantém comportamento explícito quando a API está indisponível; nenhuma fixture mockada aparece como dado de operação.

### Etapa 6 — Criar aplicativo mobile

- Definir primeiro as telas e permissões: acompanhamento, histórico, alertas e eventual operação autorizada.
- Reutilizar a API e os mesmos contratos da plataforma, com autenticação adequada ao mobile.
- Nunca incluir a chave de ingestão ou credenciais administrativas no aplicativo.
- Distinguir informações reais, simuladas, desatualizadas e indisponíveis.

**Critério de conclusão:** o aplicativo compila, autentica usuários autorizados e consulta dados reais da API para o cruzamento A/B.

### Etapa 7 — Validar ponta a ponta e documentar evidências

- Validar componentes isolados e depois o fluxo completo: câmeras → Python → Arduino e Python → API → MySQL → plataforma/mobile.
- Preparar cenários reproduzíveis de demanda assimétrica, alternância, desconexão de câmera, falha de rede, indisponibilidade da API e reinício de componentes.
- Registrar versão do código, data, ambiente, configuração, resultado e evidência de cada ensaio.
- Medir a maquete separadamente de resultados SUMO e de dados fictícios. O resultado SUMO documentado no RAG é virtual e não prova o desempenho da maquete.
- Atualizar README e `rag/docs/` apenas após confirmar o comportamento correspondente.

**Critério de conclusão:** há evidência rastreável de que os fluxos funcionam na maquete e nos clientes; as limitações e falhas observadas estão documentadas.

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
