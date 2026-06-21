# Notas de Ensamblaje - PKE Keyless PCB v1.0

## Orden de Soldadura Recomendado

Soldar de menor a mayor altura para facilitar el proceso:

### Paso 1: Componentes SMD (Cara Superior)
1. Resistencias (R1-R14) - 0805
2. Capacitores cerámicos (C2-C8) - 0805
3. Diodos (D1-D7) - SOD-123
4. LEDs (LED1, LED2) - 0805
5. IC regulador (U6 MP1584EN) - SOT-23-5
6. IC CAN (U4 TJA1050) - SOP-8
7. IC USB-UART (U3 CP2102N) - QFN-28
8. IC Darlington (U5 ULN2003A) - SOP-16
9. Inductor (L1) - CD54
10. Polyfuse (F1) - 1812

### Paso 2: Componentes Through-Hole
11. Capacitor electrolítico (C1 100μF)
12. Botones RESET y BOOT (SW1, SW2)
13. Buzzer (BZ1)
14. Conectores de tornillo (J1-J3, J7) - KF301
15. Conectores JST (J4, J5)
16. Conector USB-C (J6)

### Paso 3: Módulo ESP32
17. ESP32-WROOM-32E (U1) - Soldar con pistola de calor o
    estación de soldadura con punta ancha. Asegurar pad GND central.

### Paso 4: Relés
18. Relés (RL1-RL6) - Through-hole. Verificar orientación.

## Verificaciones Post-Soldadura

```
ANTES DE CONECTAR ALIMENTACIÓN:

□ Verificar con multímetro que NO hay cortocircuito entre:
  - +12V y GND
  - +3.3V y GND
  - +5V y GND

□ Verificar orientación de:
  - Diodos D1-D7 (banda = cátodo)
  - LEDs (punto verde = ánodo)
  - ESP32 (antena hacia fuera del borde de la placa)
  - Capacitor electrolítico C1 (banda blanca = negativo)

□ Verificar soldaduras del ESP32:
  - Todos los pads laterales deben tener filetes visibles
  - El pad central GND debe estar soldado (usar vía térmica)

□ Verificar que los relés están en los zócalos correctos:
  - RL1 = UNLOCK
  - RL2 = LOCK
  - RL3 = ACC
  - RL4 = IGN
  - RL5 = START
  - RL6 = SPARE
```

## Primera Energización

```
1. Conectar fuente de laboratorio a J1 (empezar con 9V, 0.5A limit)
2. Medir que 3.3V aparece en el pin 3V3 del ESP32
3. Si todo OK, subir a 12V
4. El LED1 debe parpadear brevemente (boot del ESP32)
5. Conectar USB-C y verificar que aparece como puerto COM
6. Flashear el firmware con PlatformIO
7. Verificar comunicación BLE con la app
```

## Notas Especiales

### Pad Térmico del ESP32
El ESP32-WROOM-32 tiene un pad de tierra grande en la parte inferior.
Este pad DEBE estar soldado correctamente para:
- Disipación de calor
- Conexión a tierra RF (rendimiento BLE)
- Estabilidad mecánica

Técnica: Usar estación de aire caliente a 300°C con flux abundante.
O si se usa cautín: calentar desde las vías térmicas en el reverso.

### Antena BLE
La zona de antena del ESP32 (último 1cm del módulo) NO debe tener
cobre, componentes ni plano de tierra debajo. El layout de la PCB
ya respeta esta zona de exclusión (keepout area).

### Voltaje de Entrada
El circuito acepta 9-16V (rango normal de batería automotriz).
El regulador buck MP1584EN tiene protección de sobrevoltaje hasta 28V.
Sin embargo, NO conectar directamente al alternador sin filtro.

### Disipación Térmica
- Los relés no necesitan disipador (operan en pulsos cortos)
- El regulador MP1584EN puede calentar si los 6 relés están activos
  simultáneamente. El plano de cobre actúa como disipador.
- Temperatura máxima de operación: 85°C
