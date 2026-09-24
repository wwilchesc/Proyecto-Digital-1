# Proyecto Digital 1: Consola Retro Modular (SoC basado en FPGA)

## 1. Descripción del Proyecto
Este repositorio contiene el diseño, hardware y firmware de una consola de videojuegos retro modular basada en FPGA. Se implementa una **arquitectura tipo Cubo**, donde cada módulo de procesamiento/pantalla es independiente, eliminando cuellos de botella en la memoria de video y maximizando el rendimiento por jugador.

## 2. Organización del Repositorio
* `cores/`: Diseños de los núcleos IP y procesador principal (ej. RISC-V).
* `docs/`: Documentación de arquitectura, diagramas de flujo y manuales.
* `firmware/include/`: Código fuente C/C++ (Bootloader, SO y drivers CSR).
* `interfaces/`: Controladores de periféricos (SPI, UART, PS2, NES Controller).
* `soc/`: Integración del *System on Chip* (Top level).
* `tools/`: Scripts de automatización (Python/Bash) para formateo de ROMs y assets.

## 3. Secciones y Decisiones de Diseño (Documentación)
Las decisiones arquitectónicas tomadas para este proyecto se encuentran detalladas en la carpeta de documentación:
* [Arquitectura de Hardware y Concepto Modular](docs/Arquitectura.md)
* [Fases de Ejecución, Bootloader y Hot-Plugging](docs/Fases_Sistema.md)
* [Diagrama de Flujo del Sistema](docs/Diagrama_de_Flujo.md)

## 4. Designación de Tareas por Grupos
El proyecto se divide equitativamente en 6 ejes de desarrollo. Cada grupo de 3 integrantes asume la responsabilidad de un componente crítico:

**Grupo 1: Núcleo del SoC y Arquitectura Base (Hardware)**
* 1. Instanciar e integrar el procesador Soft-core (ej. RISC-V) en el proyecto (Top Level).
* 2. Definir y rutar el mapa de memoria para el bus de registros CSR.
* 3. Gestionar la lógica de reloj (Clocks) y los sistemas de Reset.

**Grupo 2: Bootloader y Controladores de Memoria (Hardware/Firmware)**
* 1. Instanciar la BRAM y los controladores SPI para la memoria RAM y Flash (HDL).
* 2. Desarrollar el programa Bootloader principal (C / Ensamblador).
* 3. Programar las rutinas de autotest en el arranque y los reportes de error por UART.

**Grupo 3: Subsistema de Video y Gráficos (Hardware)**
* 1. Desarrollar el controlador de sincronización de video (VGA o HDMI en VHDL/Verilog).
* 2. Implementar el motor gráfico o gestor de Framebuffer para dibujar gráficos en pantalla.
* 3. Sincronizar la lectura de video con la memoria SPI-RAM evitando cuellos de botella.

**Grupo 4: Periféricos y "Hot-Plugging" (Hardware)**
* 1. Desarrollar el módulo de hardware para decodificar el protocolo de los mandos NES.
* 2. Desarrollar el módulo para el teclado/ratón PS2 y el sistema de LED indicadores de puerto.
* 3. Implementar el sistema de interrupciones (IRQ) que avise al procesador cuando un mando se desconecte físicamente.

**Grupo 5: Interfaz de Usuario (UI) y Menús (Firmware)**
* 1. Programar el renderizado del menú en pantalla (opciones de selección de juego y modos).
* 2. Implementar la lógica del `Timer_Idle` y el algoritmo que lanza el *Modo Demo* tras 30 segundos de inactividad.
* 3. Validar las entradas de control (evitar que un usuario inicie un juego si no hay controles conectados).

**Grupo 6: Integración, Juegos y Herramientas (Software)**
* 1. Escribir la rutina de software que transfiere el binario del juego de la Flash a la RAM de ejecución.
* 2. Implementar la lógica del "Menú de Pausa" que reacciona a las interrupciones (IRQ) del Grupo 4.
* 3. Desarrollar scripts de Python (`tools/`) para automatizar el formateo de los ROMs y assets gráficos que irán grabados en la memoria Flash.

## 5. Mapa de Memoria del Sistema (Preliminar)
El sistema gestiona la comunicación mediante un bus de registros **CSR (Control and Status Registers)**. 
*(Esta sección se irá actualizando conforme avance el desarrollo RTL).*
