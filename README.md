============================================================================

  DINOSAURIO - MANUAL DE OPERACIÓN DEL SIMULADOR ANALÍTICO
  Versión 1.18 (corregida)

============================================================================

ÍNDICE
------
  1. Misión de la herramienta
  2. Puesta en marcha
  3. Panel de mando: categoría y modo de cálculo
  4. El modelo jerárquico bayesiano con contracción (shrinkage)
  5. Motor de remuestreo con datos reales (bootstrap de residuos)
  6. Sistema de posiciones guardadas (snapshots A, B, C)
  7. Panel de diferencias entre posiciones
  8. Modo exploración fuera de norma (+25%)
  9. Decaimiento temporal académico (Half-Life)
  10. Ajuste global de sensibilidad (no destructivo)
  11. Waterfall de la edad: desglose exacto por bloque
  12. Oráculo de Reclutamiento: simulador inverso
  13. Motor Monte Carlo: proyección a futuro
  14. Deriva empírica real (2023-2025)
  15. Número de simulaciones: ajuste de la barra de iteraciones
  16. Vista Espectral: lectura de la proyección en relieve
  17. Lectura de indicadores clave (KPI)
  18. Generación de informe imprimible
  19. Exportación de datos
  20. Advertencias y límites del modelo
  21. Resolución de incidencias frecuentes
  22. Historial de correcciones (versión 8)


----------------------------------------------------------------------------
1. MISIÓN DE LA HERRAMIENTA
----------------------------------------------------------------------------

DINOSAURIO es un simulador analítico que permite estimar, a partir de los
apartados reales del baremo (Tiempo de Servicio, Méritos Académicos,
Méritos Generales y Mérito Específico), la puntuación total esperada y su
relación con la edad media de la vacante seleccionada.

La herramienta no sustituye al baremo oficial ni a la convocatoria vigente.
Es un instrumento de apoyo para entender el terreno antes de presentar
instancia: cómo se comporta cada apartado y cómo se mueve la edad estimada
si se modifican las variables de entrada.


----------------------------------------------------------------------------
2. PUESTA EN MARCHA
----------------------------------------------------------------------------

El archivo es autónomo. No requiere conexión a internet ni instalación de
ningún tipo. Se abre con cualquier navegador moderno (Chrome, Firefox, Edge)
con un doble clic, como cualquier documento.

Si al abrirlo la pantalla queda en blanco o en negro, cierre el navegador
por completo y vuelva a abrir el archivo en una pestaña nueva. Verifique
también que está usando la última versión entregada, porque las versiones
anteriores suelen quedar duplicadas en la carpeta de descargas y es fácil
confundirlas.

Los gráficos de superficie, Waterfall, abanico Monte Carlo y Vista
Espectral se redibujan ahora a la resolución física real de la pantalla
(Retina, 4K), por lo que se ven nítidos en cualquier equipo moderno sin
ajuste adicional por su parte.


----------------------------------------------------------------------------
3. PANEL DE MANDO: CATEGORÍA Y MODO DE CÁLCULO
----------------------------------------------------------------------------

En la parte superior se fijan dos parámetros de partida, antes de tocar
ningún otro mando:

  - Categoría de vacante: Oficiales, Suboficiales, Tropa y Marinería, o
    Cuerpos Comunes. Cada categoría tiene su propio rango de edad legal
    (suelo y techo) y su propio comportamiento estadístico, heredado de
    los datos reales de convocatoria.

  - Modo de ajuste global: Uniforme o Proporcional. Determina cómo se
    reparte cualquier desplazamiento de méritos entre los distintos
    apartados. El modo Proporcional respeta el peso relativo de cada
    apartado y es el recomendado por defecto.

Cualquier cambio en estos dos mandos recalcula al instante todos los
paneles inferiores. No hace falta confirmar ni recargar la página.


----------------------------------------------------------------------------
4. EL MODELO JERÁRQUICO BAYESIANO CON CONTRACCIÓN (SHRINKAGE)
----------------------------------------------------------------------------

