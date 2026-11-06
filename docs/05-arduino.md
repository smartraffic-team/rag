# 05 — Arduino

[Fonte firmware](https://github.com/smartraffic-team/arduino/blob/main/arduino_code.ino). Serial 9600 baud; comandos `A` e `B`, sem protocolo de confirmação estruturada. LEDs da via A: verde 9, amarelo 10, vermelho 11; via B: verde 3, amarelo 5, vermelho 6. Inicia em A verde / B vermelho. Transições com três piscadas amarelas de 1 s (delay 500 ms ligado e 500 ms desligado).

**Limitações:** sem vermelho geral explícito entre fases, temporizador fixo autônomo, watchdog, intertravamento validado ou tratamento robusto de repetição de comando. O estado reportado no MySQL não comprova a condição física do LED. Segurança cabe ao controlador local, não à API.

Uso somente em maquete didática, jamais em vias públicas reais.
