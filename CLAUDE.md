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
  effort max. En main están también (mismo squash 7ca37e0): reglas de tramos en el prompt
  (1er aro = posición, última zona = resto, nunca 1 sola zona) + normalizarTipo() servidor.
  NO volver a reescribir analyze-plan: la base es la de Randy.

## Estado de los PRs (30/08/2026 — FUSIONADO TODO en acp-foundation)
- **`main` de acp-foundation quedó en `b347e28` con LOS NUEVE cambios adentro** y **143 tests**
  en verde (`npm test`, `node --test`, cero dependencias, sin bundler): 44 · 4 · 17 · 20 · 14 ·
  4 · 21 · 13 · 6. Canaria del motor verificada sobre ese `main`: Guilford sigue dando **31 aros
  por pilote, 1302 en la obra y 72.90 yd³**. Si eso cambia, algo se rompió.
- **OJO — LAS PILAS DE PRs SE ROMPEN SOLAS. No volver a apilar.** Pasó el 30/08: los #8→#9→#10→#11
  eran una cadena (cada uno con base en el anterior). Al fusionar el #8, GitHub **borró su rama**,
  y como esa rama era la BASE del #9, GitHub **cerró el #9 solo, sin fusionarlo**. Arrastró al #11
  por lo mismo. Hubo que rehacerlos como **#12 y #13**. Si alguna vez hace falta una cadena: o se
  fusiona de ARRIBA hacia abajo, o se desactiva el borrado automático de ramas, o directamente no
  se apila y se manda todo en un PR sobre `main`.
- **Antes de afirmar NADA sobre `main`, correr `git fetch --prune`.** El 30/08 se dio por perdido
  trabajo que en realidad estaba fusionado, por leer un `origin/main` viejo. La señal estaba a la
  vista y se ignoró: un `git rev-parse origin/<rama>` había fallado por refs incompletas. La regla
  de siempre vale doble acá: la API dice qué pasó, el código dice qué hay — pero sólo si el código
  que mirás es el de verdad.
- **Fusionar NO despliega.** El deploy sale de la máquina de Randy, siempre. Al 30/08 todo esto
  está en `main` y **sin desplegar**, junto con ~48 commits suyos de gestión anteriores.
- **Randy tiene que hacer dos cosas a mano, y ahora URGE porque el PR #3 YA ESTÁ EN `main`:**
  (1) **rotar la contraseña de Pablito** — se sacó del fuente pero sigue en el historial de git, y
  quitarla del código NO la invalida; (2) poner **`ACP_PW_PEPPER`** en las variables de Netlify —
  sin ella funciona igual, pero el índice de búsqueda deja de proteger contra búsqueda offline si
  alguien se lleva el blob.
- **`sw-kill.js` es la salida de emergencia**, y hay que saber que existe: un service worker mal
  desplegado deja clavados los teléfonos ya instalados y ningún deploy los rescata. Se copia sobre
  `sw.js`, se despliega, y cada teléfono se limpia solo.
- **EL FACTOR DE CONCRETO (bombazos por yarda) YA NO ES CONSTANTE.** Era 63 escrito a mano en once
  lugares de tres módulos, y el estimador de camiones decía 65. Ahora sale de `/acp-bpy.js`:
  primero `ACPBomb.getTruck().bpy` (lo único que trae lo de la nube), después
  `localStorage['acpBombYd']`, y 63 al final. El motor lo recibe POR PARÁMETRO
  (`runAll(state,{bombazosYd3})`) — NO le metas `localStorage`, rompe el `require()` de Node.
  Decisiones de Randy (29/08): el PDF del cliente SIGUE la calibración, y van los tres módulos.
  **El TTL de la caché de prompt del agente se deja en 5 minutos A PROPÓSITO**: la de 1 hora cuesta
  2× la escritura y necesita 3 consultas para amortizar contra 2 de la corta, y el patrón de uso de
  Randy NO está medido. Hay un rollup diario de tokens en `ai.js`; se decide mirando `cacheRead`
  contra `cacheWrite5m`, no de memoria.
  Consecuencia que hay que tener presente: por pilote se guarda el bombazo CRUDO y ningún PDF se
  archiva, así que recalibrar reescribe el concreto de obras ya cerradas.