EL PROBLEMA A RESOLVER
  Los datos disponibles se agrupan en dos bloques: Ejércitos (Oficiales,
  Suboficiales y Tropa, con 769 registros) y Cuerpos Comunes (354
  registros). Cuando un grupo dispone de pocos registros, calcular un
  coeficiente propio para ese grupo aislado da resultados poco fiables,
  sujetos al ruido de la muestra.

LA SOLUCIÓN: CONTRACCIÓN HACIA LA MEDIA GLOBAL
  En vez de usar el coeficiente de cada grupo en solitario, o el
  coeficiente global sin distinción, el modelo combina ambos con un
  factor de ponderación, siguiendo la lógica de los modelos jerárquicos
  bayesianos: cada grupo se apoya en la información del conjunto, en
  proporción a los datos propios de que dispone.

  La fórmula aplicada es:

      beta_grupo = lambda x beta_grupo_OLS + (1 - lambda) x beta_global

  donde lambda se calcula como:

      lambda = n_grupo / (n_grupo + tau)

  n_grupo es el número de registros del grupo y tau es el parámetro de
  contracción, fijado en 80. A más registros propios, lambda se acerca a
  1 y el grupo depende menos de la media global.

VALORES ACTUALES DEL MODELO
      Ejércitos       (n=769):  lambda = 0.906  (contracción ligera)
      Cuerpos Comunes (n=354):  lambda = 0.816  (contracción algo mayor)

  Ejércitos conserva casi intacto su propio patrón. Cuerpos Comunes se
  apoya algo más en el comportamiento conjunto, al tener menos registros
  propios sobre los que asentar el cálculo.

DÓNDE SE VE EN PANTALLA
  El recuadro "Modelo jerárquico bayesiano activo" muestra en todo
  momento el valor de lambda de la categoría seleccionada. Si cambia de
  categoría y ese valor no varía, el modelo no se está aplicando bien.

AVISO DE CALIBRACIÓN (NUEVO)
  Justo debajo aparece, solo cuando procede, un recuadro rojo de "Aviso
  de calibración". Se activa automáticamente si la suma de los valores
  de partida de Experiencia + Titulación + Cursos no coincide con la
  constante de calibración interna del modelo para Mérito Específico.
  Cuando esto ocurre, la edad que se ve al cargar la página, antes de
  tocar ningún slider, no coincide exactamente con la "edad base"
  anunciada arriba. El recuadro explica el desfase exacto en años, sin
  alterar ningún valor por defecto para forzar el cuadre: es preferible
  mostrar la discrepancia real que fabricar una cifra de muestra que no
  se pueda verificar.


----------------------------------------------------------------------------
5. MOTOR DE REMUESTREO CON DATOS REALES (BOOTSTRAP DE RESIDUOS)
----------------------------------------------------------------------------

El motor Monte Carlo no asume una campana de Gauss simétrica para simular
el margen de error de cada estimación. En su lugar, toma prestado al
azar, con reemplazo, uno de los errores reales observados entre la edad
real y la edad que predice el modelo, para cada uno de los aspirantes de
la muestra (769 en Ejércitos, 354 en Cuerpos Comunes).

Esta forma de proceder respeta la asimetría real de los datos: los
residuos de Ejércitos tienen una ligera cola hacia edades más bajas
(asimetría de -0.26), y los de Cuerpos Comunes hacia edades más altas
(asimetría de 0.42). Una campana de Gauss perfecta habría borrado esa
asimetría y ofrecido una falsa sensación de precisión.

En la práctica, esto no exige ninguna acción por su parte: el motor lo
aplica de forma automática según la categoría elegida.

