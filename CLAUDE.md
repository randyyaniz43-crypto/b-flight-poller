# Memoria de trabajo — sesiones de pilotes (Randy · Overseas Builders LLC)

## Acceso permanente concedido (16/08/2026)
- Randy creó una cuenta de GitHub para Claude Code **con todos los permisos**. Repos visibles:
  `randyyaniz43-crypto/b-flight-poller` (este) y `randyyaniz43-crypto/ob-intelligence` (CRM de leads).
- Vía de acceso que funciona: `add_repo` + herramientas MCP de GitHub (`get_file_contents`,
  `push_files`, `search_code`). El `git clone` por Bash puede quedar bloqueado por el
  clasificador de permisos — usar las herramientas MCP en su lugar.

## La app «ACP Pilotes / ACP Foundation» — UBICADA (17/08/2026)
- Repo privado `randyyaniz43-crypto/acp-foundation`, rama `main` — espejo exacto de lo
  desplegado (BUILD 257, commit 94e2f9c). Módulo de pilotes: `piles/index.html` (una sola
  página, ~3.4k líneas). Funciones serverless en `netlify/functions/` (12).
- **REGLAS DE RANDY, no negociables**: (1) NUNCA desplegar a Netlify ni tocar el build —
  el deploy sale sólo de su máquina local. (2) Todo cambio va en rama nueva + Pull Request
  a `main`, nunca commit directo. (3) El cambio de tramos de aros YA ESTÁ HECHO en 94e2f9c
  (campo «Espaciamiento por TRAMOS» en Editar tipo, `parseTramos`/`tramosTexto`,
  `zonas_espaciamiento` en el modelo) — no duplicarlo.
- Datos del modelo por tipo: cantidad · longitud_colado_pies (pica) · diametro_pulgadas ·
  varilla_longitudinal_numero · varillas_por_pilote · aro_numero ·
  espaciamiento_uniforme_pulgadas XOR zonas_espaciamiento[{espaciamiento_pulgadas,
  longitud_zona_pies?}] · acero_central_numero · elemento_estructural.
  answers[idx]: {L_colado, L_corte, L_fabrica, zona_conf}.
- El análisis de foto (analyze-plan) es un PROXY al sitio viejo de ACP Piles
  (ornate-otter-fc386f.netlify.app) — el prompt/schema de extracción vive ALLÁ, no en el repo.

## Proyecto activo: 125 Guilford Ct., Tavernier FL 33070
- Geotecnia: CVIII Engineering Group (Daniel Morao PE 87771), orden 26-0507-G, 07/05/2026.
  Estructura: LINCHENATENG / David G. Osborn AIA, job 2024071, set FOR SUBMITTAL 09/07/2026.
- 42 pilotes ACIP Ø16" · 30 ton/10 ton/2 ton · punta 30'-0" · grout 5000 psi.
  Inventario verificado: 18 aislados (viga 16") · 6×2PC (34") · 12 pil. · 4×3PC (30") · 12 pil.
- Decisiones de Randy para la app (16-17/08/2026):
  - **Pica = 27 ft** (barreno 30 ft; 3 ft sin colar arriba). OJO: 27 vale para 2PC/3PC;
    bajo viga la cabeza queda a 20.5" del terreno → esos 18 piden ~28'-4" de colado.
  - **Un solo registro / un solo tipo de acero** para los 42.
  - Aros en dos zonas: 1er aro a 6" · 12'-0" @ 8" · resto @ 12" (motivo del cambio en la app).
  - Jaula sobresale ~3 ft por encima del colado, dentro del cabezal.
- Libro Excel (plantilla Overseas Builders) ya entregado con: método B (Zelada) ELIMINADO
  a petición de Randy; Meyerhof con dos promedios (N fuste = 27 · N punta = 35, celda B25
  `NTIP`); Método A FHWA GEC-8 intacto. Q_adm 30.7/31.5 ton · tracción 12.7/15.0 ton → CUMPLE.
- Cotas de corte (tope 5.75' NGVD del cuadro de vigas): 4.42' (viga) · 3.25' (3PC) · 2.92' (2PC).
  Losa S-2 a 6.00' — 3" de discrepancia con el cuadro: pendiente de estructural.
  Profundidad bajo terreno NO DETERMINABLE sin topografía (terreno ≈ tope cabezal + 4½").
- Pendientes del expediente: nadie exige prueba de carga (pedir D1143 + D3689); separación
  48" = 3D exacto sin margen; S-8 cita "CIVIL ENGINEERING GROUP" y es CVIII.

## Orden de trabajo acordado (repetir en cada set nuevo)
1. Plano de fundación con los pilotes señalados y clasificados por tipo.
2. Secciones que documentan la cota de la cabeza (detalle + wall section + cuadros GB/cabezales)
   con la cadena de cotas resultante.
3. Tabla/curvas de capacidad calculadas con el Excel de Randy (Meyerhof + Método A).
4. Preguntas de confirmación y llenado de la app ACP Foundation.
