<p align="center">
  <img
    src="assets/dinosaurio.png"
    alt="DINOSAURIO — Laboratorio y análisis"
    width="800"
  />
</p>
============================================================================

  DINOSAURIO 2.0 · MANUAL DE OPERACIÓN DEL SIMULADOR ANALÍTICO

============================================================================


============================================================================

¿Cuánto cambia la edad de una promoción cuando se altera la pendiente de un
solo mérito sin tocar un día los límites legales de ingreso? Entre las
convocatorias de Reservistas Voluntarios publicadas en el BOE de 2023 a 2026,
sobre un total de 1.238 registros y 1.550 plazas ofertadas, la reforma del
Apéndice 3 reduce en cinco años el tramo necesario para alcanzar la máxima
puntuación por experiencia civil. El articulado no menciona la edad en una
sola línea, pero el desplazamiento demográfico sobre la cohorte seleccionada
es inmediato y medible.

============================================================================

ÍNDICE
------
  1. Exposición de motivos y propósito del instrumento
  2. Puesta en marcha y arquitectura del archivo autónomo
  3. Marco regulatorio (BOE 2023-2025 frente a BOE 2026)
  4. Los dos recorridos del Laboratorio (Directo e Inverso)
  5. Modelo jerárquico bayesiano con contracción hacia la media
  6. Palancas de política y bloqueo de factores no significativos
  7. Posiciones guardadas (A, B, C) y contraste de escenarios
  8. Exploración fuera de norma (+25%), vida media académica y equidad
  9. Oráculo de Reclutamiento (resolución algebraica directa)
  10. Motor Monte Carlo, exclusión en barrera y vistas 2D y 3D
  11. Desglose en cascada, mapa de calor y comparador de densidades
  12. Observatorio de sensibilidad de políticas
  13. Simulador de oportunidades y redistribución de vacantes
  14. Alternativas del aspirante por ajuste curricular y territorial
  15. Catálogo longitudinal BOE 2023-2026 y alerta de renumeración
  16. Optimización multicriterio de Pareto y decodificador inverso
  17. Informe imprimible en PDF y exportación reproducible
  18. Batería de verificación matemática (T1-T18) y diagnóstico
  19. Límites metodológicos y lectura de resultados
  20. Resolución de incidencias operativas
  21. Reflexión final


----------------------------------------------------------------------------
1. EXPOSICIÓN DE MOTIVOS Y PROPÓSITO DEL INSTRUMENTO
----------------------------------------------------------------------------

Cualquier baremo de concurso que puntúe el tiempo de servicio o los años de
ejercicio profesional lleva implícito un filtro biológico. Quien diseña la
norma suele tratar los apartados de méritos como compartimentos estancos de
capacidad técnica, pero el tiempo necesario para acumular esos puntos ata la
puntuación final a la fecha de nacimiento del candidato.

DINOSAURIO 2.0 nace para medir esa atadura con datos reales. No sustituye al
baremo oficial ni interpreta la voluntad del legislador, sino que modela la
relación estadística entre los cuatro bloques de puntuación (Tiempo de
Servicio, Méritos Académicos, Méritos Generales y Mérito Específico) y la
edad resultante en cada escala.

El simulador sirve por igual a dos perfiles de usuario. Al aspirante le
permite comprobar dónde rinde más su currículo antes de firmar la instancia,
mientras que al analista de planificación le ofrece un banco de pruebas para
cuantificar qué provoca cada retoque normativo sobre la pirámide de edad.


----------------------------------------------------------------------------
2. PUESTA EN MARCHA Y ARQUITECTURA DEL ARCHIVO AUTÓNOMO
----------------------------------------------------------------------------

La herramienta se entrega en un único fichero HTML autónomo de unos 338 KB.
No necesita servidor, instalación previa ni conexión a internet, basta con
abrirlo mediante doble clic en cualquier navegador actual (Chrome, Edge,
Firefox o Safari), tanto en equipos de escritorio como en teléfonos móviles.

