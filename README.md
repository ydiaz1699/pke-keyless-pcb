# PKE Keyless PCB - Placa Profesional

PCB personalizada para el sistema **PKE + Keyless Start** con ESP32.
Reemplaza los cables Dupont y protoboard con una placa de grado automotriz.

## Especificaciones de la Placa

| Parámetro | Valor |
|-----------|-------|
| Dimensiones | 100mm x 70mm |
| Capas | 2 (Top + Bottom) |
| Espesor | 1.6mm FR4 |
| Cobre | 2oz (70μm) - para corrientes de relés |
| Máscara | Negro mate (estética automotriz) |
| Serigrafía | Blanca |
| Acabado | HASL lead-free |
| Temperatura | -40°C a +85°C |
| Voltaje entrada | 9-16V DC (batería auto) |

## Componentes Integrados en la PCB

```
┌─────────────────────────────────────────────────────────────────┐
│                    PKE KEYLESS v1.0                               │
│  ┌────────────────┐                                             │
│  │ ESP32-WROOM-32 │  ← MCU principal (BLE integrado)            │
│  └────────────────┘                                             │
│                                                                  │
│  ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐                    │
│  │RL1 │ │RL2 │ │RL3 │ │RL4 │ │RL5 │ │RL6 │  ← 6 Relés 12V/10A│
│  └────┘ └────┘ └────┘ └────┘ └────┘ └────┘                    │
│   UNLK   LOCK   ACC    IGN   START  SPARE                      │
│                                                                  │
│  ┌──────────┐  ┌─────────┐  ┌──────┐  ┌────────┐              │
│  │ TJA1050  │  │ LM2596  │  │ USB-C│  │ BUZZER │              │
│  │ CAN Bus  │  │ 12V→3.3V│  │ Prog │  │        │              │
│  └──────────┘  └─────────┘  └──────┘  └────────┘              │
│                                                                  │
│  [J1-POWER] [J2-LOCKS] [J3-IGNITION] [J4-SENSORS] [J5-BUTTON]  │
│   Conectores automotrices de tornillo                            │
└─────────────────────────────────────────────────────────────────┘
```

## Estructura del Repositorio

```
pke-keyless-pcb/
├── kicad/
│   ├── pke-keyless.kicad_pro     # Proyecto KiCad 8
│   ├── pke-keyless.kicad_sch     # Esquemático completo
│   ├── pke-keyless.kicad_pcb     # Layout PCB
│   ├── symbols/                   # Símbolos personalizados
│   ├── footprints/                # Huellas personalizadas
│   └── 3dmodels/                  # Modelos 3D
├── gerbers/                       # Archivos para fabricación
│   ├── pke-keyless-gerbers.zip   # ZIP listo para JLCPCB/PCBWay
│   └── drill files               # Archivos de taladro
├── production/
│   ├── BOM.csv                    # Bill of Materials
│   ├── CPL.csv                    # Component Placement List
│   └── assembly-notes.md          # Notas para ensamblaje SMT
├── docs/
│   ├── assembly/                  # Guía de soldadura
│   └── images/                    # Renders y fotos
└── README.md
```

## Fabricación

### Opción 1: JLCPCB (China - más barato)
- 5 PCBs: ~$2-5 USD + envío
- Ensamblaje SMT disponible (agregan componentes por ti)
- Tiempo: 7-15 días

### Opción 2: PCBWay (China - mejor calidad)
- 5 PCBs: ~$5-10 USD + envío
- Acabado premium disponible
- Tiempo: 7-15 días

### Opción 3: OSH Park (USA - rápido)
- 3 PCBs: ~$15-20 USD
- Calidad excelente, color morado característico
- Tiempo: 5-10 días

## Licencia

MIT - Hardware Open Source
