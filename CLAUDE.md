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
  `zonas_espaciamiento` en el modelo) — no duplicarlo. Revisión 17/08: dos fallas
  (1 tramo con longitud → 0 aros; 3er tramo ignorado en el conteo) corregidas en el
  PR #1 de acp-foundation (rama claude/tramos-fixes), **FUSIONADO el 17/08/2026**
  (squash 7ca37e0, PR #1 cerrado), y Randy YA LOS DESPLEGÓ desde su máquina
  (BUILD 259, commit c89fdf2, ~20/08): `main` volvió a ser igual a lo desplegado.
  Verificado en `main` (29/08): están `parseTramos`/`tramosProblema`, el aviso
  «Un solo tramo con longitud no define el resto del fuste» y el `normalizarTipo()`
  del servidor en `analyze-plan.js`. Los dos bugs de aros YA NO EXISTEN: no rehacerlos.
  Ojo: su regresión de tramos (20 casos) corre en Chromium headless contra la página
  real, no con `npm test` — sigue sin extraerse a un módulo.
- Datos del modelo por tipo: cantidad · longitud_colado_pies (pica) · diametro_pulgadas ·
  varilla_longitudinal_numero · varillas_por_pilote · aro_numero ·
  espaciamiento_uniforme_pulgadas XOR zonas_espaciamiento[{espaciamiento_pulgadas,
  longitud_zona_pies?}] · acero_central_numero · elemento_estructural.
  answers[idx]: {L_colado, L_corte, L_fabrica, zona_conf}.
- El análisis de foto (analyze-plan): RANDY MISMO lo pasó a Claude directo en `main`
  (commits 3fc4fd4 + ecbf70b, 17/08): `claude-opus-5` con effort max, prompt propio que
  también lee TABLAS MANUSCRITAS del contratista (filas Qty|Pile|Acero|Aros, "6#6" = 6
  varillas #6, CB = acero central), parser con fences. ai.js también quedó en opus-5 +
  effort max. El PR #1 (mergeado con main, sw v347) solo le SUMA: reglas de tramos en el
  prompt (1er aro = posición, última zona = resto, nunca 1 sola zona) + normalizarTipo()
  servidor. NO volver a reescribir analyze-plan: la base es la de Randy.

## Pull Requests abiertos (foto del 29/08/2026 — VERIFICAR ANTES DE ACTUAR)
- **Esta lista caduca.** La nota de tramos de arriba decía «pendiente de merge» de un PR
  fusionado hacía doce días, y por poco se rehace trabajo ya en producción. Antes de tocar
  nada de lo de abajo: `list_pull_requests` con state=all y mirar `merged_at`. Si aparece
  fusionado, además confirmarlo en el código de `main` — la API dice qué pasó, el código
  dice qué hay.
- **Ninguno toca el build ni el despliegue.** Los del #3 al #7 salen de `main`, son
  independientes y se fusionan en cualquier orden; añaden el mismo bloque `scripts.test`
  a `package.json`, byte a byte idéntico, para que git los fusione sin conflicto.
  El #8, #9 y #10 son una **PILA** y hay que fusionarlos EN ORDEN.
  Ojo: hay un **#4 en cada repo**, son cosas distintas.

| Repo | PR | Base | Qué |
|---|---|---|---|
| acp-foundation | #3 | main | scrypt + límite de intentos + respaldo diario de Blobs + PDF.js |
| acp-foundation | #4 | main | deduplicar 9,9 MB: una sola copia de `polish.*` e `images/` |
| acp-foundation | #5 | main | service worker: tope de red, caché honesta, extintor |
| acp-foundation | #6 | main | reporte de errores desde el teléfono de la cuadrilla |
| acp-foundation | #7 | main | aviso cuando un tramo de aros queda en 0 pies + tests del motor |
| acp-foundation | #8 | main | el estimador de camiones usaba 65 bombazos/yd³ y el resto 63 |
| acp-foundation | #9 | #8 | el factor pasa a ser configurable de verdad en /log/, /gestion/ y /piles/ |
| acp-foundation | #10 | #9 | el agente de IA lee el factor real en vez de uno de memoria |
| b-flight-poller | #4 | main | esta corrección de memoria |

- **EL FACTOR DE CONCRETO (bombazos por yarda) YA NO ES CONSTANTE** — #8, #9 y #10. Era 63
  escrito a mano en once lugares de tres módulos, y el estimador de camiones decía 65. Ahora
  sale de `/acp-bpy.js`: primero `ACPBomb.getTruck().bpy` (lo único que trae lo de la nube),
  después `localStorage['acpBombYd']`, y 63 al final. El motor lo recibe POR PARÁMETRO
  (`runAll(state,{bombazosYd3})`) — NO le metas `localStorage`, rompe el `require()` de Node.
  Decisiones de Randy (29/08): el PDF del cliente SIGUE la calibración, y van los tres módulos.
  Consecuencia que hay que tener presente: por pilote se guarda el bombazo CRUDO y ningún PDF
  se archiva, así que recalibrar reescribe el concreto de obras ya cerradas.

- **Randy tiene que hacer dos cosas a mano** (no las puede hacer una sesión):
  (1) **rotar la contraseña de Pablito** — se sacó del fuente en el PR #3 pero sigue en el
  historial de git, y quitarla del código NO la invalida; (2) poner **`ACP_PW_PEPPER`** en
  las variables de Netlify — sin ella el PR #3 funciona igual, pero el índice de búsqueda
  deja de proteger contra búsqueda offline si alguien se lleva el blob.
- **`sw-kill.js` (PR #5) es la salida de emergencia**, y hay que saber que existe: un
  service worker mal desplegado deja clavados los teléfonos ya instalados y ningún deploy
  los rescata. Se copia sobre `sw.js`, se despliega, y cada teléfono se limpia solo.
- Con los OCHO fusionados hay **137 tests** con `npm test` (`node --test`, cero
  dependencias, sin bundler): 44 · 4 · 17 · 20 · 14 · 4 · 21 · 13. Los de service worker y los del cliente
  de errores ejecutan el archivo REAL dentro de un entorno simulado con `node:vm`, así que no
  pueden divergir de lo desplegado. **Correr `npm test` antes de tocar nada.**
- Los ocho fusionan limpio (los tres últimos en su orden de pila) — comprobado fusionando de verdad
  sobre `main`, no suponiéndolo. Ese chequeo es barato (`git merge` en local) y **hay que
  hacerlo antes de afirmar que no hay conflicto**: en esta sesión se afirmó dos veces sin
  comprobar y una de las dos era falsa.
- **OJO con los PRs de memoria de b-flight-poller: chocan ENTRE ELLOS.** Hay tres abiertos y
  los tres editan este archivo: #2 (`claude/piles-f87lis`, 21/08 — regla PERMANENTE de capas
  House/Pool y fórmulas de rendimiento calibradas sobre 41 días reales), #3
  (`claude/pilotes-exsihv`, 25/08 — alta de 102 Milano Dr.) y #4 (éste). Comprobado
  fusionando de verdad: **#2 choca con #3 y con #4**; #3 y #4 entre sí van limpios. El choque
  es UN SOLO párrafo, el de tramos, que #2 y #4 reescriben los dos. Este #4 ya trae la UNIÓN
  de ambos textos, así que al resolver **quedate con la versión del #4 y no perdés nada**.
  El #2 NO se puede descartar: su regla de capas y sus fórmulas no están en ningún otro lado.
- Sigue abierto y NO es mío: **PR #2 de acp-foundation** (`claude/rendimiento-agente`,
  fórmulas de rendimiento del agente IA), del 21/08, sobre una base anterior al `main`
  actual — puede pedir un merge cuando Randy lo retome.
- **El plan de cinco pasos está completo.** Y ojo con lo que decía esta misma nota antes:
  que faltaba «extraer el cálculo de aros de `piles/index.html` a un módulo». ESO YA ESTABA
  HECHO — el motor vive en `/acp-calc-pilotes.js` (482 líneas, JS puro, sin DOM ni idioma,
  compartido por `/piles/` y `/gestion/`) y ya traía `module.exports`, así que se puede
  `require()` desde Node sin navegador. Lo que faltaba de verdad eran los tests, y los
  añade el PR #7.
- Referencia del motor, verificada ejecutándolo: 125 Guilford Ct. da **31 aros por pilote y
  1302 en la obra**. `L_efectiva = L_colado − punta`; la cabeza NO descuenta aros (en
  `L_corte − punta − cabeza` el `L_corte` se cancela). Si algún día ese 31 cambia, algo se
  rompió.

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
- **CARGADO EN LA APP (17/08/2026, con la última palabra de Randy)**: preguntas del módulo
  respondidas → L_colado 27 · L_corte 30 · L_fabrica 40 · resúmenes. Obra creada en Piles
  (job 42×ACIP Ø16" · 7 #5 · aros #3 en 2 zonas 12'@8"+resto@12" → 31 aros/pilote) y
  proyecto creado en Pile Log (id f3c67b9f-258e-4b1f-86d9-63431313f12d) COMPLETO: jaula,
  plano S-1 subido (blob pilelog-i-4dd83649-6079-4bb6-9aa5-1577c31ebc08, 3001×1473 recorte
  755,655–2365,1445 del PDF estructural a escala 3000/1610) y los 42 pilotes CONFIRMADOS
  con coordenadas de la detección geométrica, colores de la app (#5CB8FF 1P · #FF8A33 2PC ·
  #2EE6A1 3PC) y colorLabels. Falta solo el cliente (Randy en la app). Escritura vía POST /api/sync
  (doc completo: piles `{updatedAt,jobs}` SIN envoltura; log `{updatedAt,data:{projects,symbols}}`),
  header x-acp-write = contraseña de edición que Randy pasa cuando hace falta — NO guardarla
  en esta memoria. Siempre: GET → respaldo → agregar → POST completo → GET de verificación.

## Orden de trabajo acordado (repetir en cada set nuevo)
1. Plano de fundación con los pilotes señalados y clasificados por tipo.
2. Secciones que documentan la cota de la cabeza (detalle + wall section + cuadros GB/cabezales)
   con la cadena de cotas resultante.
3. Tabla/curvas de capacidad calculadas con el Excel de Randy (Meyerhof + Método A).
4. **Hacerle a Randy LAS MISMAS PREGUNTAS del módulo Piles** (su Flow.build real, en orden),
   para que él dé LA ÚLTIMA PALABRA — nunca inventar preguntas propias ni saltarse esto:
   ① «¿Longitud total del COLADO del pilote?» (pies; profundidad real colada en sitio)
   ② «¿A cuántos pies se PICA la varilla longitudinal?» (L_corte; cabeza = L_corte − L_colado)
   ③ «¿Longitud de la varilla de FÁBRICA?» (20/30/40/60)
   ④ «¿Cuántos pies tiene la zona de espaciamiento más cerrado?» — SOLO si hay tramos y la
      zona 1 no declara largo (con largo declarado el módulo la salta)
   ⑤ «¿Qué información necesitas?» (Solo resúmenes / Todo completo).
   Toda pregunta admite «Otro (manual)» — ofrecer el valor ya decidido por Randy como opción.
5. Con la última palabra dada: cargar la obra en el módulo Piles (confirmedTypes + answers)
   y DESPUÉS llevar el trabajo a Pile Log. Nunca cargar sin la última palabra de Randy.
