# Hell or Tails — Ideas para la versión Unity

Backlog de ideas pensadas mientras se itera la demo web, pero cuya
implementación real tiene sentido en el juego completo de Unity (no en
el simulador web). Se apuntan aquí para no perderlas.

---

## Skins del bate por colección de galería

- Cada **10 objetos** distintos conseguidos en la galería desbloquea una
  **skin nueva para el bate** (el arma con la que golpeas a los NPCs en la calle).
- El catálogo actual tiene ~71 objetos únicos, así que da para unas **7 skins**
  (10, 20, 30, 40, 50, 60, 70 piezas expuestas).
- Es un premio cosmético al progreso de coleccionismo: la galería deja de ser
  solo un museo y pasa a recompensar con identidad visible en la calle.
- En la web NO tiene mucho sentido implementarlo (el bate no se ve como modelo,
  la calle es una escena SVG estática). En Unity sí: el bate es un asset 3D
  low poly con material/decoración intercambiable.
- Pendiente de decidir: estética de cada skin (de bate de béisbol roñoso a algo
  cada vez más infernal, en paralelo al descenso del club), si hay un selector
  de skin o se equipa la última desbloqueada, y si alguna skin es puramente
  visual o añade un guiño (partículas, sonido de impacto distinto).

---

## Mejora de velocidad de movimiento

- Mejora comprable que aumenta la **velocidad de desplazamiento del personaje**
  por la calle (correr más rápido entre NPCs, llegar antes al callejón, escapar
  mejor de la policía si se añade ese riesgo).
- Encaja con el pilar de "bucle rápido" del GDD: menos tiempo andando, más tiempo
  robando y apostando. Hermana de la "Mano rápida" de la web (que acelera la
  tirada), pero aplicada al mundo abierto de la calle.
- En la web NO aplica (la calle es una escena estática sin movimiento libre).
  En Unity sí: el jugador se mueve en tercera persona por 2 manzanas.
- Pendiente de decidir: por niveles (como la moneda) o compra única, cuánto sube
  cada nivel, precio, y si afecta solo a la calle o también a lo rápido que
  cruzas el vestíbulo/ascensor.