Para lograr ese tamaño sin destruir la legibilidad del código fuente, la
arquitectura separa los datos brutos de la lógica de negocio. Los textos de
la interfaz, las 1.238 filas del catálogo oficial y los 1.123 residuos
empíricos viajan comprimidos en bloques estáticos, mientras que el motor
matemático conserva íntegros los nombres de sus funciones, estructurados en
cincuenta hitos comentados (S1.1 a S3.21) y una hoja de estilos dispuesta a
una regla por línea.

Todos los lienzos gráficos leen la densidad real de píxeles de la pantalla
para dibujar textos y curvas con nitidez en monitores de alta resolución,
adaptando las rejillas de controles desde anchos móviles de 390 píxeles sin
desbordamientos laterales.


----------------------------------------------------------------------------
3. MARCO REGULATORIO (BOE 2023-2025 FRENTE A BOE 2026)
----------------------------------------------------------------------------

En la barra superior del Laboratorio se conmuta el marco normativo entre las
convocatorias de 2023-2025 y la regulación introducida en 2026.

La diferencia crítica entre ambos marcos se concentra en el Apéndice 3 del
Mérito Específico. Hasta 2025, la experiencia laboral civil afín exigía más
de veinte años de ejercicio para alcanzar el techo de 25 puntos oficiales, y
un perfil intermedio de siete años sumaba 10 puntos. Con el baremo de 2026,
el techo de 25 puntos se alcanza a los quince años de experiencia, y esos
mismos siete años pasan a otorgar 15 puntos oficiales (un salto de +2,0
puntos en la escala interna del modelo).

Al marcar la casilla de vinculación de años BOE dentro de la tarjeta de
experiencia civil, el simulador traduce al instante cualquier antigüedad
entre 0 y 30 años a su puntuación normativa vigente, manteniendo intacta la
referencia de partida para poder medir el salto neto.


----------------------------------------------------------------------------
4. LOS DOS RECORRIDOS DEL LABORATORIO (DIRECTO E INVERSO)
----------------------------------------------------------------------------

El espacio de trabajo del Laboratorio permite abordar el problema desde dos
direcciones opuestas según la pregunta que se quiera responder.

El Recorrido A (Directo) está pensado para explorar qué ocurre cuando se
mueven las palancas. El usuario modifica manualmente cualquiera de los
quince controles de mérito del aspirante o los tres hiperparámetros de
política de la convocatoria, y el motor devuelve al instante el desplazamiento
producido sobre la puntuación total y sobre la edad estimada.

El Recorrido B (Inverso) invierte la operación. El analista fija primero
cuántos años desea rejuvenecer o envejecer la media respecto a la referencia
(usando los botones dinámicos calibrados para cada escala o introduciendo la
cifra exacta), y el optimizador cuadrático despeja qué combinación de topes
y multiplicadores de política alcanza esa meta alterando lo mínimo posible
la estructura del baremo.


----------------------------------------------------------------------------
5. MODELO JERÁRQUICO BAYESIANO CON CONTRACCIÓN HACIA LA MEDIA
----------------------------------------------------------------------------

La base empírica reconstruida consta de dos subpoblaciones con distinto
tamaño muestral, Ejércitos (Oficiales, Suboficiales y Tropa, con 769
observaciones) y Cuerpos Comunes (con 354 observaciones). Si se estimaran
las pendientes de cada grupo por mínimos cuadrados ordinarios de forma
aislada, el grupo menor quedaría excesivamente expuesto a la varianza de su
propia muestra.

Para evitar ese sobreajuste sin borrar las diferencias reales entre escalas,
el motor pondera la regresión propia de cada grupo con la regresión global
conjunta (N = 1.123) mediante un factor de contracción bayesiana regulado
por la constante tau = 80. El peso propio de cada subpoblación se obtiene
dividiendo su tamaño muestral entre la suma de dicho tamaño más ochenta.