- Sigue abierto y NO es mío: **PR #2 de acp-foundation** (`claude/rendimiento-agente`, fórmulas de
  rendimiento del agente IA), del 21/08, sobre una base anterior — puede pedir un merge.
- **Los tres PRs de memoria de b-flight-poller están RESUELTOS dentro del #4.** Los tres editaban
  este archivo y chocaban: #2 (`claude/piles-f87lis`, 21/08) y #4 reescribían los dos el párrafo de
  tramos; #2 y #3 (`claude/pilotes-exsihv`, 25/08) metían secciones nuevas en el mismo punto de
  inserción. Se fusionaron el #2 y el #3 DENTRO del #4 con merge commits. **Fusionando el #4 entra
  todo, y lo simple es fusionarlo y CERRAR el #2 y el #3.** El #2 y el #3 se dejaron EN BORRADOR a
  propósito: GitHub no deja fusionar un borrador, así que mientras sigan así es imposible caer en
  la única combinación mala (#2 y #3 seguidos sin el #4 en el medio).
  Lección: resolver un choque de este archivo puede MOVER el conflicto a un tercer PR (pasó con el
  #3). Después de resolver, re-verificar TODOS los pares, no sólo el que se tocó.

## Sesión del 08-09/09/2026 — CUATRO PRs, TODOS FUSIONADOS (main: BUILD 344 → 345)
- **PR #14 · los ejes van DEBAJO de los pilotes.** El eje que Randy pone a mano se dibujaba
  encima y le tapaba el círculo y el número, en pantalla Y en el PDF. Pantalla: la capa de ejes
  se partió en dos — las líneas ya puestas en `axis-svg-back` (debajo de `pile-svg`) y la regla
  con las × de borrar arriba, porque son controles y tienen que quedar tocables. PDF: el bloque
  de ejes se movió ANTES del bucle de pilotes. Un solo archivo (`log/index.html`).
- **PR #15 · la conversación de la IA sobrevive al cambio de módulo.** Vivía sólo en memoria
  (`var msgs = []` en el widget y en `/ai/`) y cada salto entre módulos es una navegación
  completa: el chat volvía vacío. Ahora hay UNA conversación compartida, guardada en
  `acp-ai-acciones.js` (un solo dueño, al lado del handoff): fotos afuera SIEMPRE (una revienta
  los ~5 MB y el setItem falla EN SILENCIO), degradación por cuota, leerla no la consume,
  vence a las 12 h, y «Cerrar conversación» la borra. 11 tests nuevos + guard de acoplamiento.
- **PR #15 (mismo) · las 5 preguntas del módulo, DE MARCAR.** La app no tenía NINGÚN mecanismo
  de botones de respuesta: se creó el tipo de acción `pregunta` en `acp-ai-acciones.js` (liviano,
  lo dibuja también el widget) — un botón por opción con su línea chica, más el «Otro (manual)»
  que **pone la app sola** (el manual tiene prohibido mandarlo en las opciones) y deja el foco en
  el teclado. Contestar entra por el MISMO camino que el teclado (`ctx.enviar`, nuevo en los dos
  ctx). Las 5 preguntas quedaron en el manual con el texto EXACTO del `Flow.build`.
  Bug que encontró el test y no la lectura: los botones se apagaban redibujando la tarjeta, pero
  dos toques entran en el mismo tick y salían DOS respuestas → la guarda va en el handler.
  **Se metió en el PR #15 a propósito** (no en uno nuevo): toca los mismos 3 archivos y apilar
  PRs ya se rompió una vez. 13 tests, uno lee las preguntas del `piles/index.html` REAL y las
  compara contra el manual.
- **PR #16 · el cerebro del skill `/Pilotes` adentro del agente.** Archivo NUEVO
  `netlify/functions/ai-knowledge-pilotes.js` (~10 KB), inyectado por `ai.js` como SEGUNDO bloque
  estable ANTES de los DATOS y con UN solo punto de caché al final (la caché es calce de PREFIJO:
  1 breakpoint en vez de 2). Lleva los tres métodos con sus ecuaciones (Meyerhof con `z_req`
  cerrado, FHWA GEC-8 y Zelada capa a capa), FS 2.5/3.0 con su condición, por qué los β subestiman
  en caliza, el encuadre FBC §1810.3.3.1, leer el detalle POR FOTO (la cadena de cotas manda sobre
  la nota de la sección), la cadena del nivel del concreto (se RESTA, nunca se suma), el molde de
  7 campos y el briefing del PM. **Lo que NO se puede portar (Excel, Python, rasterizar PDF) está
  DECLARADO**, no fingido. 14 tests: FS que no se aflojan, los topes de punta que no se mezclan
  (30 tsf Meyerhof vs 45 de A y B) y el acoplamiento — cada campo que el manual manda leer tiene
  que EXISTIR en el snapshot de `ai.js`.
  **Los 3 guards del manual ahora leen LOS DOS archivos** (antes sólo `ai-knowledge.js`, y el
  texto nuevo habría quedado sin vigilancia). El guard de caché se actualizó a la forma de dos
  bloques SIN aflojarlo: además comprueba que los manuales van antes de los DATOS.
- **FUSIONADO TODO el 09/09** a pedido de Randy («haz deploy»): #14, #15, #16 y además el #17.
  `main` quedó en **`8f6c08a`** con **BUILD 345 · log/version.txt 345 · CACHE_NAME v434** y la
  suite **247 en verde — entera, por primera vez**. Fusionar NO despliega: el comando es
  `netlify deploy --prod --dir=.` desde `C:\dev\acp-foundation`, la máquina de Randy (acá no hay
  CLI ni token, y no los pidas: es su regla).
- **PR #17 · el empaquetado del deploy y un test que mentía.**
  (a) Subir el `CACHE_NAME` NO es opcional cuando se tocan los `.js` compartidos: son
  CACHÉ-PRIMERO en el service worker, así que sin eso los teléfonos servirían el
  `acp-ai-acciones.js` viejo mientras el HTML (red primero) ya es el nuevo — las funciones
  nuevas no aparecen hasta la SEGUNDA apertura. Hay un guard nuevo (`test/version-deploy.test.js`)
  que ata los tres números: los 7 `window.BUILD` iguales entre sí, `log/version.txt` igual a ese
  número, `CACHE_NAME` sin retroceder, y que el auto-actualizador siga borrando cachés antes de
  recargar. Verificado que el guard FALLA con un bump a medias.
  (b) **El test de gestión fallaba SEGÚN EL DÍA de la semana y no era el motor.** El ayudante de
  fechas del propio test saltaba SÓLO el domingo mientras la app considera hábil lunes a viernes
  (`habilAuto: w>=1 && w<=5`): un miércoles `habil(3)` caía SÁBADO, el fixture nacía con la tanda
  ahí y la invariante «el fin de semana lo pone Randy, no el motor» la encontraba y culpaba al
  motor —que hacía lo correcto: respetar un finde que ya venía en el documento—. Segunda trampa
  detrás: empujando el finde, `habil(3)` y `habil(5)` caían los DOS en el mismo lunes y el día
  partido se mezclaba con el día gordo. Ahora cuenta DÍAS HÁBILES, verificado para los 7 días.
  **Lección: antes de acusar al código de producción, mirá si el fixture del test es el que
  está mintiendo — y un test que depende del calendario no dice nada el día que se pone rojo.**
- Randy pidió **no seguir ningún PR** (nada de check-ins automáticos): se cancelaron las
  suscripciones. Los PRs quedan para que él los fusione cuando quiera.

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
  con coordenadas de la detección geométrica, TODOS en ROJO #FF4D4D capa «House» (regla de
  Randy, 18/08; jaula también renombrada 'House'). El plano de la app va LIMPIO, sin leyenda
  impresa (Randy la pidió y luego la quitó: la leyenda con profundidades quedó solo como
  imagen entregada en el chat). Falta solo el cliente (Randy en la app). Escritura vía POST /api/sync
  (doc completo: piles `{updatedAt,jobs}` SIN envoltura; log `{updatedAt,data:{projects,symbols}}`),
  header x-acp-write = contraseña de edición que Randy pasa cuando hace falta — NO guardarla
  en esta memoria. Siempre: GET → respaldo → agregar → POST completo → GET de verificación.

## Regla PERMANENTE de capas en Pile Log (Randy, 18/08/2026 — «esto siempre aplica»)
- Pilotes de CASA → capa ROJA `#FF4D4D` con nombre **House**.
- El AZUL `#5CB8FF` queda RESERVADO para la PISCINA → nombre **Pool** (solo si hay piscina).
- Los nombres de capas (colorLabels) van SIEMPRE en inglés (House, Pool, Carport, Deck…).
- OJO técnico: el color de capa se escribe en `manualColor` del pilote (el campo `color` solo
  pinta los 'detected'; un 'confirmed' sin manualColor se dibuja VERDE de estado). Poner
  `color` Y `manualColor` iguales + colorLabels con ese hex.

## Fórmulas de RENDIMIENTO — MANDA LA APP, no esta memoria (revisado 08/09/2026)
Salieron de una regresión sobre el Pile Log real (41 días de hinca medidos en vivo · 21 obras ·
789 pilotes, may–ago 2026), se le pasaron al agente de la app el 21/08 y **Randy después las
recalibró con más datos**. Desde entonces la fuente de verdad es la sección
«# PRONOSTICAR UNA OBRA» de `netlify/functions/ai-knowledge.js` en `main`. **Antes de pronosticar
nada, leer esa sección**: si esta memoria y el manual difieren, gana el manual, y lo que hay que
hacer es corregir ACÁ, nunca agregar una segunda fórmula allá.
Lo que Randy cambió (y por qué importa no volver a las cifras viejas):
- Jornada **6 h / 305 min** para pilotes (acá decía 300 min / 5 h) → pilotes por día
  `n = 305 ÷ (5.7 + 0.41 × pica)`. Prohibido el «20 por día» fijo: depende de la profundidad.
- Concreto del día: **pies × 0.0643** (acá decía 0.065), de 4.052 bombazos por pie medidos sobre
  840 pilotes. Los bombazos por yarda NO son constantes: salen de `bombazosPorYd3` de los DATOS
  (calibración de la bomba, `/acp-bpy.js`), y 63 sólo si no viene.
- Camiones: `yd³ del día ÷ yd3Camion` (**7.5**, el configurado en la app), redondeado HACIA
  ARRIBA y cada día por separado. **NO usar el 5.6 que decía esta memoria**: eso era la carga
  MEDIA observada (7.07 hoy), y meter una media como capacidad ya dio de más una vez. Randy
  eligió 7.5 A PROPÓSITO: 8.5 acierta más días pero se queda corto 9 veces contra 4, y quedarse
  sin concreto es la cuadrilla parada. Bolsillo: 1 camión cada ~115 pies.
- Sigue valiendo lo demás: minutos del día ≈ 55 + 5.7×pilotes + 0.41×pies (±49 min); el Nº de
  pilotes pronostica mejor (r² 0.94) que pies o concreto (0.91); días YA trabajados mandan los
  datos reales del Log (`completadosPorDia`, `camionesPorDia`), no la fórmula; el «Drill Time»
  manual queda ~6 min por debajo del ciclo real; las esperas de camión son 1.2% del tiempo.
- **Jaulas**: el manual tiene su propia regla en GESTIÓN (`horas = aros ÷ 385 × 6 × (6 ÷ personas)`,
  medida en 907 Caroline St). La de esta memoria —min/jaula ≈ 0.96 × (aros + 2×varillas)— salió
  de 7 días de dayWork y es más gruesa: usarla sólo si la de GESTIÓN no aplica, y decir cuál se usó.
  Lo que no cambia: pronosticar por COMPLEJIDAD (aros + varillas), NUNCA por toneladas — el peso
  infla ~2× las jaulas largas.

## El manual del agente tiene TESTS que lo vigilan (08/09/2026)
`npm test` en acp-foundation corre guards sobre `ai-knowledge.js`, y hay que conocerlos antes de
editarlo: (1) **no puede nombrar ninguna versión de modelo** (decía «Opus 4.8» mientras el código
llamaba a otro: si le preguntabas, mentía); (2) **no puede afirmar un factor fijo de bombazos por
yd³** ni los bombazos por camión — tiene que mandar al campo `bombazosPorYd3` de los DATOS;
(3) el glosario del reporte no puede atar el concreto a los 7.5 yd³ del camión. Si se agrega un
manual nuevo en otro archivo, **extender esos guards para que también lo lean**, o el texto nuevo
queda fuera de la vigilancia. Ojo con la sintaxis: es un template literal, hay que escapar los
backticks de los ejemplos y evitar la secuencia dólar-llave.
Para correr la suite hace falta `npm install` primero (sin `node_modules` fallan 12 tests que en
realidad están bien). Al 08/09: **215 en verde**.

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
  + banda de alerta).

