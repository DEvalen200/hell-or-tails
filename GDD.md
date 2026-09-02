# Hell or Tails — Game Design Document

**Versión:** 0.9 (dificultad calibrada: 5 puntos por piso y por nivel) · **Fecha:** 2026-09-01
**Género:** Aventura / gambling con progresión de economía · **Estilo:** Low poly
**Plataforma:** PC (Unity) · **Modos:** Un jugador y multijugador (2–4)

---

## 1. Concepto

Estás en la ruina absoluta. Un club de apuestas clandestino oculto en un callejón te ofrece una salida: aquí no se juega con dinero, se apuestan **objetos robados** a **cara o cruz**. Roba en la calle a palazos, apuesta lo robado en mesas cada vez más exigentes, gana objetos de mayor valor, véndelos y compra **mejoras para tu moneda** que inclinan la probabilidad a tu favor.

**Fantasía central:** convertir una lata abollada en un maletín de diamantes a base de cara o cruz… o perderlo todo y volver a la calle con el bate.

**Pilares de diseño:**
1. **Tensión pura** — cada lanzamiento puede duplicarte o arruinarte. Sin apuesta máxima.
2. **Bucle rápido** — robar, apostar, vender, mejorar: ciclos de 2–5 minutos.
3. **La suerte se compra** — la probabilidad empieza en 50/50 pero el dinero la corrompe.
4. **Humor negro** — la miseria del protagonista y la estética infernal del club se toman con sorna.

---

## 2. Historia

Te deja tu novia. Te echan del trabajo. Se muere tu perro. Acabas mendigando en la calle.

Una noche, alguien te susurra desde un callejón. Al fondo, un letrero de neón parpadea: **HELL OR TAILS**. Dentro, un tipo te mira y sonríe: *"Se te ve en la cara que estás pasando un mal momento. Tranquilo, estás en el lugar correcto: el mayor club de apuestas clandestino de la ciudad."*

Le dices que no llevas dinero. Se ríe: *"Aquí no se juega con dinero. Esto es un mercado negro — evitamos el dinero para que no nos rastreen y para deshacernos cuanto antes de los objetos robados. Apuestas un objeto para ganar otro."*

Le dices que no tienes nada que apostar. Te pone un **bate** en las manos: *"Pues espabila."*

**Tono:** tragicomedia. El club tiene una estética progresivamente infernal (cuanto más profundo entras, menos parece un club y más parece el infierno), pero nunca se explica del todo si es literal o metáfora.

---

## 3. Bucle de juego (core loop)

```
ROBAR (calle) → APOSTAR (mesas) → GANAR objeto mejor ─┐
      ↑                │                              │
      │           └── PERDER (sin objetos → te echan) ┤
      │                                               ↓
      └───────── COMPRAR mejoras de moneda ← VENDER objetos
```

1. **Robar:** golpea a un NPC con el bate; suelta un objeto según su tipo (loot table).
2. **Apostar:** baja en el ascensor a un piso desbloqueado, coloca tu objeto en la mesa del apostador, elige cara o cruz.
   - **Ganas** → te quedas tu objeto **y** el suyo, y la mesa sigue abierta.
   - **Pierdes** → pierdes el objeto apostado; **solo te echan del club si te quedas sin ningún objeto**.
3. **Vender:** cambia objetos por dinero en la tienda del perista.
4. **Mejorar:** gasta el dinero en mejoras para tu moneda (más probabilidad de acertar) y avanza a mesas más profundas.

**Condición de derrota suave:** nunca hay game over — sin objetos y sin dinero siempre te queda el bate y la calle.
**Objetivo a largo plazo (propuesta):** llegar a la última sala y ganar la apuesta final (ver §7.4).

---

## 4. Mecánicas

### 4.1 El robo

