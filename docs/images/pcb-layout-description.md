# Descripción del Layout PCB

## Vista Superior (Top Layer)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ ○                          PKE KEYLESS v1.0                          ○  │
│                                                                         │
│  ┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓  │
│  ┃                                                                   ┃  │
│  ┃   ╔═══════════╗                    ╔══════════════════════════╗   ┃  │
│  ┃   ║  POWER    ║                    ║    ESP32-WROOM-32E       ║   ┃  │
│  ┃   ║  SECTION  ║                    ║                          ║   ┃  │
│  ┃   ║           ║                    ║  ┌────────────────────┐  ║   ┃  │
│  ┃   ║ [MP1584]  ║                    ║  │                    │  ║   ┃  │
│  ┃   ║ [L1][C6]  ║                    ║  │   ████████████████ │  ║   ┃  │
│  ┃   ║ [F1]      ║                    ║  │   █ BLE Antenna █ │  ║   ┃  │
│  ┃   ║           ║                    ║  │   ████████████████ │  ║   ┃  │
│  ┃   ╚═══════════╝                    ║  │                    │  ║   ┃  │
│  ┃                                    ║  └────────────────────┘  ║   ┃  │
│  ┃   ╔═══════════════════════════╗    ╚══════════════════════════╝   ┃  │
│  ┃   ║    RELAY SECTION          ║                                   ┃  │
│  ┃   ║                           ║    ╔════════════╗  ╔══════════╗   ┃  │
│  ┃   ║  ┌──┐ ┌──┐ ┌──┐ ┌──┐    ║    ║  USB-C     ║  ║ TJA1050  ║   ┃  │
│  ┃   ║  │1 │ │2 │ │3 │ │4 │    ║    ║ [CP2102N]  ║  ║ CAN Bus  ║   ┃  │
│  ┃   ║  └──┘ └──┘ └──┘ └──┘    ║    ╚════════════╝  ╚══════════╝   ┃  │
│  ┃   ║  ┌──┐ ┌──┐              ║                                    ┃  │
│  ┃   ║  │5 │ │6 │  [ULN2003]   ║    ○LED1  ○LED2   ┌──────┐       ┃  │
│  ┃   ║  └──┘ └──┘              ║                    │BUZZER│       ┃  │
│  ┃   ╚═══════════════════════════╝                    └──────┘       ┃  │
│  ┃                                                                   ┃  │
│  ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛  │
│                                                                         │
│  ═══════════════════════════════════════════════════════════════════════ │
│  CONECTORES (borde inferior):                                           │
│  [J1:PWR] [J2:LOCK] [J2:UNLK] [J3:ACC] [J3:IGN] [J3:START] [J7:CAN]  │
│  [J4:SENSORS]  [J5:BUTTON]                                             │
│                                                                         │
│ ○                                                                    ○  │
└─────────────────────────────────────────────────────────────────────────┘

Dimensiones: 100mm x 70mm
Agujeros de montaje: M3, 4 esquinas (3mm del borde)
Keepout zone: Área de antena ESP32 (sin cobre debajo)
```

## Planos de Cobre

### Top Layer (Capa Superior)
- Señales de datos y control
- Pistas de 0.25mm para señales digitales
- Pistas de 1.0mm para alimentación 3.3V
- Pistas de 2.0mm para corrientes de relés (12V)
- Polígono de relleno: GND

### Bottom Layer (Capa Inferior)
- Plano de tierra completo (GND)
- Rutas de escape para vías
- Zona de exclusión bajo antena BLE
- Pistas CAN Bus (par diferencial, 0.3mm, separadas 0.15mm)

## Consideraciones de Diseño

### EMI / EMC
- Los relés están separados del ESP32 por >15mm
- El ULN2003A actúa como buffer entre ESP32 y relés
- Diodos flyback (D1-D6) suprimen picos de los relés
- Plano de tierra sólido minimiza radiación

### Integridad de Señal BLE
- Sin cobre en los últimos 15mm del ESP32 (zona antena)
- Plano GND tiene cutout bajo la antena
- Componentes de RF alejados de la antena

### Térmico
- Regulador buck con pad térmico grande
- Vías térmicas bajo el pad GND del ESP32
- Área de cobre para disipación en relés de potencia