De este modo, Ejércitos retiene un factor de peso propio de 0,906, mientras
que Cuerpos Comunes se apoya algo más en el comportamiento general con un
factor de 0,816. El recuadro técnico situado bajo el gráfico radial muestra
en vivo el factor activo y despliega un aviso de calibración si la suma de
los valores iniciales de experiencia, titulación y cursos se aparta de la
media histórica empleada para centrar la ordenada en el origen.


----------------------------------------------------------------------------
6. PALANCAS DE POLÍTICA Y BLOQUEO DE FACTORES NO SIGNIFICATIVOS
----------------------------------------------------------------------------

El panel de intervención normativa actúa sobre tres parámetros agregados de
la convocatoria. El primero regula el tope máximo de experiencia civil entre
15 y 35 puntos BOE (con valor central en 25), el segundo aplica un factor
multiplicador de 0,5x a 1,5x sobre el mérito académico, y el tercero pondera
entre 0,0x y 2,0x el peso del tiempo de servicio militar previo.

En la subpoblación de Cuerpos Comunes, la pendiente estimada para el bloque
académico presenta un error estándar alto (0,165 ± 0,279) que impide darla
por estadísticamente distinta de cero al 95% de confianza. La casilla de
bloqueo situada bajo el selector de coeficientes permite anclar esa palanca
en su valor actual para impedir que los algoritmos de optimización apoyen
sus soluciones sobre un coeficiente ruidoso.

Desde ese mismo bloque se puede conmutar entre los coeficientes contraídos
por grupo (configuración por defecto) y los coeficientes fijos de la
regresión clásica de Ejércitos cuando se desee contrastar el impacto puro de
la contracción bayesiana.


----------------------------------------------------------------------------
7. POSICIONES GUARDADAS (A, B, C) Y CONTRASTE DE ESCENARIOS
----------------------------------------------------------------------------

Debajo del gráfico de superficie se sitúan las tres ranuras de memoria del
Laboratorio. Las posiciones A y B fotografían el estado completo que se
encuentra activo en pantalla en el momento de pulsar el botón, conservando
su escala, su marco regulatorio, sus políticas, el estado del modo
exploratorio y el valor exacto de las quince palancas. La posición C
reconstruye siempre la referencia canónica oficial (39,722135 años en la
escala de Oficiales), con independencia de los cambios que se hayan hecho en
los controles.

Cuando el usuario guarda al menos dos posiciones, se despliega el panel de
contraste automático. Este bloque mide las diferencias netas en área
integrada, puntuación total, edad estimada y cuota de captación menor de 35
años, dibuja las barras de variación por apartado y permite copiar el
informe comparativo en texto plano al portapapeles.


----------------------------------------------------------------------------
8. EXPLORACIÓN FUERA DE NORMA (+25%), VIDA MEDIA ACADÉMICA Y EQUIDAD
----------------------------------------------------------------------------

Al activar el interruptor de exploración fuera de norma (+25%), los controles
de mérito amplían su recorrido un 25% por encima del tope oficial del BOE y
se desbloquean las herramientas avanzadas de simulación al final de la
columna izquierda.

El módulo de decaimiento académico por vida media permite simular la pérdida
de vigencia de las titulaciones (Grado, Titulación Específica y Posgrado) en
función de los años transcurridos desde que se obtuvieron. El control maestro
recorre seis escalones de exigencia, desde la validez perpetua actual hasta
una vida media de solo tres años. Si se seleccionan presiones severas (vida
media igual o inferior a cinco años), el panel abre automáticamente una
alerta de escenario extremo, y el botón de simulación real dibuja la
retención de puntos aplicando las edades medias de titulación observadas en
España.