- El jugador lleva siempre el **bate** (no se puede vender ni apostar; regalo del club).
- Golpear a un NPC lo aturde/derriba y suelta **1 objeto** de su loot table. El NPC queda "robado" un tiempo (no vuelve a soltar nada hasta su respawn).
- Los NPC reaccionan: la mayoría huye al ver el bate en alto; algunos corren más, otros gritan (atraen la atención de los demás y espantan el loot de la zona unos segundos).
- **Riesgo (propuesta v2, opcional en MVP):** algunos NPC se defienden o aparece un policía de ronda que te persigue; si te pilla, te confisca un objeto aleatorio.
- Respawn de NPCs: 60–120 s, para que perder en la mesa duela pero no aburra.

### 4.2 Objetos y rareza

Todo objeto tiene **valor de venta** (V) y **tier**. Los tiers marcan qué mesas puedes jugar (apuesta mínima por sala).

| Tier | Nombre | Valor venta (aprox.) | Ejemplos low poly |
|------|--------|----------------------|-------------------|
| 1 | Chatarra | 5–15 € | Lata, zapato viejo, paraguas roto, revista, botella |
| 2 | Común | 20–50 € | Radio, cartera, reloj de plástico, patinete oxidado |
| 3 | Decente | 75–200 € | Teléfono, cadena de plata, altavoz, monopatín |
| 4 | Valioso | 300–800 € | Reloj de oro, portátil, guitarra eléctrica, cámara |
| 5 | Lujo | 1.200–3.000 € | Joyas, lingote pequeño, cuadro, abrigo de pieles |
| 6 | Ilegal | 5.000–12.000 € | Maletín cerrado, estatuilla robada de museo, llaves de un deportivo |
| 7+ | Profundo | ×4 por tier | Generados por patrón: deportivo, obra maestra robada, escrituras de un rascacielos, la deuda de un senador… |
| ∞ | Infernal | — | Objetos únicos del club (ver §7.4); no se venden |

- **Catálogo fijo:** todos los objetos existen en un catálogo con **precio fijo** — no hay dos monopatines con precios distintos (los repetidos son válidos). Los premios de cada piso son los «objetos-billete» del catálogo (precio = mínima del piso siguiente +0–10 €); el premio del −9 es un objeto único, **El Billete del Diablo** (1.200.000 €): la única apuesta que la sala del −10 acepta.
- Inventario limitado (propuesta: **6 huecos**) para forzar decisiones entre vender y apostar.
- Cada NPC tiene loot table según su tipo (ver §6), pero **relativa al techo de la calle** (ver abajo): el mendigo siempre lleva lo peor disponible, el pijo el techo del momento.

**La calle va por detrás del club:** al empezar, el loot callejero nunca excede el premio del piso −1 — solo cae **Tier 1**. Cuando desbloqueas el piso **−5**, el mercado negro inunda el barrio y empieza a aparecer **Tier 2** en la calle (lo que se gana en el −1); con el **−6** aparece Tier 3, y en general **desbloquear el piso −(4+k) sube el techo de la calle al tier k+1**. La calle siempre va ~4 pisos por detrás de tu progreso: lo que ayer era un premio, mañana lo lleva un pijo por la acera. (Diegético: el club revende lo que la gente pierde en las mesas, y el barrio se llena de género.)

### 4.3 El ascensor y los pisos

El club se hunde bajo tierra: desde el vestíbulo (**piso 0**) un **ascensor** baja a los pisos −1, −2, −3… Cada piso es la sala de un **apostador**: un personaje con su mesa.

**El ascensor:**
- Al entrar aparece el **panel de pisos**: solo se muestran los pisos ya desbloqueados; los bloqueados ni aparecen. Que el panel gane botones ES la barra de progreso del juego.
- Un piso se desbloquea **al ganar una apuesta en el piso anterior**, y queda desbloqueado para siempre: la apuesta mínima del −2 es el valor del objeto que ganas en el −1, y así sucesivamente.
- Puedes subir y bajar cuando quieras. Jugar con el apostador es opcional: puedes entrar, mirar, hablar con él y volver al ascensor sin apostar.