MURO LEGAL SUSTITUIDO POR EXCLUSIÓN REAL (CORREGIDO EN V8)
  En versiones anteriores, cuando una trayectoria simulada alcanzaba el
  techo o el suelo legal de la escala, se quedaba artificialmente
  "congelada" en ese límite para el resto de la proyección. Esto
  generaba un pico de densidad irreal justo en la frontera de edad, que
  no corresponde a ningún fenómeno real: en la práctica, quien supera el
  límite de edad legal queda excluido del proceso de selección, no se
  queda "esperando" en el límite.

  Ahora, cuando una trayectoria supera el techo o cae por debajo del
  suelo legal, queda marcada como excluida y deja de contar en los
  percentiles, la media y el histograma de densidad de ese año en
  adelante. El indicador de edad mediana muestra, cuando procede, el
  porcentaje de trayectorias excluidas en el último año proyectado.


----------------------------------------------------------------------------
6. SISTEMA DE POSICIONES GUARDADAS (SNAPSHOTS A, B, C)
----------------------------------------------------------------------------

La herramienta permite guardar hasta tres posiciones de cálculo distintas
para comparar escenarios sin perder ninguno de ellos:

  Posición A y B: se guardan pulsando el botón correspondiente tras
  ajustar los mandos a su gusto. Sirven para enfrentar dos escenarios
  cualquiera, por ejemplo "situación actual" contra "situación tras
  reforzar tiempo de servicio".

  Posición C: guarda de forma automática la referencia oficial del BOE,
  para usarla como punto fijo de comparación frente a A o B.

  Botón de intercambio: invierte A y B al instante, sin tener que volver
  a guardar ninguna de las dos.

  Botón de borrado: limpia las tres posiciones guardadas de una sola vez.

CADA POSICIÓN GUARDA SU PROPIO ESCENARIO Y CATEGORÍA (CORREGIDO EN V8)
  Antes, si guardaba una posición con el modo "exploración fuera de
  norma" activado, la posición terminaba almacenando los valores
  normativos en vez de los del escenario real que estaba viendo en
  pantalla. Ahora cada posición recuerda fielmente si se guardó en modo
  normativo o en modo exploración, y ese dato se refleja correctamente
  en el Waterfall y en el resto de paneles que la consultan.

  Del mismo modo, si guarda posiciones en distintas categorías de
  vacante (por ejemplo A en Tropa y B en Cuerpos Comunes), cada una
  conserva su propio límite legal y su propio modelo estadístico al
  proyectarla en el motor Monte Carlo, en vez de heredar por error la
  categoría que esté activa en pantalla en ese momento.


----------------------------------------------------------------------------
7. PANEL DE DIFERENCIAS ENTRE POSICIONES
----------------------------------------------------------------------------

En cuanto hay al menos dos posiciones guardadas, aparece de forma
automática un panel que calcula la diferencia entre ambas: variación de
edad media, de puntuación total y de capacidad de captación de perfiles
jóvenes. El botón "Copiar" traslada ese resumen al portapapeles en
formato de texto, listo para pegar en un informe o en un correo.


----------------------------------------------------------------------------
8. MODO EXPLORACIÓN FUERA DE NORMA (+25%)
----------------------------------------------------------------------------

El interruptor situado bajo el panel de mando permite subir cada slider
hasta un 25% por encima de su tope oficial, para simular escenarios
hipotéticos. La zona ampliada se marca en rojo rayado en cada barra, y
ningún valor alcanzado ahí representa una puntuación válida en
convocatoria real.

Al activar este modo aparecen automáticamente tres paneles adicionales,
descritos en los apartados 9, 10 y 12 de este manual, que solo tienen
sentido en el contexto de un escenario exploratorio.


----------------------------------------------------------------------------
9. DECAIMIENTO TEMPORAL ACADÉMICO (HALF-LIFE)
----------------------------------------------------------------------------

Este panel, visible solo en modo exploración fuera de norma, permite
simular cómo pierde valor con el tiempo un título académico, usando una
curva de decaimiento exponencial (vida media, o "half-life") en vez de
restar puntos de forma lineal e inventada.

