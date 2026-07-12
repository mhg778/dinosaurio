============================================================================
  DINOSAURIO - MANUAL DE OPERACION DEL SIMULADOR ANALITICO
  ============================================================================

INDICE
------
  1. Mision de la herramienta
  2. Puesta en marcha
  3. Panel de mando: categoria y modo de calculo
  4. El modelo jerarquico bayesiano con contraccion (shrinkage)
  5. Motor de remuestreo con datos reales (bootstrap de residuos)
  6. Sistema de posiciones guardadas (snapshots A, B, C)
  7. Panel de diferencias entre posiciones
  8. Motor Monte Carlo: proyeccion a futuro
  9. Numero de simulaciones: ajuste de la barra de iteraciones
  10. Vista Espectral: lectura de la proyeccion en relieve
  11. Lectura de indicadores clave (KPI)
  12. Exportacion de datos
  13. Advertencias y limites del modelo
  14. Resolucion de incidencias frecuentes


----------------------------------------------------------------------------
1. MISION DE LA HERRAMIENTA
----------------------------------------------------------------------------

DINOSAURIO es un simulador analitico que permite estimar, a partir de los
apartados reales del baremo (Tiempo de Servicio, Meritos Academicos,
Meritos Generales y Merito Especifico), la puntuacion total esperada y su
relacion con la edad media de la vacante seleccionada.

La herramienta no sustituye al baremo oficial ni a la convocatoria vigente.
Es un instrumento de apoyo para entender el terreno antes de presentar
instancia: como se comporta cada apartado y como se mueve la edad estimada
si se modifican las variables de entrada.


----------------------------------------------------------------------------
2. PUESTA EN MARCHA
----------------------------------------------------------------------------

El archivo es autonomo. No requiere conexion a internet ni instalacion de
ningun tipo. Se abre con cualquier navegador moderno (Chrome, Firefox, Edge)
con un doble clic, como cualquier documento.

Si al abrirlo la pantalla queda en blanco o en negro, cierre el navegador
por completo y vuelva a abrir el archivo en una pestana nueva. Verifique
tambien que esta usando la ultima version entregada, porque las versiones
anteriores suelen quedar duplicadas en la carpeta de descargas y es facil
confundirlas.


----------------------------------------------------------------------------
3. PANEL DE MANDO: CATEGORIA Y MODO DE CALCULO
----------------------------------------------------------------------------

En la parte superior se fijan dos parametros de partida, antes de tocar
ningun otro mando:

  - Categoria de vacante: Oficiales, Suboficiales, Tropa y Marineria, o
    Cuerpos Comunes. Cada categoria tiene su propio rango de edad legal
    (suelo y techo) y su propio comportamiento estadistico, heredado de
    los datos reales de convocatoria.

  - Modo de ajuste global: Uniforme o Proporcional. Determina como se
    reparte cualquier desplazamiento de meritos entre los distintos
    apartados. El modo Proporcional respeta el peso relativo de cada
    apartado y es el recomendado por defecto.

Cualquier cambio en estos dos mandos recalcula al instante todos los
paneles inferiores. No hace falta confirmar ni recargar la pagina.


----------------------------------------------------------------------------
4. EL MODELO JERARQUICO BAYESIANO CON CONTRACCION (SHRINKAGE)
----------------------------------------------------------------------------

EL PROBLEMA A RESOLVER
  Los datos disponibles se agrupan en dos bloques: Ejercitos (Oficiales,
  Suboficiales y Tropa, con 769 registros) y Cuerpos Comunes (354
  registros). Cuando un grupo dispone de pocos registros, calcular un
  coeficiente propio para ese grupo aislado da resultados poco fiables,
  sujetos al ruido de la muestra.

LA SOLUCION: CONTRACCION HACIA LA MEDIA GLOBAL
  En vez de usar el coeficiente de cada grupo en solitario, o el
  coeficiente global sin distincion, el modelo combina ambos con un
  factor de ponderacion, siguiendo la logica de los modelos jerarquicos
  bayesianos: cada grupo se apoya en la informacion del conjunto, en
  proporcion a los datos propios de que dispone.

  La formula aplicada es:

      beta_grupo = lambda x beta_grupo_OLS + (1 - lambda) x beta_global

  donde lambda se calcula como:

      lambda = n_grupo / (n_grupo + tau)

  n_grupo es el numero de registros del grupo y tau es el parametro de
  contraccion, fijado en 80. A mas registros propios, lambda se acerca a
  1 y el grupo depende menos de la media global.

VALORES ACTUALES DEL MODELO
      Ejercitos       (n=769):  lambda = 0.906  (contraccion ligera)
      Cuerpos Comunes (n=354):  lambda = 0.816  (contraccion algo mayor)

  Ejercitos conserva casi intacto su propio patron. Cuerpos Comunes se
  apoya algo mas en el comportamiento conjunto, al tener menos registros
  propios sobre los que asentar el calculo.

DONDE SE VE EN PANTALLA
  El recuadro "Modelo jerarquico bayesiano activo" muestra en todo
  momento el valor de lambda de la categoria seleccionada. Si cambia de
  categoria y ese valor no varia, el modelo no se esta aplicando bien.