**El duelo (regla común a todos los pisos):**
- Al entrar al piso, el apostador saca **su objeto** de la vitrina y lo deja **sobre la mesa, a la vista**. Su valor está clavado a la **apuesta mínima del piso siguiente**: nunca menos, y como mucho **~10 € por encima**. Ganar te da exactamente el billete de entrada al siguiente piso. Cada visita al piso saca un objeto-billete distinto del catálogo.
- Para sentarte colocas **un objeto tuyo** que cumpla la **apuesta mínima** del piso. Apostar algo mejor no mejora el premio (su apuesta ya está sobre la mesa): la decisión interesante es *qué* objeto arriesgar.
- El piso −N se juega **al primero que llegue a N puntos** contra el apostador (equivale a «al mejor de 2N−1»). En cada lanzamiento eliges cara o cruz: acierto = punto para ti; fallo = punto para él.
- Se lanza **tu moneda** (las mejoras viajan contigo), pero **el club pesa**: cada piso resta un hándicap a tu probabilidad por lanzamiento. Tus mejoras lo compensan — y en los pisos altos, lo superan.
- **Ganas el duelo** → te llevas **su objeto** y conservas el tuyo, y la mesa sigue abierta: el apostador saca otro objeto de la vitrina por si quieres otra ronda.
- **Lo pierdes** → él se queda tu objeto, pero la silla sigue siendo tuya mientras te quede algo que apostar. Solo te echan del club **cuando pierdes tu último objeto**: a la calle a robar (los pisos desbloqueados se conservan).

**La regla de dificultad (una sola fórmula):**

> **Prob. de ganar el duelo = 55 % + 5 puntos × (nivel de moneda − piso)**, acotada entre 2 % y 100 %.

Cada piso es **exactamente 5 puntos más difícil** que el anterior (nunca menos: la dificultad siempre baja al bajar), cada nivel de moneda devuelve exactamente 5 puntos en todos los pisos, y la regla que el jugador aprende sola es: **moneda al día (nivel = piso) → 55 %**. Por detrás se sufre (un nivel: 50 %; dos: 45 %); por delante se aplasta (el −1 con el doblón al máximo: **100 %** — tu moneda ya ni deja hablar a la del Trilero). Comprar suerte ES la progresión, y se nota en cada compra.

*Implementación:* la probabilidad por lanzamiento de la moneda se deriva internamente de la probabilidad objetivo del duelo (inversión numérica de la fórmula del "primero a N"), de modo que el marcador a N puntos y su drama se conservan sin que los duelos largos alteren la dificultad diseñada.

| Piso | Duelo | Prob. victoria (base · al día · máx.) | Tu apuesta mínima | Su apuesta (el premio) |
|------|-------|----------------------------------------|-------------------|------------------------|
| −1 | Primero a 1 | 50 % · 55 % · **100 %** | Tier 1 (5 €) | 20–30 € (mínima del −2) |
| −2 | Primero a 2 | 45 % · 55 % · 95 % | Tier 2 (20 €) | 75–85 € (mínima del −3) |
| −3 | Primero a 3 | 40 % · 55 % · 90 % | Tier 3 (75 €) | 300–310 € (mínima del −4) |
| −4 | Primero a 4 | 35 % · 55 % · 85 % | Tier 4 (300 €) | 1.200–1.210 € (mínima del −5) |
| −5 | Primero a 5 | 30 % · 55 % · 80 % | Tier 5 (1.200 €) | 5.000–5.010 € (mínima del −6) |

*(«al día» = nivel de moneda igual al número del piso; «máx.» = doblón al nivel 10.)*

**Las Profundidades (pisos −6 a −9):** siguen el patrón: duelo al primero a N (tope: primero a 8), la misma fórmula de dificultad (base: 25/20/15/10 %; al día: 55 %; con el doblón al máximo: 75/70/65/60 %), tu mínima = el premio del piso anterior, y su premio sigue subiendo (la mínima del piso siguiente, ×4 por piso, +≤10 €) con objetos de tiers 7+ cada vez más absurdos. Escalera completa de mínimas: 5 → 20 → 75 → 300 → 1.200 → 5.000 → 20.000 → 75.000 → 300.000 → **1.200.000 €**.