SLIDER MAESTRO DE PRESIÓN
  Tiene 6 niveles fijos: desactivado (sin decaimiento), y cinco niveles
  de presión creciente (H=20, H=12, H=8, H=5, H=3 años de vida media).
  A menor H, más rápido pierde valor el título con los años.

AÑOS DESDE LA OBTENCIÓN
  Tres controles independientes fijan cuántos años han pasado desde el
  Grado, el CFGS y el Máster/Doctorado. Vienen precargados con valores
  típicos (12, 15 y 6 años respectivamente) para que el efecto de la
  presión sea visible de inmediato al activar el modo: si estos tres
  controles estuvieran en cero, el slider maestro no tendría nada que
  decaer y no se apreciaría ningún cambio.

SIMULACIÓN REAL (MUESTRA 2025)
  Un botón adicional muestra, con las edades medias de titulación reales
  (INE, EOI, CSD), qué porcentaje de puntuación conservaría un aspirante
  típico de cada categoría bajo la presión elegida.


----------------------------------------------------------------------------
10. AJUSTE GLOBAL DE SENSIBILIDAD (NO DESTRUCTIVO)
----------------------------------------------------------------------------

Este panel aplica un desplazamiento porcentual a todos los méritos a la
vez, en modo Uniforme o Proporcional, para comparar el efecto de ambos
criterios sobre la igualdad de oportunidades.

CORREGIDO EN V8: YA NO BORRA SU CONFIGURACIÓN MANUAL
  En versiones anteriores, mover este slider sobrescribía de forma
  permanente el valor de cada slider individual. Si había dedicado
  tiempo a configurar un perfil concreto y después tocaba el ajuste
  global por curiosidad, perdía esa configuración sin posibilidad de
  deshacer.

  Ahora el ajuste global se aplica matemáticamente "al vuelo" en cada
  recálculo, sin tocar nunca el valor guardado en los sliders
  individuales. Al devolver el ajuste global a 0%, recupera exactamente
  la configuración manual que tenía antes de tocarlo.


----------------------------------------------------------------------------
11. WATERFALL DE LA EDAD: DESGLOSE EXACTO POR BLOQUE
----------------------------------------------------------------------------

Sustituye al antiguo "Mapa de calor 2D". A partir de un botón en el panel
del motor Monte Carlo, genera un gráfico en cascada que descompone la
edad estimada de un snapshot guardado en su edad base más la
contribución exacta de cada uno de los cuatro apartados reales del
modelo (Tiempo de Servicio, Académicos, Generales, Mérito Específico).

No es una simulación ni una nube de sensibilidad: es el cálculo exacto,
barra a barra, de cómo se llega del punto de partida al resultado final,
con el coeficiente estadístico y su significancia mostrados junto a cada
barra. Si el snapshot se guardó en modo exploración (+25%), el propio
gráfico lo indica en el título y desglosa los valores de ese escenario,
no los normativos.


----------------------------------------------------------------------------
12. ORÁCULO DE RECLUTAMIENTO: SIMULADOR INVERSO
----------------------------------------------------------------------------

Panel visible en modo exploración fuera de norma. En vez de mover
sliders a mano para acercarse a una edad objetivo, aquí se introduce
directamente la edad que se desea alcanzar y el Oráculo calcula la
combinación de méritos que produce esa edad de forma exacta, maximizando
al mismo tiempo el total de puntos.

CÓMO FUNCIONA
  El modelo de edad es una función lineal de los puntos por apartado, lo
  que permite resolver el problema mediante álgebra directa (un sistema
  de ecuaciones con límites de rango), no mediante ensayo y error
  aleatorio. El resultado se aplica de inmediato a los sliders
  correspondientes.