A su lado, el control de ajuste global aplica desplazamientos porcentuales
(-50% a +50%) al vuelo sin sobrescribir la configuración manual de las
palancas. El panel enfrenta el reparto uniforme (proporcional al máximo de
cada ítem) contra el reparto proporcional (relativo a los puntos actuales
del candidato) y cuantifica cuánto distorsiona cada método la desigualdad de
partida. Asimismo, cuando el control de cursos administrativos alcanza las
ocho unidades, se muestra el aviso que recuerda el tope normativo de 4,00
puntos en régimen ordinario.


----------------------------------------------------------------------------
9. ORÁCULO DE RECLUTAMIENTO (RESOLUCIÓN ALGEBRAICA DIRECTA)
----------------------------------------------------------------------------

El Oráculo de Reclutamiento resuelve el problema inverso desde el lado de
los méritos del aspirante. En lugar de tanteos manuales, el usuario escribe
la edad exacta que desea obtener dentro de los límites legales de la escala
y pulsa el botón de cálculo.

Dado que la ecuación de edad es una combinación lineal de los cuatro bloques
causales acotada por intervalos cerrados, el algoritmo evalúa los vértices
de saturación de tres bloques y despeja algebraicamente el cuarto,
eligiendo la solución exacta que maximiza la puntuación total del candidato
y trasladándola a los controles deslizantes.

Si la edad solicitada queda fuera del alcance físico de los topes vigentes,
el Oráculo informa de la imposibilidad sin alterar los controles ni borrar
el porcentaje de ajuste global que estuviera activo.


----------------------------------------------------------------------------
10. MOTOR MONTE CARLO, EXCLUSIÓN EN BARRERA Y VISTAS 2D Y 3D
----------------------------------------------------------------------------

El bloque inferior del Laboratorio aloja el motor de proyección estocástica
a futuro. Permite lanzar entre 50.000 y 150.000 trayectorias por cada
posición marcada (A, B o C) a lo largo de un horizonte de 3 a 15 años. En
vez de fabricar errores mediante una campana gaussiana teórica, el motor
extrae al azar con reemplazo los residuos reales de las convocatorias (769
en Ejércitos y 354 en Cuerpos Comunes) mediante un generador determinista de
semilla fija, preservando la asimetría real de cada población.

Las edades mínima y máxima de cada categoría actúan como barreras de
exclusión real. Cuando una trayectoria simulada rebasa el techo legal o cae
por debajo del suelo, el candidato sale de la cohorte activa y pasa a
contabilizarse en el porcentaje de excluidos de ese año, evitando los falsos
acumulados en la frontera que produciría un recorte artificial.

El selector de modo visual permite conmutar el lienzo principal entre el
gráfico de abanico bidimensional (con bandas P10-P90, rango intercuartílico
P25-P75 y rutas individuales) y cinco superficies tridimensionales
interactivas que proyectan la probabilidad de cumplir un umbral de edad
configurable, la amplitud de la incertidumbre, la tasa de rutas válidas
supervivientes, la asimetría distribucional de Bowley y el margen restante
hasta el techo legal. La casilla de deriva empírica reemplaza la pendiente
manual por la observada entre 2023 y 2025 (-0,9847 años por año en Ejércitos
y +0,7369 en Cuerpos Comunes), acompañada de su ficha metodológica en
ventana modal.


----------------------------------------------------------------------------
11. DESGLOSE EN CASCADA, MAPA DE CALOR Y COMPARADOR DE DENSIDADES
----------------------------------------------------------------------------

En la parte inferior de la tarjeta Monte Carlo se ubican dos visores
complementarios para inspeccionar la estructura interna de las proyecciones.

El visor espectral tridimensional apila en perspectiva isométrica los
histogramas completos de edad de cada año proyectado, permitiendo rotar la
figura con el ratón o el dedo para observar cómo se deforma la campana de la
cohorte con el paso del tiempo.