**El fondo (piso −10, la sala del millón):** la torre tiene **10 pisos**. El premio del −9 es el billete de 1,2 M€ que abre la última puerta: la sala del **Diablo**. Sus reglas no son las del club (ver §7.4): al cruzar la puerta **te quita todas las mejoras** — tu doblón infernal vuelve a ser un céntimo oxidado — y solo hay **UN lanzamiento al 50 %**. Sin hándicap, sin bonus, sin canto, sin amuleto, sin pies fríos. Y perder aquí no cuesta un objeto: lo cuesta **todo** (§7.4). La moneda más limpia de todo el edificio está en el infierno.

**Nota de balance:** el premio ya no es negocio de reventa — vale lo justo para sentarte en el piso siguiente. Ganar te hace avanzar; el dinero para mejoras sale de **vender los premios que no vas a usar** y del botín callejero. El EV sigue siendo favorable en los pisos altos (arriesgas ~5–15 € al 50–60 % por ganar ~25 €), y el freno real es la **varianza**: perder te quita el objeto y la escalera entera. Palanca restante para playtesting: si la apuesta del apostador se re-sortea en cada entrada al piso o solo en cada visita al club.

- A mitad de duelo **no puedes retirarte**… salvo con la mejora "Pies fríos": pagas al apostador **el valor de tu objeto en metálico** y te lo llevas (sin premio). Si no tienes el dinero, no hay retirada.
- La expulsión (solo al perder tu último objeto) tiene animación rápida (te sacan a rastras dos porteros) → recolocación en la calle. Sin tiempos de castigo: el castigo ya es haberlo perdido todo.

### 4.4 La moneda y sus mejoras

La moneda del jugador es **su** moneda: se muestra en primer plano en cada lanzamiento y evoluciona visualmente con las mejoras (de céntimo roñoso a doblón infernal).

**Mejora principal — Trucar la moneda:** cada nivel suma probabilidad a *tu elección* (no a un lado fijo).

**10 niveles, +5 % por nivel** — uno por cada piso de la torre. El nivel k está pensado para pagar el hándicap del piso −k:

| Nivel | Bonus | Precio (acumulado) | Aspecto de la moneda | Se compra hacia… |
|-------|-------|--------------------|----------------------|------------------|
| 0 | +0 % | — | Céntimo oxidado | — |
| 1 | +5 % | 100 € | Céntimo pulido | −1 |
| 2 | +10 % | 250 € | De latón | −2 |
| 3 | +15 % | 600 € | De plata | −2/−3 |
| 4 | +20 % | 1.500 € | Plata grabada | −3 |
| 5 | +25 % | 4.000 € | De oro | −4 |
| 6 | +30 % | 10.000 € | Oro grabado | −5 |
| 7 | +35 % | 25.000 € | Con un rubí engastado | −6 |
| 8 | +40 % | 60.000 € | Grabado demoníaco | −7 |
| 9 | +45 % | 150.000 € | Con fuego dentro | −8 |
| 10 | +50 % | 400.000 € | Doblón infernal (arde al girar) | −9 |

Cada nivel suma **5 puntos a tu probabilidad de ganar el duelo en todos los pisos** (fórmula en §4.3): al día = 55 %; por delante, pisos superados triviales (el −1 al 100 % con el doblón); por detrás, castigo que se nota. Los precios escalan ~×2,5 por nivel, en paralelo a la economía de la torre: cada nivel se paga vendiendo los premios de la profundidad donde empiezas a necesitarlo. **Regla de ritmo:** un nivel ≈ 1–2 premios vendidos de tu piso actual ≈ 2–8 duelos ganados ≈ 5–15 min de juego. Si en playtesting un nivel tarda más de ~20 min, se abarata ese tramo (la zona a vigilar son los niveles 3–5). Y en el −10 nada de esto vale (§7.4).