SI EL OBJETIVO ES INALCANZABLE
  El Oráculo lo indica de forma explícita, en vez de devolver una
  aproximación silenciosa. Puede ocurrir si la edad pedida está fuera
  del rango legal de la escala, o fuera del rango que permiten los
  límites actuales de los sliders (en cuyo caso, activar el modo fuera
  de norma amplía el margen disponible).

  El Oráculo trabaja únicamente sobre los coeficientes agregados del
  modelo estadístico, no sobre los perfiles individuales de los
  aspirantes reales de la muestra (ese nivel de detalle no está
  disponible, solo los datos agregados por apartado).


----------------------------------------------------------------------------
13. MOTOR MONTE CARLO: PROYECCIÓN A FUTURO
----------------------------------------------------------------------------

Este panel proyecta la evolución de la edad media a lo largo de varios
años, partiendo de cualquier posición guardada (A, B o C).

DERIVA ANUAL
  Controla cuánto se espera que varíe la edad media cada año, al margen
  del ruido aleatorio propio de la simulación. Véase el apartado 14 para
  la alternativa de deriva empírica real.

HORIZONTE
  Número de años hacia adelante que se proyectan, ajustable entre 3 y 15.

LECTURA DEL GRÁFICO
  Cada posición activa aparece con su propio color y su propio trazado de
  línea (sólido para A, discontinuo para B, punteado para C), de forma
  que se distinguen aun cuando parten de valores muy próximos.

BANDAS DE INCERTIDUMBRE VISIBLES CON VARIAS POSICIONES A LA VEZ
  (CORREGIDO EN V8)
  Antes, la banda de percentiles P10-P90 solo se dibujaba cuando había
  una única posición activa; con varias a la vez, solo se veían las
  líneas de media, lo que podía dar la falsa impresión de que la
  simulación "no hacía nada" (una línea recta si la deriva es cero,
  aunque la dispersión real crezca año a año). Ahora esa banda también
  se dibuja con varias posiciones activas, en un tono más tenue para no
  saturar el gráfico, para que la incertidumbre real siga siendo
  visible en todos los casos.

CÓMO ACTIVARLO
  Marque la casilla de cada posición que desee simular (A, B y/o C) y
  pulse "Ejecutar simulación". El botón queda bloqueado por diseño hasta
  que haya al menos una posición guardada y marcada.


----------------------------------------------------------------------------
14. DERIVA EMPÍRICA REAL (2023-2025)
----------------------------------------------------------------------------

Junto al slider manual de deriva anual hay una casilla "Usar deriva
empírica real 2023-2025". Al activarla, cada posición simulada deja de
usar el valor manual del slider y pasa a usar una deriva estimada a
partir de fechas de nacimiento reales de los aspirantes Clasificados de
las tres últimas convocatorias resueltas (n=373 en 2023, n=350 en 2024,
n=400 en 2025), calculada por separado para Ejércitos y para Cuerpos
Comunes.

VALORES ACTUALES
      Ejércitos:       -0.98 años/año  (la cohorte se rejuvenece)
      Cuerpos Comunes: +0.74 años/año  (la cohorte envejece)

AVISO OBLIGATORIO SOBRE ESTA CIFRA
  Con solo tres convocatorias de datos, esta pendiente no tiene respaldo
  estadístico robusto: un solo año atípico puede invertir el signo por
  completo. De hecho, en Ejércitos el patrón real ni siquiera es
  monótono (la edad baja de 2023 a 2024 y vuelve a subir ligeramente de
  2024 a 2025). Trátela como una estimación orientativa de orden de
  magnitud, no como un hecho estadístico consolidado. Cuando se disponga
  de cinco o más convocatorias, esta estimación podrá refinarse.

  Al activar la casilla, el slider manual se atenúa visualmente y queda
  deshabilitado; el texto de estado, al finalizar la simulación, indica
  explícitamente qué deriva se aplicó a cada posición.


----------------------------------------------------------------------------
15. NÚMERO DE SIMULACIONES: AJUSTE DE LA BARRA DE ITERACIONES
----------------------------------------------------------------------------