El visor contiguo conmuta entre tres representaciones diagnósticas. La
primera es el gráfico en cascada, que descompone la edad de cualquier
posición guardada sumando a la edad base el aporte exacto de cada bloque de
méritos y de las políticas activas. La segunda es el mapa de calor
bidimensional de densidades anuales, y la tercera es el comparador de
distribuciones de probabilidad, que enfrenta dos posiciones entre sí o el
inicio contra el final del horizonte, calculando el solapamiento de Weitzman
y la distancia de variación total, e informando por separado cuando alguna
de las cohortes se extingue por completo en el límite legal.


----------------------------------------------------------------------------
12. OBSERVATORIO DE SENSIBILIDAD DE POLÍTICAS
----------------------------------------------------------------------------

La segunda pestaña superior traslada el análisis desde el perfil individual
hacia el agregado de la convocatoria. Enfrenta en tres columnas la línea
base histórica 2023-2025 (N = 1.123), la proyección bajo los parámetros de
política seleccionados y el diferencial neto resultante sobre la edad media
de Ejércitos, la edad media de Cuerpos Comunes, la dispersión
intercuartílica y las cuotas de aspirantes menores de 35 años y mayores de
50 años.

En la mitad inferior, el lienzo superpone la curva bimodal de densidad de la
muestra histórica frente a la curva desplazada por la reforma normativa,
manteniendo sus mandos sincronizados en ambos sentidos con el Laboratorio y
con el módulo de Optimización.


----------------------------------------------------------------------------
13. SIMULADOR DE OPORTUNIDADES Y REDISTRIBUCIÓN DE VACANTES
----------------------------------------------------------------------------

La tercera pestaña analiza el problema de las plazas que quedan sin cubrir
en las convocatorias oficiales. Permite filtrar la oferta histórica por
periodo (2023-2025, 2026 o serie completa) y por escala, aplicando una tasa
de vacantes desiertas (iniciada en el 17,52% empírico) y un porcentaje de
recuperación bajo tres posibles criterios de reasignación (prelación de
baremo, proximidad geográfica de la unidad o reserva interna de escala).

La tabla resultante desglosa para Tierra, Armada, Aire y Espacio y Cuerpos
Comunes el total de códigos convocados, las plazas ofertadas, las vacantes
desiertas estimadas, la bolsa de candidatos sin asignación y las plazas que
se cubrirían efectivamente con la regla elegida.


----------------------------------------------------------------------------
14. ALTERNATIVAS DEL ASPIRANTE POR AJUSTE CURRICULAR Y TERRITORIAL
----------------------------------------------------------------------------

La cuarta pestaña opera como un recomendador táctico sobre la convocatoria
2026. El candidato introduce su escala, sus años de experiencia civil, su
titulación académica oficial, su número de cursos administrativos, su familia
funcional y su preferencia geográfica.

El motor evalúa las 440 líneas de plazas de 2026 combinando coincidencia de
palabras clave, ubicación de la unidad y ajuste curricular (adecuación entre
los puntos de experiencia y formación del candidato y la dureza competitiva
de la vacante según sus requisitos y número de plazas). Devuelve las tres
alternativas óptimas de unidades distintas, muestra su continuidad histórica
entre 2023 y 2026 y permite volcar ese perfil directamente al Laboratorio.


----------------------------------------------------------------------------
15. CATÁLOGO LONGITUDINAL BOE 2023-2026 Y ALERTA DE RENUMERACIÓN
----------------------------------------------------------------------------

La quinta pestaña expone la base de datos completa de las cuatro
convocatorias (1.238 registros y 1.550 plazas), sincronizada por defecto con
la categoría seleccionada en la cabecera general y filtrable por año,
ejército, escala y búsqueda textual.