### PENDIENTES de Milano Dr. — dónde está frenada, contra el orden de trabajo de abajo
Comparar con Guilford, que está COMPLETO, deja ver lo que falta: allá había geotecnia (CVIII)
y estructural (LINCHENATENG). Acá **sólo hay un Scope of Work**, y de eso cuelga todo.

| Paso | Estado | Qué lo traba |
|---|---|---|
| ① Plano de fundación con los pilotes clasificados | **BLOQUEADO** | no hay planos estructurales |
| ② Cota de la cabeza (cadena de cotas) | **BLOQUEADO** | sin planos ni topografía: NO DETERMINABLE |
| ③ Capacidad con el Excel (Meyerhof + Método A) | **BLOQUEADO** | sin estudio de suelos no hay N-SPT que meterle |
| ④ Las preguntas del módulo Piles | **ESPERA A RANDY** | ver abajo; es lo único que NO depende de terceros |
| ⑤ Cargar en Piles y después Pile Log | **BLOQUEADO por ④** | la regla es no cargar sin su última palabra |

**Hay que PEDIR (no lo tenemos, y sin esto los pasos ① a ③ no arrancan):**
1. **Las notas al pie del Scope of Work.** El escaneo llega cortado por abajo y faltan la *, la
   ** y la ***, que son justamente las que califican «17 ft», «as specified at the above
   mentioned location» y «or Refusal». Sin ellas no sabemos qué manda la longitud.