Junto al motor Monte Carlo hay una barra deslizante que fija cuántas
rutas independientes se calculan por cada posición activa.

  Mínimo:            50.000 iteraciones
  Valor por defecto: 100.000 iteraciones
  Máximo:            150.000 iteraciones

A más iteraciones, los percentiles extremos (P10 y P90) ganan
estabilidad y dejan de moverse de una ejecución a otra por simple azar de
muestreo. La contrapartida es un tiempo de cálculo algo mayor, aunque
sigue siendo cuestión de segundos en cualquier equipo moderno.

El tiempo real de cada ejecución se muestra siempre en el indicador
"runtime", junto con el número total de rutas calculadas. Si necesita
máxima precisión para un informe formal, suba la barra al tope de
150.000. Si solo quiere hacer pruebas rápidas moviendo sliders, el valor
por defecto de 100.000 es más que sobrado.

NOTA DE VISIBILIDAD (CORREGIDO EN V8)
  Este slider, junto con el de deriva anual y el de horizonte, tenía un
  fallo de estilo que dejaba invisible la barra deslizante (solo se veía
  el círculo de arrastre, sin ningún trazo detrás). Ahora los tres
  controles muestran su barra correctamente, como el resto de sliders
  del panel.


----------------------------------------------------------------------------
16. VISTA ESPECTRAL: LECTURA DE LA PROYECCIÓN EN RELIEVE
----------------------------------------------------------------------------

La Vista Espectral es una pestaña alternativa al gráfico de abanico
habitual del motor Monte Carlo. Muestra, año por año, la distribución
completa de edades simuladas como una serie de siluetas apiladas en
perspectiva, cada una un poco desplazada respecto a la anterior, de forma
que el conjunto se lee como un relieve de montañas en fila.

QUÉ APORTA ESTA VISTA
  El abanico de percentiles (P10-P90) resume la dispersión en cuatro
  números por año. La Vista Espectral enseña la forma completa de esa
  dispersión: si es simétrica, si tiene cola hacia un lado, si está muy
  concentrada o muy repartida.

CÓMO LEERLA
  Cada silueta corresponde a un año de la proyección, ordenadas de
  delante hacia atrás desde el año de partida hasta el horizonte
  elegido. El eje horizontal de cada silueta es la edad. Una línea de
  referencia marca la mediana actual, para que se aprecie de un vistazo
  hacia qué lado se desplaza el conjunto con el paso de los años.

LIMITACIÓN A TENER EN CUENTA
  Esta vista trabaja con una sola posición a la vez (A, B o C). Si tiene
  varias posiciones activas para comparar, use el abanico multilínea
  del apartado 13, que es el pensado para ese cometido.


----------------------------------------------------------------------------
17. LECTURA DE INDICADORES CLAVE (KPI)
----------------------------------------------------------------------------

  Edad mediana al final del horizonte: valor central esperado tras el
  número de años proyectado. Si hay trayectorias excluidas por superar
  el límite legal (véase apartado 5), el porcentaje excluido se muestra
  junto a la cifra.

  Rango P10-P90: intervalo dentro del cual cae la edad media con alta
  probabilidad, descartando los extremos menos probables.

  Capacidad de captación joven: porcentaje estimado de perfiles por
  debajo de 35 años bajo el escenario proyectado. Con varias posiciones
  activas a la vez, se muestra el valor real de cada una por separado
  (antes, este recuadro se sustituía por un simple contador de
  posiciones, un error corregido en la versión 8).

  Runtime: tiempo real de cálculo y número total de rutas simuladas,
  útil para comprobar que la simulación se ha ejecutado con el número de
  iteraciones que usted mismo ha fijado en la barra del apartado 15.


----------------------------------------------------------------------------
18. GENERACIÓN DE INFORME IMPRIMIBLE
----------------------------------------------------------------------------

Junto al comparador de posiciones hay tres casillas (A, B, Ref. BOE) y un
botón "Generar Informe". Marque las posiciones que quiera documentar y
pulse el botón: se abrirá el diálogo de impresión del navegador con un
documento en fondo blanco y tinta oscura, pensado para entregar en papel
o exportar a PDF (use la opción "Guardar como PDF" del propio diálogo de
impresión de su navegador).