Al pulsar sobre el detalle de cualquier vacante, el sistema cruza dos
índices longitudinales distintos. Por un lado reconstruye la serie histórica
de esa misma unidad, localidad y cometido entre 2023 y 2026; por otro,
verifica todos los usos del código alfanumérico de la plaza. Si detecta que
el Ministerio reutilizó ese mismo código en años distintos para unidades o
escalas diferentes, muestra una advertencia expresa de renumeración para
impedir que se confundan plazas distintas bajo una misma etiqueta.


----------------------------------------------------------------------------
16. OPTIMIZACIÓN MULTICRITERIO DE PARETO Y DECODIFICADOR INVERSO
----------------------------------------------------------------------------

La sexta pestaña agrupa dos motores matemáticos de inversión normativa.

En la parte superior, el optimizador de políticas traza la frontera de
Pareto entre la variación de edad deseada y el grado de intervención sobre
la norma. Ofrece tres criterios de resolución, que son la mínima distorsión cuadrática
conjunta, modificación exclusiva del tope de experiencia civil (respetando
los factores académico y de servicio que estén activos en el Laboratorio) y
mínima cardinalidad en norma L1 (para tocar el menor número posible de
parámetros del BOE). Cada propuesta informa de su valor continuo exacto, su
error de redondeo en la rejilla de controles y su efecto previsto en el
Observatorio.

En la parte inferior, el decodificador inverso recibe una puntuación total
observada de Mérito Específico (de 0 a 50 puntos) y enumera todas las
combinaciones de experiencia, titulación y cursos que pueden haberla
generado bajo los baremos de 2025 y de 2026. Distingue expresamente entre la
familia documental ordinaria (de uno a tres títulos oficiales, que alcanza
un máximo de 41 puntos) y el dominio algebraico agregado (que cubre hasta el
techo legal de 50 puntos), expresando los rangos de años de experiencia
mediante uniones exactas de intervalos matemáticos.


----------------------------------------------------------------------------
17. INFORME IMPRIMIBLE EN PDF Y EXPORTACIÓN REPRODUCIBLE
----------------------------------------------------------------------------

El botón de generación documental compone un informe técnico de tres páginas
en tinta oscura sobre fondo blanco, diseñado para imprimirse en papel o
guardarse como archivo PDF nativo desde el diálogo del navegador.

El contenedor de impresión cuelga directamente del cuerpo raíz del documento
para asegurar un maquetado limpio en cualquier motor de renderizado. Incluye
el resumen ejecutivo del escenario, la comparativa completa de las quince
palancas frente a la referencia, el desglose de políticas, las fichas de las
posiciones guardadas y la sección dedicada al motor Monte Carlo. Si se
ejecuta desde la tarjeta estocástica teniendo activa una vista
tridimensional, el documento coloca el bloque Monte Carlo en primera página
e incorpora tanto el abanico 2D de alta resolución como la captura de la
superficie 3D seleccionada.

Asimismo, la barra de exportación permite descargar los percentiles anuales
en formato CSV, el paquete completo de auditoría en formato JSON (con
vectores de residuos, estado de referencia, semillas e histogramas anuales)
y un guion autónomo para MATLAB (.m) que reproduce bit a bit el generador
pseudoaleatorio y dibuja las gráficas bidimensionales y tridimensionales.


----------------------------------------------------------------------------
18. BATERÍA DE VERIFICACIÓN MATEMÁTICA (T1-T18) Y DIAGNÓSTICO
----------------------------------------------------------------------------

El archivo incorpora una batería automatizada de dieciocho pruebas de
integridad matemática y funcional que puede ejecutarse en cualquier momento
abriendo la consola del navegador (tecla F12) e invocando la función
`window.runDinoMathAuditTests()`.