**Mejoras secundarias (propuestas, compra única salvo consumibles):**
- **Canto de plomo** (1.500 €) — pequeña probabilidad (5 %) de que un fallo caiga de canto: se repite el lanzamiento (el punto no cuenta para el apostador).
- **Amuleto de perro** (25.000 €) *(el collar de tu perro)* — una vez por duelo, un fallo que perdería el duelo se convierte en repetición. Emotivo y potentísimo en los pisos profundos: precio de late game.
- **Pies fríos** (5.000 €) — desbloquea retirarte **en cualquier momento del duelo** pagando al apostador el valor de tu objeto en metálico para conservarlo (sin premio). Sin dinero suficiente no puedes pagar la retirada: cuanto más caro es lo que arriesgas, más caro es acobardarse.
- **Ojo del tahúr** (3.000 €) — durante 1 s, la moneda brilla del color del resultado antes de taparla (skill: hay que fijarse). Cara/cruz siguen siendo aleatorios; esto premia atención.
- **Bate claveteado** (2.000 €) — los NPC sueltan a veces (15 %) un segundo objeto.
- **Soplo del diablo** (2.500 €/ud, consumible) — un lanzamiento al 75 %. Se compra de uno en uno.

### 4.5 Tiendas

Ambas dentro del club, en el vestíbulo:
- **El Perista (vender):** compra cualquier objeto por su valor V. Interfaz de arrastrar objetos al mostrador. Frases sarcásticas según lo que vendas.
- **El Buhonero (mejoras):** vende las mejoras de moneda y secundarias. Vitrina física en la que las mejoras compradas desaparecen del estante (mundo persistente, legible sin UI).

---

## 5. El mapa

Un único escenario compacto, sin pantallas de carga:

```
┌─────────────────────────── LA CALLE ───────────────────────────┐
│  Farolas, coches aparcados, tiendas cerradas, NPCs paseando    │
│                                                                │
│   [parque]      [parada bus]      [cajero]      [obra]         │
│                                                                │
│                  callejón ─┐                                   │
└────────────────────────────│───────────────────────────────────┘
                             │  letrero neón: HELL OR TAILS
                  ┌──────────▼──────────┐
                  │  VESTÍBULO (piso 0) │  Encargado · Perista · Buhonero
                  │     [ASCENSOR]      │
                  └──────────┬──────────┘
                  ═══════════╪═══════════  bajo tierra
                       ┌─────▼─────┐
                       │  PISO −1  │  Apostador 1 · trastienda cutre
                       ├───────────┤
                       │  PISO −2  │  Apostador 2 · terciopelo rojo
                       ├───────────┤
                       │  PISO −3  │  Apostador 3 · piedra y antorchas
                       ├───────────┤
                       │  PISO −4  │  Apostador 4 · grietas de lava
                       ├───────────┤
                       │  PISO −5  │  Apostador 5 · el infierno
                       ├───────────┤
                       │     ⋮     │  Las Profundidades (−6 … −9)
                       ├───────────┤
                       │  PISO −10 │  El Diablo · la sala del millón
                       └───────────┘
```

- **La calle:** 2 manzanas jugables. Zonas con densidad distinta de NPCs (el parque tiene mendigos y jubilados —loot bajo—; la zona del cajero, oficinistas —loot medio—; junto a los coches buenos, pijos —loot alto—).
- **El club:** todos los pisos cuelgan del mismo ascensor. Sin puertas ni pasillos entre pisos: el ascensor es el único camino, y su panel (solo pisos desbloqueados) es el mapa del progreso.
- **Dirección de arte del club:** transición gradual — vestíbulo de garito cutre → terciopelo → piedra → lava. El descenso es literal: el ascensor tarda un poco más y traquetea un poco más con cada piso que bajas.

---

## 6. NPCs

El loot de cada tipo es **relativo al techo de la calle** (§4.2), que empieza en Tier 1 y sube al profundizar en el club:

| Tipo | Zona | Loot (relativo al techo) | Comportamiento |
|------|------|--------------------------|----------------|
| Mendigo | Parque | Techo −1 (mínimo T1) | Lento, ni se inmuta |
| Jubilado | Parque/paseo | Techo −1 a techo | Lento, grita (alerta zona) |
| Oficinista | Cajero/parada | Techo | Huye rápido |
| Turista | Toda la calle | Techo (cámara) | Despistado, fácil, escaso |
| Pijo | Coches caros | Techo, valores altos del tier | Muy rápido, escaso |
| Repartidor | Obra/furgoneta | Caja sorpresa (techo ±1) | Aparece por eventos, en bici |
| Policía *(v2)* | Ronda aleatoria | — | Te persigue si te ve robar |

- La calle **nunca alcanza** lo que se juega en tu piso más profundo (va ~4 pisos por detrás): alimenta el bucle, pero el club es el único camino hacia abajo.
- Personajes del club (no atacables): **El Encargado** (te recibe, da el bate, comenta tu progreso), **los apostadores** (uno por piso, tus rivales de mesa, cada vez menos humanos), **porteros** (te echan al perder).

---

## 7. Progresión y estructura

### 7.1 Primeros 10 minutos (tutorial diegético)
1. Cinemática breve de la ruina (novia/trabajo/perro en 3 viñetas low poly).
2. Mendigas → susurro → callejón → diálogo del Encargado → recibes el bate.
3. El Encargado te señala la calle: "tráeme algo que apostar".
4. Primer robo guiado (mendigo cerca de la puerta) → ascensor (solo el botón −1 encendido) → primera apuesta.
5. Ganes o pierdas, el Encargado te explica vender/mejorar y te suelta al bucle.

### 7.2 Curva esperada
- **Early:** piso −1 en bucle, primeras ventas, moneda nivel 1–2.
- **Mid:** pisos −2/−3, decisiones de inventario, mejoras secundarias.
- **Late:** pisos −4/−5, apuestas de tier alto donde un fallo duele de verdad.

### 7.3 Anti-frustración
- Perder nunca quita mejoras ni dinero: solo el objeto apostado.
- El respawn de NPCs cerca del callejón es rápido: de la expulsión a la siguiente apuesta en <90 s.
- **Racha de mala suerte (piedad oculta):** tras 3 duelos perdidos seguidos, tu siguiente duelo en el piso −1 se juega con +10 puntos ocultos de probabilidad. No se comunica jamás.

### 7.4 Final (propuesta)
Al ganar en el **−9** te llevas su premio: **el billete de 1.200.000 €**. En ese momento el panel del ascensor enciende un botón **sin número, al rojo vivo**: el piso −10, la sala del millón. Abajo espera **el Diablo** (¿el Encargado sin máscara?), que pone sobre la mesa **recuperar tu antigua vida** (objeto único: "Tu vida") y exige tu objeto de 1,2 M€. Sus reglas: **te quita todas las mejoras al entrar** y se juega a **UN solo lanzamiento al 50 %** — todo lo que compraste no vale nada aquí; vuelves a ser el mendigo del principio, con una moneda limpia en la mano. Dos finales:
- **Ganas:** **completas el juego** — cinemática inversa de la intro (perro incluido). Créditos. Se desbloquea NG+ (¿la moneda se resetea, el club recuerda?).
- **Pierdes:** el Diablo se lo queda **TODO**: objetos, dinero, mejoras y pisos desbloqueados. Despiertas en la calle como la primera noche, con el bate y nada más — partida reiniciada de verdad. La apuesta definitiva se juega con el progreso entero encima de la mesa; por eso pulsar el botón rojo es la decisión más grande del juego.

---

## 8. Multijugador

**Filosofía:** el multi es la misma partida compartida, no un modo aparte. 2–4 jugadores, drop-in.