QUÉ INCLUYE EL INFORME
  Por cada posición marcada: la categoría de escala, el escenario
  (norma vigente o exploración +25%), los puntos totales, la edad
  estimada, el área integrada y un desglose por bloque con un gráfico de
  superficie en tinta oscura sobre fondo blanco. Si marca exactamente dos
  posiciones, se añade además una tabla comparativa entre ambas.

  Al final del documento, una nota breve indica el modelo estadístico
  empleado y aclara que los valores de exploración fuera de norma no
  representan una puntuación válida en convocatoria real.

QUÉ NO INCLUYE EL INFORME (A PROPÓSITO)
  El informe no reproduce los sliders, botones ni el fondo oscuro de la
  interfaz de trabajo, ni el bloque de disclaimer o de autoría de esta
  herramienta. El documento resultante queda en blanco, listo para que
  quien lo genere lo revise, lo firme y asuma la responsabilidad de su
  contenido antes de distribuirlo.


----------------------------------------------------------------------------
19. EXPORTACIÓN DE DATOS
----------------------------------------------------------------------------

Tras ejecutar una simulación, dos botones permiten descargar los
resultados completos:

  Exportar CSV (formato Vensim): tabla con percentiles y media por año y
  por posición simulada, lista para importar en una hoja de cálculo o en
  software de modelado de sistemas.

  Exportar JSON: mismo contenido en formato estructurado, con los
  metadatos de la simulación incluidos (límites de edad, tiempo de
  ejecución, posición de origen, número de iteraciones empleado).


----------------------------------------------------------------------------
20. ADVERTENCIAS Y LÍMITES DEL MODELO
----------------------------------------------------------------------------

  - Los resultados son estimaciones estadísticas, no valores oficiales
    del baremo. El BOE y la convocatoria vigente son siempre la
    referencia legal que manda.

  - El parámetro de contracción (tau=80) del modelo bayesiano se ha
    fijado a partir del volumen de datos disponible hoy. Un cambio
    notable en el tamaño de muestra de futuras convocatorias podría
    exigir su revisión.

  - El motor Monte Carlo reproduce el margen de error observado en
    convocatorias reales, pero no puede anticipar cambios normativos
    futuros ni decisiones de planificación de personal que aún no se han
    publicado. La deriva empírica del apartado 14 es orientativa, no
    concluyente, por estar calculada sobre solo tres convocatorias.

  - Los sliders de Idiomas, TIC, Carnets, Moto, ADR, Deportista, FCSE y
    Funcionariado son informativos: no afectan a la edad estimada
    porque su peso ya está incluido en el apartado agregado de Méritos
    Generales. Cada uno de ellos muestra ahora un aviso visual junto a
    su nombre para dejarlo claro (corrección incorporada en la versión
    8, antes no había ningún aviso y el efecto podía parecer un fallo).

  - Las bandas de percentiles se muestran de forma más tenue cuando hay
    más de una posición activa en el abanico, para no saturar el
    gráfico, pero ya no desaparecen del todo. La Vista Espectral, por
    diseño, solo trabaja con una posición a la vez.

  - El Oráculo de Reclutamiento y el aviso de calibración trabajan sobre
    los coeficientes agregados del modelo, no sobre los perfiles
    individuales de los aspirantes de la muestra real, que no están
    disponibles a ese nivel de detalle.