----------------------------------------------------------------------------
5. MOTOR DE REMUESTREO CON DATOS REALES (BOOTSTRAP DE RESIDUOS)
----------------------------------------------------------------------------

A partir de la version 13, el motor Monte Carlo ha dejado de asumir una
campana de Gauss simetrica para simular el margen de error de cada
estimacion. En su lugar, toma prestado al azar, con reemplazo, uno de los
errores reales observados entre la edad real y la edad que predice el
modelo, para cada uno de los aspirantes de la muestra (769 en Ejercitos,
354 en Cuerpos Comunes).

Esta forma de proceder respeta la asimetria real de los datos: los
residuos de Ejercitos tienen una ligera cola hacia edades mas bajas
(asimetria de -0.26), y los de Cuerpos Comunes hacia edades mas altas
(asimetria de 0.42). Una campana de Gauss perfecta habria borrado esa
asimetria y ofrecido una falsa sensacion de precision.

En la practica, esto no exige ninguna accion por su parte: el motor lo
aplica de forma automatica segun la categoria elegida. Se menciona aqui
para que sepa que las bandas de percentiles que ve en pantalla vienen de
datos de convocatoria reales, no de una formula generica de libro.


----------------------------------------------------------------------------
6. SISTEMA DE POSICIONES GUARDADAS (SNAPSHOTS A, B, C)
----------------------------------------------------------------------------

La herramienta permite guardar hasta tres posiciones de calculo distintas
para comparar escenarios sin perder ninguno de ellos:

  Posicion A y B: se guardan pulsando el boton correspondiente tras
  ajustar los mandos a su gusto. Sirven para enfrentar dos escenarios
  cualquiera, por ejemplo "situacion actual" contra "situacion tras
  reforzar tiempo de servicio".

  Posicion C: guarda de forma automatica la referencia oficial del BOE,
  para usarla como punto fijo de comparacion frente a A o B.

  Boton de intercambio: invierte A y B al instante, sin tener que volver
  a guardar ninguna de las dos.

  Boton de borrado: limpia las tres posiciones guardadas de una sola vez.


----------------------------------------------------------------------------
7. PANEL DE DIFERENCIAS ENTRE POSICIONES
----------------------------------------------------------------------------

En cuanto hay al menos dos posiciones guardadas, aparece de forma
automatica un panel que calcula la diferencia entre ambas: variacion de
edad media, de puntuacion total y de capacidad de captacion de perfiles
jovenes. El boton "Copiar" traslada ese resumen al portapapeles en
formato de texto, listo para pegar en un informe o en un correo.


----------------------------------------------------------------------------
8. MOTOR MONTE CARLO: PROYECCION A FUTURO
----------------------------------------------------------------------------

Este panel proyecta la evolucion de la edad media a lo largo de varios
anos, partiendo de cualquier posicion guardada (A, B o C).

DERIVA ANUAL
  Controla cuanto se espera que varie la edad media cada ano, al margen
  del ruido aleatorio propio de la simulacion.

HORIZONTE
  Numero de anos hacia adelante que se proyectan, ajustable entre 3 y 15.

LECTURA DEL GRAFICO
  Cada posicion activa aparece con su propio color y su propio trazado de
  linea (solido para A, discontinuo para B, punteado para C), de forma
  que se distinguen aun cuando parten de valores muy proximos. Si solo
  hay una posicion activa, se muestran ademas las bandas de percentiles
  P10-P90 y P25-P75, que marcan el rango de dispersion esperado.

COMO ACTIVARLO
  Marque la casilla de cada posicion que desee simular (A, B y/o C) y
  pulse "Ejecutar simulacion". El boton queda bloqueado por diseno hasta
  que haya al menos una posicion guardada y marcada.


----------------------------------------------------------------------------
9. NUMERO DE SIMULACIONES: AJUSTE DE LA BARRA DE ITERACIONES
----------------------------------------------------------------------------

Junto al motor Monte Carlo hay una barra deslizante que fija cuantas
rutas independientes se calculan por cada posicion activa.

  Minimo:            50.000 iteraciones
  Valor por defecto: 100.000 iteraciones
  Maximo:            150.000 iteraciones

A mas iteraciones, los percentiles extremos (P10 y P90) ganan
estabilidad y dejan de moverse de una ejecucion a otra por simple azar de
muestreo. La contrapartida es un tiempo de calculo algo mayor, aunque
sigue siendo cuestion de segundos en cualquier equipo moderno.

El tiempo real de cada ejecucion se muestra siempre en el indicador
"runtime", junto con el numero total de rutas calculadas. Si necesita
maxima precision para un informe formal, suba la barra al tope de
150.000. Si solo quiere hacer pruebas rapidas moviendo sliders, el valor
por defecto de 100.000 es mas que sobrado.


----------------------------------------------------------------------------
10. VISTA ESPECTRAL: LECTURA DE LA PROYECCION EN RELIEVE
----------------------------------------------------------------------------