Esta verificación comprueba la exactitud de la edad inicial de referencia
(39,722135 años), la inmutabilidad del estado base al explorar controles, la
vinculación de 0 y 12 años de experiencia BOE, el centrado en las medias de
calibración de Ejércitos y Comunes, la pureza de la posición C, el respeto
al bloqueo del factor académico en los tres motores de optimización, la
invalidación por firma de contexto, la conservación de masa en las colas de
los histogramas, la notación de intervalos del decodificador, la respuesta
de los botones de equidad, la limpieza del contrato JSON y MATLAB ante pasos
extintos y el funcionamiento del botón de deshacer tras múltiples
aplicaciones. Al terminar la pasada, devuelve todos los controles al estado
exacto que tenían antes de lanzar la prueba.

Para inspeccionar las matrices internas del modelo sin alterar la sesión, la
instrucción `window.dinosaurioDebug()` imprime en consola los vectores de
coeficientes antes y después de la contracción bayesiana junto con sus
errores estándar.


----------------------------------------------------------------------------
19. LÍMITES METODOLÓGICOS Y LECTURA DE RESULTADOS
----------------------------------------------------------------------------

Las cifras que entrega DINOSAURIO 2.0 son estimaciones estadísticas y
proyecciones de simulación construidas sobre el registro histórico de las
convocatorias 2023-2026. No tienen valor vinculante ni sustituyen a las
bases publicadas en el Boletín Oficial del Estado.

Dentro del modelo lineal de edad, siete controles ejercen efecto causal
directo (servicio militar previo, experiencia civil, titulación
universitaria, cursos administrativos, formación general, académica base y
posgrado), mientras que los ocho restantes (idiomas SLP, competencias TIC,
permisos de conducción, motocicleta, ADR, deportista de alto nivel, FCSE y
condición de empleado público) suman puntos descriptivos en el baremo pero
no desplazan por separado la edad estimada, al viajar su efecto integrado en
la media del bloque general de la muestra disponible.

Del mismo modo, la pendiente de deriva empírica 2023-2025 se apoya en solo
tres cortes anuales. Debe emplearse como un contraste de sensibilidad y no
como una proyección demográfica determinista a largo plazo.


----------------------------------------------------------------------------
20. RESOLUCIÓN DE INCIDENCIAS OPERATIVAS
----------------------------------------------------------------------------

Si al abrir el fichero en local la tabla del catálogo muestra un aviso de
incompatibilidad, se debe a que el navegador empleado carece de soporte para
descompresión nativa de flujos GZIP; basta con abrir el archivo en una
versión actualizada de Chrome, Edge, Firefox o Safari.

Cuando el botón de ejecución del motor Monte Carlo permanece bloqueado,
indica que todavía no se ha guardado ninguna posición en las ranuras A, B o
C, o bien que ninguna de sus tres casillas de selección está marcada. Puede
pulsarse el botón de preparación rápida situado bajo el gráfico del
Laboratorio para fijar el caso actual en una ranura libre y bajar
directamente al simulador.

Si tras calcular una receta en el recorrido inverso el botón de aplicación
devuelve un aviso de caducidad, significa que algún control deslizante, la
categoría o el marco normativo cambiaron después de generar la propuesta. El
bloqueo por firma de contexto impide aplicar parámetros calculados para un
escenario distinto; basta con pulsar de nuevo el botón de cálculo de receta.


----------------------------------------------------------------------------
21. REFLEXIÓN FINAL
----------------------------------------------------------------------------

La neutralidad formal de una regla administrativa no equivale a su
neutralidad demográfica. Cuando un sistema de concurso modifica los tramos
de saturación de la experiencia acumulada, redefine quién tiene ventaja
competitiva para ocupar las plazas antes incluso de que se publique la
relación de admitidos.

La utilidad de este simulador no reside en predecir el futuro de una
convocatoria particular, sino en hacer transparente la estructura matemática
que conecta las reglas del Boletín Oficial del Estado con la pirámide de
edad resultante. Lo que antes quedaba oculto tras la intuición informal
pasa ahora a ser auditable, reproducible y medible al milímetro.


============================================================================

          FIN DEL MANUAL · DINOSAURIO 2.0 · MANUEL HERNÁNDEZ

============================================================================
