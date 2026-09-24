# Arquitectura de Hardware: Enfoque Modular

## El Concepto del "Cubo"
En lugar de implementar una única arquitectura monolítica multitarea (1 FPGA controlando 4 pantallas simultáneamente), este proyecto ha decidido optar por una **arquitectura distribuida y modular**.

### Justificación Técnica:
1. **Ancho de Banda de Memoria (Cuello de Botella):** Manejar 4 salidas de video simultáneas (ej. VGA o HDMI) desde una sola SPI-RAM o SDRAM saturaría el bus de datos y arruinaría los tiempos de síntesis (*timing closure*).
2. **Tolerancia a Fallos:** Al tener 4 consolas independientes integradas en una estructura física de "Cubo", si un módulo falla, los otros tres continúan operando sin interrupción.
3. **Escalabilidad:** Simplifica el desarrollo del RTL (Hardware Description Language). Solo se debe diseñar y validar el funcionamiento perfecto de **un** sistema individual (un SoC).

## Componentes Críticos del Sistema
* **BRAM (Block RAM):** Se reserva para almacenar el *Bootloader* interno y un menú de emergencia. Garantiza que el sistema siempre inicie, incluso si las memorias externas se corrompen.
* **SPI-Flash:** Almacena los "Assets" (iconos del menú) y los archivos binarios (ROMs) de los juegos.
* **SPI-RAM:** Memoria principal de ejecución rápida donde se transfieren los juegos desde la Flash.
* **Bus CSR:** Eje vertebral del sistema. Permite al procesador leer el estado del hardware y periféricos mapeándolos en direcciones de memoria específicas.