La Vista Espectral es una pestana alternativa al grafico de abanico
habitual del motor Monte Carlo. Muestra, ano por ano, la distribucion
completa de edades simuladas como una serie de siluetas apiladas en
perspectiva, cada una un poco desplazada respecto a la anterior, de forma
que el conjunto se lee como un relieve de montanas en fila.

QUE APORTA ESTA VISTA
  El abanico de percentiles (P10-P90) resume la dispersion en cuatro
  numeros por ano. La Vista Espectral ensena la forma completa de esa
  dispersion: si es simetrica, si tiene cola hacia un lado, si esta muy
  concentrada o muy repartida. Es la diferencia entre leer un parte
  meteorologico y ver el mapa de nubes completo.

COMO LEERLA
  Cada silueta corresponde a un ano de la proyeccion, ordenadas de
  delante hacia atras desde el ano de partida hasta el horizonte
  elegido. El eje horizontal de cada silueta es la edad. Una linea de
  referencia marca la mediana actual, para que se aprecie de un vistazo
  hacia que lado se desplaza el conjunto con el paso de los anos.

LIMITACION A TENER EN CUENTA
  Esta vista trabaja con una sola posicion a la vez (A, B o C). Si tiene
  varias posiciones activas para comparar, use el abanico multi-linea
  del apartado 8, que es el pensado para ese cometido.


----------------------------------------------------------------------------
11. LECTURA DE INDICADORES CLAVE (KPI)
----------------------------------------------------------------------------

  Edad mediana al final del horizonte: valor central esperado tras el
  numero de anos proyectado.

  Rango P10-P90: intervalo dentro del cual cae la edad media con alta
  probabilidad, descartando los extremos menos probables.

  Capacidad de captacion joven: porcentaje estimado de perfiles por
  debajo de 35 anos bajo el escenario proyectado.

  Runtime: tiempo real de calculo y numero total de rutas simuladas,
  util para comprobar que la simulacion se ha ejecutado con el numero de
  iteraciones que usted mismo ha fijado en la barra del apartado 9.


----------------------------------------------------------------------------
12. EXPORTACION DE DATOS
----------------------------------------------------------------------------

Tras ejecutar una simulacion, dos botones permiten descargar los
resultados completos:

  Exportar CSV (formato Vensim): tabla con percentiles y media por ano y
  por posicion simulada, lista para importar en una hoja de calculo o en
  software de modelado de sistemas.

  Exportar JSON: mismo contenido en formato estructurado, con los
  metadatos de la simulacion incluidos (limites de edad, tiempo de
  ejecucion, posicion de origen, numero de iteraciones empleado).


----------------------------------------------------------------------------
13. ADVERTENCIAS Y LIMITES DEL MODELO
----------------------------------------------------------------------------

  - Los resultados son estimaciones estadisticas, no valores oficiales
    del baremo. El BOE y la convocatoria vigente son siempre la
    referencia legal que manda.

  - El parametro de contraccion (tau=80) del modelo bayesiano se ha
    fijado a partir del volumen de datos disponible hoy. Un cambio
    notable en el tamano de muestra de futuras convocatorias podria
    exigir su revision.

  - El motor Monte Carlo asume una deriva anual constante y reproduce el
    margen de error observado en convocatorias reales, pero no puede
    anticipar cambios normativos futuros ni decisiones de planificacion
    de personal que aun no se han publicado.

  - Las bandas de percentiles se sustituyen por lineas medias cuando hay
    mas de una posicion activa en el abanico, para no saturar el
    grafico. La Vista Espectral, por el mismo motivo, solo trabaja con
    una posicion a la vez.


----------------------------------------------------------------------------
14. RESOLUCION DE INCIDENCIAS FRECUENTES
----------------------------------------------------------------------------

  Pantalla en blanco o negro al abrir el archivo:
    Cierre el navegador por completo y reabra el archivo en una pestana
    nueva. Compruebe que no esta abriendo una version antigua duplicada
    en la carpeta de descargas.

  El boton "Ejecutar simulacion" no responde:
    Compruebe que ha guardado al menos una posicion (A, B o C) y que la
    casilla correspondiente esta marcada. El boton queda bloqueado por
    diseno hasta que se cumplan ambas condiciones.

  El panel de diferencias no aparece:
    Es necesario guardar al menos dos posiciones distintas (por ejemplo
    A y B) para que el panel se active de forma automatica.

  La simulacion tarda mas de lo esperado:
    Revise el valor de la barra de iteraciones. Con el maximo de 150.000
    y varias posiciones activas a la vez, el calculo puede tardar unos
    segundos mas de lo habitual. Es un comportamiento normal, no un
    fallo.

  Los valores de lambda no cambian al cambiar de categoria:
    Abra la consola del navegador (tecla F12) y compruebe el contenido
    de la variable LAMBDA_GRUPO. Si no varia entre categorias, informe
    de la incidencia indicando navegador y sistema operativo empleados.


============================================================================
  FIN DEL MANUAL - DINOSAURIO - POR MANUEL HERNÁNDEZ GONZÁLEZ
============================================================================
