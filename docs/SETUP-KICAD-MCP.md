# Guía de Instalación: KiCad + 3 MCPs + Kiro

## Requisitos Previos

- **Sistema Operativo:** Windows 10/11, macOS 12+, o Linux
- **Python:** 3.10 o superior
- **uv:** 0.8+ (gestor de paquetes Python moderno)
- **KiCad:** 9.0 o superior
- **Kiro:** Última versión

---

## Paso 1: Instalar KiCad 9

### Windows
```powershell
# Opción A: Desde la web
# Descargar de: https://www.kicad.org/download/windows/

# Opción B: Con winget
winget install KiCad.KiCad
```

### macOS
```bash
# Con Homebrew
brew install --cask kicad
```

### Linux (Ubuntu/Debian)
```bash
sudo add-apt-repository ppa:kicad/kicad-9.0-releases
sudo apt update
sudo apt install kicad
```

### Verificar instalación
```bash
kicad-cli --version
# Debe mostrar: KiCad 9.x.x
```

---

## Paso 2: Instalar uv (si no lo tienes)

```bash
# Windows (PowerShell)
irm https://astral.sh/uv/install.ps1 | iex

# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Verificar
uv --version
```

---

## Paso 3: Instalar los 3 MCPs de KiCad

### MCP 1: lamaalrajih/kicad-mcp (Lectura/Edición de esquemáticos y PCBs)

```bash
# Clonar el repositorio
git clone https://github.com/lamaalrajih/kicad-mcp.git
cd kicad-mcp

# Instalar con uv
uv pip install -e .

# O con pip
pip install kicad-mcp
```

### MCP 2: mixelpixx/kicad-mcp-server (Control completo, spec 2025-06-18)

```bash
# Clonar
git clone https://github.com/mixelpixx/kicad-mcp-server.git
cd kicad-mcp-server

# Instalar
uv pip install -e .

# O con pip
pip install kicad-mcp-server
```

### MCP 3: kicad-mcp-pro (Esquemático, PCB, DFM, export manufactura)

```bash
# Instalar directamente desde PyPI
pip install kicad-mcp-pro

# Verificar
kicad-mcp-pro --version
```

---

## Paso 4: Configurar los MCPs en Kiro

Crea o edita el archivo de configuración de MCPs en tu workspace.
La ubicación depende de tu sistema:

### Archivo: `.kiro/settings/mcp.json`

```json
{
  "mcpServers": {
    "kicad-mcp": {
      "command": "uv",
      "args": [
        "run",
        "--directory",
        "/ruta/a/kicad-mcp",
        "kicad-mcp"
      ],
      "env": {
        "KICAD_PATH": "/ruta/a/kicad/bin"
      }
    },
    "kicad-mcp-server": {
      "command": "uv",
      "args": [
        "run",
        "--directory",
        "/ruta/a/kicad-mcp-server",
        "kicad-mcp-server"
      ]
    },
    "kicad-mcp-pro": {
      "command": "kicad-mcp-pro",
      "args": ["serve"],
      "env": {
        "KICAD_PROJECT_DIR": "/ruta/a/tu/proyecto"
      }
    }
  }
}
```

### Rutas típicas de KiCad por sistema:

| Sistema | Ruta de KiCad |
|---------|---------------|
| Windows | `C:\Program Files\KiCad\9.0\bin` |
| macOS | `/Applications/KiCad/KiCad.app/Contents/MacOS` |
| Linux | `/usr/bin` o `/usr/local/bin` |

---

## Paso 5: Verificar que Funciona

1. **Abre Kiro** en tu navegador
2. **Abre una sesión nueva** en el workspace donde está `pke-keyless-pcb`
3. **Prueba un comando** como:
   - "Abre el proyecto KiCad en /ruta/a/pke-keyless-pcb/kicad/"
   - "Lista los componentes del esquemático"
   - "Agrega un ESP32-WROOM-32 al esquemático"

Si los MCPs están bien configurados, Kiro tendrá acceso a las
herramientas de KiCad directamente.

---

## Herramientas Disponibles por MCP

### lamaalrajih/kicad-mcp
| Herramienta | Descripción |
|-------------|-------------|
| `read_schematic` | Lee estructura del esquemático |
| `read_pcb` | Lee layout de PCB |
| `run_drc` | Ejecuta Design Rule Check |
| `list_components` | Lista todos los componentes |
| `get_netlist` | Obtiene la netlist completa |
| `export_gerber` | Exporta archivos Gerber |

### mixelpixx/kicad-mcp-server
| Herramienta | Descripción |
|-------------|-------------|
| `create_project` | Crea un nuevo proyecto KiCad |
| `add_component` | Agrega componente al esquemático |
| `connect_pins` | Conecta pines entre componentes |
| `set_footprint` | Asigna footprint a componente |
| `place_component` | Coloca componente en la PCB |
| `route_track` | Rutea pista entre pads |
| `pour_zone` | Crea zona de cobre (plano GND) |

### kicad-mcp-pro
| Herramienta | Descripción |
|-------------|-------------|
| `schematic_create` | Crea esquemático desde netlist |
| `pcb_layout` | Auto-layout de componentes |
| `pcb_route` | Auto-ruteo de pistas |
| `validate_drc` | Validación DRC avanzada |
| `validate_dfm` | Design For Manufacturing check |
| `export_manufacturing` | Genera Gerbers + BOM + CPL |

---

## Troubleshooting

### "KiCad not found"
```bash
# Verificar que kicad-cli está en el PATH
which kicad-cli   # Linux/macOS
where kicad-cli   # Windows

# Si no está, agregar al PATH:
# Windows: Agregar C:\Program Files\KiCad\9.0\bin al PATH del sistema
# macOS/Linux: export PATH=$PATH:/usr/local/bin
```

### "Python version too old"
```bash
python3 --version  # Debe ser 3.10+
# Si es menor, instalar Python 3.12:
# https://www.python.org/downloads/
```

### "MCP no responde"
```bash
# Verificar que el servidor arranca manualmente:
kicad-mcp-pro serve
# Debe mostrar: "MCP server listening on stdio"
```

---

## Siguiente Paso

Una vez tengas los 3 MCPs instalados y configurados en Kiro,
abre una nueva sesión y dime:

> "Tengo KiCad + MCPs instalados. Diseña la PCB completa
> del sistema PKE Keyless Start."

Y yo podré crear el esquemático directamente en tu KiCad local.
