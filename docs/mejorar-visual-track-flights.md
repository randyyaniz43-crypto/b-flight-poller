# Mejorar la parte visual de Track Flights

Guía para correr Claude Code en la máquina de Randy (Windows, `C:\dev\...`),
sobre la carpeta de la app de vuelos.

Origen: video de @migue.baena (TikTok, ago/2026). La idea del video es que a la IA
no hay que decirle "hazlo bonito": hay que darle **referencias + reglas + revisión**.
Las herramientas de abajo son exactamente esas tres cosas.

Todos los comandos fueron probados antes de escribirlos aquí.

---

## Qué hace cada herramienta

| Herramienta | Papel | Licencia |
|---|---|---|
| `design-taste-frontend`, `high-end-visual-design` (Taste Skill) | Reglas de diseño: evita que todo salga con la misma cara genérica | MIT |
| `redesign-existing-projects` (Taste Skill) | Rediseña una app que YA existe sin romper lo que funciona | MIT |
| `image-to-code` (Taste Skill) | Genera primero la imagen del diseño, después el código que la copia | MIT |
| `web-design-guidelines` (Vercel) | Control de calidad: audita la interfaz y devuelve `archivo:línea` | — |
| `@playwright/cli` (Microsoft) | El agente abre la app en un navegador real y ve cómo quedó | Apache-2.0 |
| `DESIGN.md` (Awesome DESIGN.md) | La referencia: colores, tipografía y espaciado ya resueltos | MIT |

Opcional, **requiere API key y manda contexto a un servidor de terceros**:
21st MCP (`npx @21st-dev/cli@latest init --client claude`, key en <https://21st.dev/mcp>).
No hace falta para nada de lo de abajo.

---

## Lo que hay que pegarle a Claude Code

Abrir una terminal **dentro de la carpeta de la app de vuelos** y pegar esto tal cual:

```text
Estoy en la carpeta de mi app "Track Flights". Quiero mejorar SOLO la parte visual.
No toques la lógica, los datos, ni la conexión con el poller de GitHub Actions.

PASO 1 — Instalá las herramientas de diseño en este proyecto.
Estoy en Windows, así que usá --copy (no symlinks):

  npx -y skills@latest add Leonxlnx/taste-skill -s design-taste-frontend -s redesign-existing-projects -s high-end-visual-design -s image-to-code -y --copy
  npx -y skills@latest add vercel-labs/agent-skills -s web-design-guidelines -y --copy
  npm install -g @playwright/cli@latest
  playwright-cli install --skills

PASO 2 — Bajá la referencia de diseño (paleta, tipografía y espaciado de Linear:
oscuro y denso en datos, que es justo lo que necesita un rastreador de vuelos):

  curl.exe -L -o DESIGN.md https://raw.githubusercontent.com/VoltAgent/awesome-design-md/main/design-md/linear.app/DESIGN.md

PASO 3 — Antes de tocar nada, arrancá la app y sacale capturas con playwright-cli
en dos tamaños: celular 390x844 y computadora 1440x900. Guardalas en diseno/antes/.

PASO 4 — Corré la skill web-design-guidelines sobre los archivos de interfaz.
Dame la lista de problemas en formato archivo:línea, ordenada por gravedad.
Pará acá y mostrámela antes de cambiar nada.

PASO 5 — Rediseño con redesign-existing-projects + el DESIGN.md. Reglas duras:
  - NO cambiar lógica, nombres de funciones, ni cómo se piden los datos.
  - NO tocar el mapa (Leaflet) ni los endpoints /api/.
  - Sólo color, tipografía, espaciado, jerarquía y estados (cargando, vacío, error).
  - Celular primero: si no se ve bien en 390px de ancho, no sirve.

PASO 6 — Capturas después en diseno/despues/, los mismos dos tamaños.
Mostrame antes y después lado a lado.

PASO 7 — NO despliegues nada. Dejá todo en una rama nueva, hacé commit,
y decime en una lista qué cambiaste y por qué.
```

---

## Notas

- **`curl.exe`, no `curl`.** En PowerShell `curl` es un alias de `Invoke-WebRequest` y
  no entiende `-L -o`. Con `.exe` se usa el curl de verdad.
- **`--copy` en Windows.** Sin eso el instalador crea symlinks, que en Windows piden
  modo desarrollador o permisos de administrador.
- **Otros estilos.** Hay 74 referencias en <https://github.com/VoltAgent/awesome-design-md>
  (stripe, raycast, vercel, apple, notion, superhuman…). Se cambia el nombre en la URL
  del PASO 2. Para un tablero de datos, Linear y Raycast son los que mejor pegan.
- **El PASO 4 corta a propósito.** Primero se ve la lista de problemas, después se decide.
  Es la misma regla de siempre: nada se cambia sin la última palabra de Randy.
- Estas skills corren con todos los permisos del agente. Revisar antes de usar.
