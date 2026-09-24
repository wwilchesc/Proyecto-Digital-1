# Diagrama de Flujo del Sistema

El siguiente diagrama ilustra las 4 fases de ejecución del hardware y firmware del sistema, desde el encendido hasta la ejecución y gestión de periféricos (Hot-Plugging).

```mermaid
graph TD
    %% Fase 1: Arranque
    Start([Encendido del Sistema]) --> BRAM[Carga Bootloader desde BRAM]
    BRAM --> Init[Inicializar UART y Bus CSR]
    Init --> Test[Autotest: SPI-RAM y SPI-Flash]
    Test --> Check{Estado del Hardware}
    
    Check -- Fallo Crítico --> Err[Rutina Error BRAM + UART Log]
    Check -- Checksum Inválido --> Warn[Menú Emergencia BRAM]
    Check -- OK --> F2
    Warn --> F2

    %% Fase 2: Periféricos
    F2[Fase 2: Gestión de Periféricos] --> Scan[Escaneo ID CSR]
    Scan --> Type{Detección de Control}
    Type -- Comando 0xF2 --> PS2[Controlador PS2]
    Type -- Pin Presencia --> NES[Controlador NES]
    PS2 --> LED[Activar LED Indicador de Puerto]
    NES --> LED
    
    %% Fase 3: Menú UI
    LED --> UI[Fase 3: Interfaz y Selección]
    UI --> Idle{¿Inactividad > 30s?}
    Idle -- Sí --> Demo[Modo Demo / Animación]
    Demo --> UI
    Idle -- No --> Input[Esperar Input de Usuario]
    Input --> Val{¿Controles Validados?}
    Val -- No --> UI
    Val -- Sí --> F4
    
    %% Fase 4: Ejecución
    F4[Fase 4: Ejecución del Juego] --> RAM[Transferir Binario de Flash a RAM]
    RAM --> Play[Ejecución del juego en RAM]
    Play --> CSR_Check{Monitoreo CSR Constante}
    
    CSR_Check -- Desconexión de Mando --> Pause[Interrupción por Hardware: Pausa]
    CSR_Check -- OK --> Play
    
    Pause --> Wait{¿Mando Reconectado?}
    Wait -- Sí --> Play
    Wait -- Salida forzada al menú --> UI

