# 05 — Arduino e sinalização da maquete

**Fonte:** [`arduino/arduino_code.ino`](https://github.com/smartraffic-team/arduino/blob/main/arduino_code.ino), branch `main`, SHA de blob `98272a5c11b12e14fa6d8ada34ded7d9cf4d21d5`.

## Mapeamento encontrado
| Via | Verde | Amarelo | Vermelho |
|---|---:|---:|---:|
| A | Pino 9 | Pino 10 | Pino 11 |
| B | Pino 3 | Pino 5 | Pino 6 |

Serial em **9600 baud**. Em `setup()`, o código chama `setSemaforoA('V')` e `setSemaforoB('M')`, iniciando A em verde e B em vermelho.

## Comandos recebidos
- `A`: executa `piscaAmareloB(3)`, muda B para vermelho e A para verde; escreve informação na serial.
- `B`: executa `piscaAmareloA(3)`, muda A para vermelho e B para verde; escreve informação na serial.
- Funções `setSemaforoA/B` começam desligando as três saídas daquela via e depois ligam apenas a saída correspondente.
- Piscadas são feitas com `delay(500)` ligado + `delay(500)` desligado, três vezes (**3 s no total**).

## Limitações e riscos a corrigir antes de expandir
- **Não há watchdog local autônomo** para detectar perda de comandos/software e iniciar ciclo de contingência.
- **Não há temporizador fixo autônomo** implementado neste firmware.
- **Não há confirmação estruturada de comando** com sequência/ack; há apenas texto impresso na serial.
- **Não existe estado explícito de vermelho geral entre transições**, nem modelo formal de tempos mínimos e permissões por via.
- O código é **bloqueante** por `delay` durante a transição.
- Chamadas A/B recebidas de forma repetitiva podem gerar transições inesperadas e precisam de validação de estado.
- Não foi analisado esquema físico/circuito nesta documentação; não afirmar compatibilidade elétrica, ligação ou proteção sem verificação.
- README menciona arquivo `smarttraffic_semaphore.ino`, mas o arquivo inspecionado se chama `arduino_code.ino`.

## Direção técnica proposta
Criar máquina de estados local que gerencie fases com transições seguras e temporizações configuráveis, watchdog, confirmação de comandos e comportamento de falha definido. **É uma proposta de melhoria**, não uma descrição do estado atual.

> **Atenção:** firmware didático para LEDs em maquete. Não empregar em semáforos de vias reais.
