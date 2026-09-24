# Flujo de Ejecución y Gestión del Sistema

El ciclo de vida del SoC se divide en 4 fases principales que garantizan la estabilidad y respuesta del sistema en tiempo real.

## Fase 1: Arranque y Autotest Liviano
1. **Encendido:** Carga del Bootloader desde la BRAM interna.
2. **Inicialización:** Se activan la UART y el Bus CSR.
3. **Autotest:** Comprobación rápida de lectura/escritura en SPI-RAM y SPI-Flash.
   * *Fallo Crítico:* Si el hardware principal falla, se levanta una rutina de error en BRAM y log por UART.
   * *Advertencia:* Si el checksum del menú externo falla, carga un menú de emergencia desde BRAM y continúa.

## Fase 2: Gestión de Periféricos por Bus CSR
* **Identificación Dinámica:** El SoC escanea los puertos de entrada mediante el registro `ID CSR`.
* Puede identificar qué tipo de control se ha conectado: Teclado/Mouse PS2 (mediante envío de comando `0xF2`) o Mando Clásico NES.
* Cada puerto posee un LED indicador gestionado por la FPGA para advertir al usuario del reconocimiento del dispositivo.

## Fase 3: Navegación e Inactividad (UI)
* Se despliegan opciones: *Single-player, Multi-Local, Multi-Link*.
* **Timer_Idle:** Si el sistema supera los 30 segundos sin registrar interrupciones (inputs) desde los puertos, ingresa a un *Modo Demo/Animación* hasta recibir una nueva señal.

## Fase 4: Ejecución y "Hot-Plugging"
* El binario del juego se transfiere a la memoria RAM y comienza la ejecución.
* **Sistema de Pausa Automática (Hot-Plugging):** Si el registro CSR detecta una desconexión física del control durante el juego, el hardware emite una interrupción. El sistema pausa inmediatamente la ejecución, notifica en pantalla y espera a que el mando sea reconectado o se fuerce la salida al menú.
