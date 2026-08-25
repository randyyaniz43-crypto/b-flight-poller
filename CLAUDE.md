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
  PR #1 de acp-foundation (rama claude/tramos-fixes) — pendiente de merge de Randy.
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

## Proyecto nuevo: 102 Milano Dr., Islamorada FL 33036 (25/08/2026)
- Contrato/Scope of Work de ACP Foundation Works LLC. Owner: Mr. David Engel,
  102 Milano Dr, Islamorada FL 33036 · (305) 304-4501 · homeinspectorinthekeys@gmail.com.
  Project Name: «102 Milano add. auger Piles».
- Alcance leido del escaneo (verificado a zoom): **21 pilotes auger cast Ø16"**,
  **17 ft «or Refusal»** de longitud, **6 varillas #6**, **aros #3 @ 10" o.c. uniformes**
  (sin zona de confinamiento declarada), **grout 5000 psi**.
- El escaneo llega CORTADO por abajo: faltan las notas al pie *, ** y *** que califican
  «17 ft», «as specified at the above mentioned location» y «or Refusal». Pedirlas.
- NO hay estudio de suelos ni planos estructurales en mano → cota de corte, nivel del
  concreto, recubrimiento, traslape, separadores y largo de jaula quedan NO DETERMINABLES.
- Ficha-imagen entregada: `Ficha_Pilotes_102_MILANO_DR.png` (molde de 7 campos + complemento
  + banda de alerta). Pendiente: la ULTIMA PALABRA de Randy en las preguntas del modulo
  Piles (L_colado / L_corte / L_fabrica) antes de cargar la obra en la app.

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
