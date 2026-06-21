# Guía de Soldadura - PKE Keyless PCB

## Herramientas Necesarias

| Herramienta | Especificación | Uso |
|-------------|---------------|-----|
| Estación de soldadura | 60W min, temp regulable | Componentes THT y SMD |
| Pistola de aire caliente | 300-400°C | ESP32, QFN packages |
| Multímetro | Básico | Verificaciones |
| Flux en pasta | No-clean, RMA | Facilitar soldadura |
| Mecha desoldadora | 2mm | Correcciones |
| Pinzas ESD | Punta fina | Colocar SMD |
| Lupa o microscopio | 3x-10x | Inspección |
| Alcohol isopropílico | 99% | Limpieza post-soldadura |

## Temperaturas Recomendadas

| Componente | Temperatura cautín | Tiempo máximo |
|-----------|-------------------|---------------|
| Resistencias/Caps 0805 | 320°C | 3s por pad |
| Diodos SOD-123 | 320°C | 3s por pad |
| ICs SOP/SOIC | 340°C | 2s por pin |
| ESP32-WROOM-32 | 300°C aire | 30s total |
| Relés through-hole | 370°C | 5s por pin |
| Conectores KF301 | 370°C | 5s por pin |
| USB-C | 320°C | 2s por pin |

## Layout Visual de Componentes

```
┌─────────────────────────────────────────────────────────────────────┐
│  PKE KEYLESS v1.0                              ┌─────────────────┐  │
│                                                │   ESP32-WROOM   │  │
│  [J1: 12V IN]                                  │     -32E        │  │
│   +12V  GND                                    │                 │  │
│                                                │    ▓▓▓▓▓▓▓▓▓    │  │
│  ┌────────────┐                                │    (antena)     │  │
│  │  MP1584EN  │ L1                             └─────────────────┘  │
│  │  (Buck)    │ ▓▓                                                  │
│  └────────────┘                [SW1]  [SW2]                         │
│                                RESET  BOOT                          │
│  F1(polyfuse)                                                       │
│                                                     [USB-C J6]      │
│  ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐                       │
│  │RL1 │ │RL2 │ │RL3 │ │RL4 │ │RL5 │ │RL6 │       LED1 ● LED2 ●  │
│  │UNLK│ │LOCK│ │ACC │ │IGN │ │STRT│ │SPAR│                        │
│  └────┘ └────┘ └────┘ └────┘ └────┘ └────┘       ┌──────┐        │
│                                                    │ BZ1  │        │
│  [J2: LOCK]  [J2: UNLOCK]                         │Buzzer│        │
│   COM NO NC   COM NO NC                           └──────┘        │
│                                                                     │
│  [J3: ACC]  [J3: IGN]  [J3: START]    [J7: CAN]                   │
│   COM NO NC  COM NO NC  COM NO NC      CANH CANL                  │
│                                                    ┌──────────┐    │
│  [J4: SENSORES]         [J5: BOTÓN]               │ TJA1050  │    │
│  BRK DOR HOD 5V GND     SW+ LED+ LED- GND         └──────────┘    │
│                                                                     │
│  TP1(3V3) TP2(GND) TP3(TX) TP4(RX)                                │
│                                                                     │
│  ●  ●  ●  ●  (Mounting holes M3, 4 esquinas)                      │
└─────────────────────────────────────────────────────────────────────┘
```

## Verificación Funcional Post-Ensamblaje

### Test 1: Alimentación
```
□ Conectar 12V en J1
□ Medir 3.3V en TP1 (debe ser 3.28-3.35V)
□ Medir GND en TP2
□ LED1 debe encender brevemente
□ Corriente en reposo: < 100mA
```

### Test 2: USB y Programación
```
□ Conectar cable USB-C
□ Debe aparecer puerto COM en el PC
□ Presionar BOOT, luego RESET → modo programación
□ Flashear firmware con: pio run --target upload
□ Monitor serial: pio device monitor (115200 baud)
```

### Test 3: Relés
```
□ Desde el monitor serial, activar cada relé individualmente
□ Verificar click audible en cada uno
□ Medir continuidad COM-NO cuando está activo
□ Medir continuidad COM-NC cuando está inactivo
□ Verificar que no hay cortocircuito entre relés
```

### Test 4: BLE
```
□ Abrir app "nRF Connect" en el teléfono
□ Buscar dispositivo "PKE-AutoKey"
□ Verificar que aparece el servicio UUID correcto
□ Conectar y leer las características
```

### Test 5: CAN Bus (si aplica)
```
□ Conectar analizador CAN o segundo ESP32 en J7
□ Verificar que se reciben frames de prueba
□ Medir voltaje diferencial: CANH-CANL ≈ 2V (dominante)
```
