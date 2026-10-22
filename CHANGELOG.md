# DINOSAURIO 2.02 · Registro de cambios

Release de calibración, validación y funcionalidad. IDs del backlog entre paréntesis.

La 2.02 incorpora cuatro ideas:

1. **Dual interpretable (C1).** El resultado se traduce a **valores reales de política** (tope de experiencia, factor académico, factor de servicio), con el valor continuo, el valor aplicable redondeado a la rejilla del Laboratorio (1 · 0,1 · 0,1), las palancas que saturan y su aporte a la edad. El presupuesto puede expresarse en unidades **normalizadas** (fracción del rango de cada palanca, por defecto) o **brutas** (las del Oráculo). Incluye el botón **«Aplicar al Laboratorio»**, que mueve los deslizadores reales, y se refresca solo cuando cambian el bloqueo del factor académico o los deslizadores.
2. **Percentiles de edad (Modo 2).** Mediana y P10-P25-P75-P90 por año y rama, con **bandas** P10-P90 y P25-P75 en el gráfico y columnas nuevas en la tabla. Muestran la dispersión de edades de la plantilla, no incertidumbre del modelo.
3. **Integridad del diccionario.** Hash **FNV-1a** del diccionario `_T` decodificado, comparado con el esperado en la construcción y con el recalculado (T30). Coincide con el de `gunzip` del sistema.
4. **Retrospectiva 2016→2025 (V7).** Con las plazas convocadas reales del ET y la `h` actual, el modelo se compara con las diez existencias observadas (desviación máxima 4,3 %, RMSE 2,4 %). **Un parámetro frente a diez observaciones**, no un ajuste del punto final. Esta calibración fija `h` en **7 %** (antes 8 %). El total FAS 2025 se muestra como contraste (2.680 efectivos y 50,9 años frente a 2.989 y 50,2 observados; el flujo de los demás ejércitos es un supuesto).

## 2.01 - Añadido

- **Modo 2 del Monte Carlo: fuerza activa con efecto arrastre (C2).** Stock oficial 2025 (ET 1.386, Armada 452, Aire 435, Comunes 716), envejecimiento +1,0/año, bajas por límite legal de edad (a) y otras bajas (b) separadas, altas con desfase de un año según el cupo del catálogo, Δ de la edad de entrada como escenario. Validaciones V1-V6. No toca los coeficientes del predictor.
- **Años de servicio esperados (C3).**
- **Dual del Pareto (C1):** máximo desplazamiento con intervención acotada (cotas, L2, L1); dualidad primal-dual exacta.
- **Tarjeta «Cupo y repesca observados» (A6, C4)** en Oportunidades.
- **Tests T19-T29** (contabilidad, mecanismos separados, C3, stock inicial, determinismo, predictor intacto, dual, conmutador de modos, documentación, compresión, seguridad).
- **Documentación:** auditoría de calibración, Modo 2, supuestos, fuentes y límites de la validación 2026 en el centro de ayuda.
- **Herramientas offline** (`tools/`): parser y fixtures (A8, A0, A1, A1b), análisis de celdas, deriva, cupo y repesca (A3, A4, A6, A7), contraste con simulacro (B2), efecto mecánico (B3) y tabla del baremo (D3).

## Cambiado

- **Compresión (D1):** diccionario `_T` en gzip estándar con inflate mínimo síncrono (antes, LZ77 propio). `window.DINO_T_META` informa del códec.
- **Oportunidades (C4):** el 17,52 % heredado (184 vacantes ÷ 1.050 plazas) mezclaba unidades; se documenta, se renombra «plazas sin cubrir» y la referencia por defecto pasa a 20,0 % (210 plazas reasignadas ÷ 1.050). El 17,5 % sigue seleccionable como referencia heredada.
- Título y textos visibles a 2.01. **Se mantienen** los identificadores de contrato de exportación (`DINOSAURIO-2.0-AUDITADO`, `DINOSAURIO-BAYES-2.0-AUDITADO`) por compatibilidad.

## Corregido

- **XSS en los campos del catálogo (D5):** `innerHTML` insertaba sin escapar `area`, `cometido`, `unidad`, experiencia, formación y requisitos en la tabla del catálogo, la ficha de plaza, el asistente y la trazabilidad longitudinal. Con un catálogo manipulado, la 2.0 creaba 61 elementos ejecutables; la 2.01, ninguno. 56 puntos de salida escapados.
- **Documentación inexacta:** la descripción «GZIP» del diccionario `_T` (era LZ77 propio) y la referencia a los artículos 16 y 21.1.a) del RD 383/2011 (el límite 61/58 está en el art. 19.1).

## Verificado (sin cambios de código)

- Los 1.123 residuos del motor se reproducen con error ≤ 0,005 (edad decimal a la fecha de firma de cada Resolución; OLS de cuatro bloques por grupo; primera aparición de cada solicitante). Coeficientes reproducidos: 0,4101 y 0,5062 (específicos), 0,165 con SE 0,279 (académicos de Comunes).
- El baremo es idéntico 2023-2025 (salvo el bloque de pilotos, que desaparece en 2024); el único cambio de puntuación de 2026 es la escala de experiencia.

---

# DINOSAURIO 2.0  Registro de cambios

El artefacto `DINOSAURIO2.html` consolida toda la plataforma analítica de seis módulos en un único fichero mantenible, con el catálogo longitudinal BOE 2023-2026 íntegro (`1.238` registros, `1.550` plazas) y `1.123` residuos empíricos IEEE-754.


## 1. Arquitectura de compactación y mantenibilidad humana

- **Compresión de datos sin pérdida de legibilidad lógica**

- **Preservación de identificadores de dominio y mapa de 50 hitos**

- **Intervención CSS de Nivel A (una regla por línea)**

- **Poda de código muerto**


## 2. Cierre de regresiones de auditoría y defectos de visibilidad heredados

- **RC2-01 (Generación real de PDF en 2D y 3D)**

- **RC2-02 (Cascada responsive en móvil a 390 px)**

- **VIS-01 (Aviso de presión extrema en Half-Life `#halfLifeWarn`)**

- **VIS-02 (Gráfica «Simulación real 2025» `#simRealBox` / `#chartSimReal`)**



## 3. Motores matemáticos, Laboratorio Inverso y Frontera de Pareto

- **Identidad de arranque y aislamiento de referencia (`T1`, `T2`, `T3`, `T15`)**

- **Calibración del modelo y presets dinámicos (`T4`)**

- **Solvers de políticas `QP L2`, `Solo Experiencia` y `Sparse L1` (`T6`, `T13`, `T14`)**

- **Firma de contexto e historial de deshacer (`T7`, `T16`)**



## 4. Monte Carlo, Comparador de Densidades y Contrato de Exportación

- **Semillas deterministas y conservación de masa (`T5`, `T8`, `T17`)**

- **Separación de extinción en `Delta PDF` y pasos sin supervivientes (`T11`, `T18`)**

- **Generador MATLAB reproducible (`T11`)**



## 5. Catálogo BOE 2023-2026, Asistente y Decodificador

- **Sincronización de escala y trazabilidad longitudinal**

- **Asistente sensible al currículo y supuesto `2026_perfil` (`T12`)**

- **Decodificador inverso con unión formal de intervalos (`T9`)**




# DINOSAURIO — Release v1.18

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