- **Mundo compartido:** misma calle y mismo club. Los NPCs son recursos en competencia (si te roban "tu" pijo, a espabilar).
- **Economía individual:** inventario, dinero y mejoras por jugador. Nada de banco común (evita griefing).
- **Espectar mesas:** bajar al piso donde otro jugador está en pleno duelo y verlo en directo, con emotes para animar o gafar.
- **Mesa PvP (sala aparte en el vestíbulo):** dos jugadores apuestan un objeto cada uno; ambos eligen lado de la MISMA moneda (uno cara, otro cruz) y se lanza **una moneda neutral del club (50 % real, sin mejoras)** — el ganador se lo lleva todo. Los duelos son el único sitio donde las mejoras no cuentan: puro pique justo.
- **Toques sociales:** golpear a otro jugador con el bate no roba, pero lo tira al suelo (y sí, habrá lobbies que solo hagan esto).
- **Single player = host sin invitados** (misma arquitectura, cero código duplicado).

**Tecnología (decisión para la fase Unity):** Netcode for GameObjects con host local (P2P via Unity Relay) — encaja con 2–4 jugadores y permite que el single sea "host solo". Se detallará en el TDD.

---

## 9. Arte y sonido

- **Estilo:** low poly con paleta reducida. Calle: noche azulada, luz naranja de farolas. Club: del granate al rojo lava según se desciende. El neón del letrero es el ancla visual del juego.
- **Personajes:** proporciones ligeramente cabezonas, sin rasgos faciales detallados (excepto la sonrisa del Encargado).
- **La moneda:** el asset más importante del juego. Cámara lenta en el lanzamiento, sonido metálico gordo, slow-mo en el último bote cuando es el punto que decide el duelo.
- **Sonido:** lo-fi sórdido en la calle; en el club, jazz sucio que se va distorsionando sala a sala hasta ser coros infernales en la 5.
- **Feedback de apuesta:** al ganar, lluvia breve de chispas doradas; al perder, silencio seco, un portazo y la pantalla en frío.

---

## 10. UI/UX

- HUD mínimo: dinero (esquina), inventario en rueda/barra de 6 huecos, prompt contextual ("E — Apostar", "Clic — Palazo").
- En la mesa: vista fija cinematográfica, elección CARA/CRUZ en dos botones grandes, marcador del duelo siempre a la vista (tú ●●○ · él ●○○), valor del objeto en juego siempre visible.
- **Panel del ascensor:** botonera física diegética (sin menú flotante). Los pisos bloqueados ni aparecen; al desbloquear uno suena un *ding* y se enciende el botón nuevo delante del jugador.
- La probabilidad real **nunca** se muestra como número en la mesa; solo en la tienda de mejoras ("tu moneda: 56 %"). En la mesa, la confianza es una sensación, no una cifra.

---

## 11. Alcance y roadmap

### MVP (vertical slice)
- [ ] Calle con 3 tipos de NPC + robo con bate
- [ ] Club: vestíbulo + ascensor con panel + pisos −1 y −2 funcionales
- [ ] Sistema de objetos (tiers 1–4) + perista
- [ ] Moneda con mejora principal (3 niveles) + buhonero
- [ ] Bucle completo single player + guardado

### Beta
- [ ] Pisos −3 a −5, tiers 5–6, mejoras secundarias
- [ ] Intro/tutorial diegético + final
- [ ] Multijugador co-op (mundo compartido)
- [ ] Policía y eventos de calle

### Release
- [ ] Mesa PvP + emotes
- [ ] Las Profundidades (pisos −6 a −9) + la sala del millón (−10, el Diablo)
- [ ] Audio completo, pulido de animaciones, NG+
- [ ] Balance con datos de playtesting

---

## 12. Preguntas abiertas

1. **Guardado en multi:** ¿el progreso de un invitado viaja con él a otras partidas, o cada mundo tiene su progreso?
2. **NG+ tras ganar al Diablo:** ¿la moneda se resetea, el club recuerda tu cara, cambian los apostadores?
3. **Perspectiva de cámara:** ¿tercera persona (mejor para el bate y el humor físico) o primera (mejor para la tensión de la moneda)? Propuesta: tercera en el mundo, cinemática fija en la mesa.
4. **¿El bate mejora?** (daño no importa; podría mejorar velocidad de robo o loot — ya cubierto por "Bate claveteado").
5. Nombre del protagonista y si habla o es mudo.
