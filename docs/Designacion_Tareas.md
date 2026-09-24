# Designacion de Tareas por Grupos

El proyecto de la consola FPGA modular se divide equitativamente en 6 ejes de desarrollo. Cada grupo (conformado por 3 integrantes) asume la responsabilidad de un componente critico del hardware o firmware.

## Grupo 1: Nucleo del SoC y Arquitectura Base (Hardware)
* **Responsabilidad:** Integracion del procesador y gestion de los buses principales.
* **Tareas individuales (3):**
  1. Instanciar e integrar el procesador Soft-core (ej. RISC-V) en el proyecto (Top Level).
  2. Definir y rutar el mapa de memoria para el bus de registros CSR.
  3. Gestionar la logica de reloj (Clocks) y los sistemas de Reset.

## Grupo 2: Bootloader y Controladores de Memoria (Hardware/Firmware)
* **Responsabilidad:** Ejecucion de la Fase 1 (Arranque y Autotest).
* **Tareas individuales (3):**
  1. Instanciar la BRAM y los controladores SPI para la memoria RAM y Flash (HDL).
  2. Desarrollar el programa Bootloader principal (C / Ensamblador).
  3. Programar las rutinas de autotest en el arranque y los reportes de error por UART.

## Grupo 3: Subsistema de Video y Graficos (Hardware)
* **Responsabilidad:** Salida de video a la pantalla del modulo.
* **Tareas individuales (3):**
  1. Desarrollar el controlador de sincronizacion de video (VGA o HDMI en VHDL/Verilog).
  2. Implementar el motor grafico o gestor de Framebuffer para dibujar graficos en pantalla.
  3. Sincronizar la lectura de video con la memoria SPI-RAM evitando cuellos de botella.

## Grupo 4: Perifericos y "Hot-Plugging" (Hardware)
* **Responsabilidad:** Fase 2 y Fase 4 (Deteccion de entradas y desconexion).
* **Tareas individuales (3):**
  1. Desarrollar el modulo de hardware para decodificar el protocolo de los mandos NES.
  2. Desarrollar el modulo para el teclado/raton PS2 y el sistema de LED indicadores de puerto.
  3. Implementar el sistema de interrupciones (IRQ) que avise al procesador cuando un mando se desconecte fisicamente.

## Grupo 5: Interfaz de Usuario (UI) y Menus (Firmware)
* **Responsabilidad:** Fase 3 (Logica de software del menu principal).
* **Tareas individuales (3):**
  1. Programar el renderizado del menu en pantalla (opciones de seleccion de juego y modos).
  2. Implementar la logica del `Timer_Idle` y el algoritmo que lanza el *Modo Demo* tras 30 segundos de inactividad.
  3. Validar las entradas de control (evitar que un usuario inicie un juego si no hay controles conectados).

## Grupo 6: Integracion, Juegos y Herramientas (Software)
* **Responsabilidad:** Fase 4 (Carga final) y empaquetado del proyecto.
* **Tareas individuales (3):**
  1. Escribir la rutina de software que transfiere el binario del juego de la Flash a la RAM de ejecucion.
  2. Implementar la logica del "Menu de Pausa" que reacciona a las interrupciones (IRQ) del Grupo 4.
  3. Desarrollar scripts de Python (`tools/`) para automatizar el formateo de los ROMs y assets graficos que iran grabados en la memoria Flash.