----------------------------------------------------------------------------
21. RESOLUCIÓN DE INCIDENCIAS FRECUENTES
----------------------------------------------------------------------------

  Pantalla en blanco o negro al abrir el archivo:
    Cierre el navegador por completo y reabra el archivo en una pestaña
    nueva. Compruebe que no está abriendo una versión antigua duplicada
    en la carpeta de descargas.

  El botón "Ejecutar simulación" no responde:
    Compruebe que ha guardado al menos una posición (A, B o C) y que la
    casilla correspondiente está marcada. El botón queda bloqueado por
    diseño hasta que se cumplan ambas condiciones.

  El panel de diferencias no aparece:
    Es necesario guardar al menos dos posiciones distintas (por ejemplo
    A y B) para que el panel se active de forma automática.

  La simulación tarda más de lo esperado:
    Revise el valor de la barra de iteraciones. Con el máximo de 150.000
    y varias posiciones activas a la vez, el cálculo puede tardar unos
    segundos más de lo habitual. Es un comportamiento normal, no un
    fallo.

  El slider maestro de Half-Life no parece cambiar nada:
    Compruebe que los tres controles de "años desde la obtención" (Grado,
    CFGS, Máster) no están en cero. Sin antigüedad no hay nada que
    decaer, aunque suba la presión al máximo. Vienen precargados por
    defecto precisamente para evitar esta confusión.

  El botón "Generar Informe" no produce ningún documento visible:
    Compruebe que al menos una de las tres casillas (A, B, Ref. BOE)
    está marcada y que la posición correspondiente está guardada. El
    mensaje de estado bajo el botón indica el motivo exacto si el
    informe no puede generarse.

  El Oráculo indica que el objetivo es "inalcanzable":
    La edad pedida está fuera del rango legal de la escala seleccionada,
    o fuera de lo que permiten los límites actuales de los sliders.
    Pruebe a activar el modo exploración fuera de norma para ampliar el
    margen disponible, o elija una edad más cercana a la edad base de la
    categoría.

  Los valores de lambda no cambian al cambiar de categoría:
    Abra la consola del navegador (tecla F12) y compruebe el contenido
    de la variable LAMBDA_GRUPO. Si no varía entre categorías, informe
    de la incidencia indicando navegador y sistema operativo empleados.


----------------------------------------------------------------------------
22. HISTORIAL DE CORRECCIONES (VERSIÓN 8)
----------------------------------------------------------------------------

Esta versión corrige, respecto a la anterior entregada:

  - Un error de precedencia que mostraba "NaN años" en el pie de la
    Vista Espectral en determinadas condiciones.
  - La pérdida de fidelidad de las posiciones guardadas en modo
    exploración fuera de norma (guardaban valores normativos en vez de
    los reales del escenario visto en pantalla).
  - La contaminación cruzada de categorías en el motor Monte Carlo
    cuando se simulaban posiciones guardadas en distintas escalas a la
    vez.
  - Un error de unidades en el panel de ajuste global que sumaba
    "número de cursos" directamente como si fueran puntos.
  - La sustitución del contador de posiciones activas en el indicador
    de "Captación joven", que ocultaba el dato demográfico real.
  - La destrucción silenciosa de la configuración manual de sliders al
    mover el ajuste global de sensibilidad.
  - El pico de densidad irreal en el límite legal de edad del motor
    Monte Carlo (sustituido por exclusión real de trayectorias).
  - La invisibilidad de las bandas de incertidumbre con varias
    posiciones Monte Carlo activas a la vez.
  - La falta de contraste visual en tres barras deslizantes del panel
    Monte Carlo (iteraciones, deriva, horizonte).
  - La ausencia de aviso en los sliders informativos que no afectan a
    la edad estimada.
  - Código duplicado y funciones sin uso en el motor de cálculo interno,
    sin efecto visible pero relevante para el mantenimiento futuro.

Y añade como funciones nuevas: el panel de decaimiento Half-Life, el
Waterfall de desglose exacto por bloque, el Oráculo de Reclutamiento, la
opción de deriva empírica real basada en convocatorias 2023-2025, el
aviso de calibración en vivo, el escalado de alta resolución de los
gráficos, y el módulo de generación de informe imprimible.


============================================================================

          FIN DEL MANUAL - DINOSAURIO - POR MANUEL HERNÁNDEZ

============================================================================
