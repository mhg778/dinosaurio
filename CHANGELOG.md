# DINOSAURIO — Release v1.18 (corregida)

Release candidata verificada contra regresión respecto a la última versión estable en GitHub (`dinosaurio16.html`, idéntica byte a byte al punto de partida de esta serie de correcciones). 41 cambios acumulados: 0 pérdidas de funcionalidad no intencionadas.

## Bugs corregidos

- **NaN de precedencia** en el pie de la Vista Espectral 3D (`proj.results.length-1` sin paréntesis).
- **`captureSnapshot()` ignoraba el modo hipotético**: ahora guarda edad/área/puntos/catPts del escenario realmente activo (norma o +25%), no siempre los normativos.
- **Waterfall desalineado con snapshots hipotéticos**: el desglose por bloque ahora refleja el escenario real del snapshot guardado.
- **Contaminación cruzada de categorías en Monte Carlo**: cada snapshot proyecta ahora con su propio límite legal, modelo de residuos y categoría, en vez de heredar la categoría activa en pantalla.
- **Scraping frágil de DOM eliminado**: `readSnapshotFromDOM` sustituido por lectura directa de `window.DINO_ENGINE.snapshots`.
- **`computeEquity()` sumaba cursos como puntos**: ahora convierte correctamente unidades de curso a puntos antes de agregar al Mérito Específico.
- **KPI "Captación joven" secuestrado**: ya no se sobrescribe con un contador de snapshots en modo multi-snapshot; muestra la captación real de cada uno.
- **Deduplicación** de `erf`/`normCDF`/`clamp`/`AGE_BOUNDS` entre los dos bloques `<script>` (antes duplicados, ahora Script 2 reutiliza los de Script 1).
- **Funciones muertas eliminadas**: `randNormal` (nunca invocada), `applyMetaAdjustToSliders` (sustituida por inyección no destructiva).

## Metodología del Monte Carlo

- **Exclusión real en vez de muro artificial**: los candidatos que superan el techo/suelo legal quedan excluidos (`NaN`) del proceso, en vez de quedarse artificialmente congelados en el límite (evita el pico de densidad irreal en la frontera de edad).
- **Banda de incertidumbre visible en modo multi-snapshot**: antes solo se veía con un snapshot activo; ahora se muestra (más tenue) también con varios a la vez.

## UX y transparencia

- Sliders de Monte Carlo (Iteraciones, Deriva, Horizonte) con track visible (antes invisibles por falta de CSS).
- Aviso inline en sliders "fantasma" (Idiomas, TIC, Carnets, etc.) que no afectan a la edad.
- Ajuste global (`metaAdjust`) ya no sobrescribe destructivamente los sliders individuales.
- Aviso de calibración en vivo: detecta y muestra si la suma de sub-ítems "media muestra 2025" no coincide con la constante de calibración del modelo, sin fabricar cifras nuevas.
- Escalado de alta resolución (Retina/4K) en los 5 canvas principales.

## Funciones nuevas

- **Deriva empírica real 2023-2025**: checkbox que sustituye la deriva manual del Monte Carlo por la estimada con fechas de nacimiento reales de los Clasificados (n=373+350+400), por categoría (Ejércitos/Comunes), con aviso explícito de fragilidad estadística (solo 3 puntos anuales).
- **Oráculo de Reclutamiento**: solucionador inverso exacto (no fuerza bruta aleatoria) que, dado un objetivo de edad, calcula la combinación de méritos que lo alcanza exactamente maximizando puntos totales, resolviendo el sistema lineal del modelo.
- **Módulo de informe imprimible**: botón "Generar Informe" que construye un documento en blanco y negro (sin la interfaz de sliders, sin autoría ni disclaimer académico) a partir de los snapshots guardados, con tablas de datos y gráfico ligero, listo para `Ctrl+P` → PDF.

## Verificación de regresión realizada

- Diff funcional completo contra el HTML base de GitHub: 61 funciones se mantienen sin cambios de comportamiento, 3 eliminaciones son intencionadas y verificadas (bugs corregidos), 3 aparentes eliminaciones son falsos positivos (convertidas a alias de variable, mismo patrón de uso), 8 funciones nuevas corresponden a las features de esta lista.
- Verificados los 80 `id` de HTML referenciados por el JavaScript original: todos presentes en la nueva versión, ninguno renombrado ni eliminado.
- Balance de `<div>`/`</div>` y bloques `<script>` verificado tras cada cambio.
