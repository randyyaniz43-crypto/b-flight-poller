# Memoria de trabajo — sesiones de pilotes (Randy · Overseas Builders LLC)

## Acceso permanente concedido (16/08/2026)
- Randy creó una cuenta de GitHub para Claude Code **con todos los permisos**. Repos visibles:
  `randyyaniz43-crypto/b-flight-poller` (este) y `randyyaniz43-crypto/ob-intelligence` (CRM de leads).
- Vía de acceso que funciona: `add_repo` + herramientas MCP de GitHub (`get_file_contents`,
  `push_files`, `search_code`). El `git clone` por Bash puede quedar bloqueado por el
  clasificador de permisos — usar las herramientas MCP en su lugar.

## La app «ACP Pilotes / ACP Foundation» — PENDIENTE DE UBICAR
- Es la PWA donde Randy carga cada tipo de pilote. Formulario de 7 campos:
  Cantidad de pilotes · Pica (profundidad del colado) · Diámetro · Varilla longitudinal ·
  Cantidad por pilote · Aro/cerco · Espaciamiento de aros.
- **NO está en GitHub**: verificado en los 2 repos de la cuenta (código y nombres), y tampoco
  es un artifact publicado. El README de ob-intelligence la cita sólo como patrón
  («PWA pura — mismo patrón que ACP Pilotes» → Netlify, JSON estáticos, localStorage).
  Hipótesis: fuente en la máquina de Randy, deploy por drag-drop a Netlify.
- **Cambio solicitado por Randy (16/08/2026), pendiente de ejecutar cuando aparezca la fuente**:
  el campo «Espaciamiento de aros» debe pasar de valor único a **lista de tramos**
  (cada tramo: longitud + separación), retrocompatible con registros de un solo valor.

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