2. **El estudio de suelos** (boring log / SPT). Sin él no hay capacidad, ni profundidad de punta
   justificada, ni forma de contrastar el «or Refusal».
3. **Los planos estructurales** (fundación + secciones + cuadro de cabezales).

**Cuando Randy esté disponible, preguntarle SOLO esto** (es el Flow.build real; ya está
verificado cuál no aplica):
- ① «¿Longitud total del COLADO del pilote?» — **no la sabemos.** El contrato dice 17 ft *or
  Refusal*, o sea que la profundidad real la define el rechazo en obra, no el papel.
- ② «¿A cuántos pies se PICA la varilla longitudinal?» — no la sabemos.
- ③ «¿Longitud de la varilla de FÁBRICA?» (20/30/40/60) — no la sabemos.
- ④ **NO APLICA.** El módulo la salta: los aros son **@ 10" o.c. UNIFORMES**, sin tramos ni zona
  de confinamiento declarada, así que no hay «zona más cerrada» que preguntar.
- ⑤ «¿Qué información necesitas?» (Solo resúmenes / Todo completo).

**TRAMPA, no repetirla:** NO copiar los números de Guilford. Allá la pica es 27 ft porque el
barreno es de 30 y quedan 3 ft sin colar arriba — eso es de ESA obra. Acá no hay barreno
declarado ni nada que diga que sobra lo mismo. Ni siquiera se puede adelantar el conteo de aros:
el motor necesita `L_colado`, y sin ese dato el número de aros de Milano NO es calculable.
Cuánto importa, corriendo el motor de verdad con los datos del contrato (Ø16", 6 #6, aros #3
@ 10" uniformes, 21 pilotes): con `L_colado` 17 ft dan **17 aros/pilote y 357 en la obra**; con
20 ft, 441; con 25 ft, 567. Un 24% de diferencia entre 17 y 20 pies. Ese 357 es lo que daría el
17 ft NOMINAL del papel y **no es un resultado para usar** — el contrato dice «or Refusal», así
que la profundidad la define el rechazo en obra. Es sólo para que se vea que adivinar acá se
paga caro. (De paso queda verificado que el motor sí toma `espaciamiento_uniforme_pulgadas`,
que es lo que confirma que la pregunta ④ no aplica.)

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
