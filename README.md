
================================================================================
  DINOSAURIO - MANUAL DE OPERACION DEL SIMULADOR ANALITICO
  Baremo de Reservistas Voluntarios - Version 11
================================================================================

INDICE
------
  1. Proposito de la herramienta
  2. Puesta en marcha
  3. Panel de mando: categorias y modo de calculo
  4. El modelo jerarquico bayesiano con contraccion (shrinkage)
  5. Sistema de posiciones guardadas (snapshots A, B, C)
  6. Panel de diferencias entre posiciones
  7. Motor Monte Carlo: proyeccion a futuro
  8. Lectura de indicadores clave (KPI)
  9. Exportacion de datos
  10. Advertencias y limites del modelo
  11. Resolucion de incidencias frecuentes


--------------------------------------------------------------------------------
1. PROPOSITO DE LA HERRAMIENTA
--------------------------------------------------------------------------------

DINOSAURIO es un simulador analitico que permite estimar, a partir de los
apartados del baremo (Tiempo de Servicio, Meritos Academicos, Meritos
Generales y Merito Especifico), la puntuacion total esperada y su relacion
con la edad media de la vacante seleccionada.

La herramienta no sustituye al baremo oficial. Es un instrumento de apoyo
para entender como se comportan los distintos apartados y como evolucionan
las cifras si se modifican las variables de entrada.


--------------------------------------------------------------------------------
2. PUESTA EN MARCHA
--------------------------------------------------------------------------------

El archivo es autonomo: no requiere conexion a internet ni instalacion.
Basta con abrirlo con cualquier navegador moderno (Chrome, Firefox, Edge).

Si al abrirlo la pantalla aparece en blanco o negro, cierre por completo el
navegador y vuelva a abrir el archivo en una pestana nueva. Compruebe
tambien que esta abriendo la ultima version entregada, ya que versiones
anteriores pueden quedar duplicadas en la carpeta de descargas.


--------------------------------------------------------------------------------
3. PANEL DE MANDO: CATEGORIAS Y MODO DE CALCULO
--------------------------------------------------------------------------------

En la parte superior se encuentran dos parametros de partida:

  - Categoria de vacante: Oficiales, Suboficiales, Tropa o Cuerpos Comunes.
    Cada categoria tiene sus propios limites de edad (minima y maxima) y
    su propio comportamiento estadistico, heredado de los datos reales
    de convocatoria.

  - Modo de equidad: determina si el calculo prioriza captacion de
    perfiles jovenes, equilibrio entre generaciones, o experiencia.

Cualquier cambio en estos dos mandos recalcula de forma inmediata todos
los paneles inferiores, sin necesidad de confirmar ni recargar la pagina.


--------------------------------------------------------------------------------
4. EL MODELO JERARQUICO BAYESIANO CON CONTRACCION (SHRINKAGE)
--------------------------------------------------------------------------------

EL PROBLEMA A RESOLVER
  Los datos disponibles se agrupan en dos bloques: Ejercitos (Oficiales,
  Suboficiales y Tropa, con 769 registros) y Cuerpos Comunes (354
  registros). Cuando un grupo tiene pocos registros, calcular un
  coeficiente propio para ese grupo aislado puede dar resultados poco
  fiables, sujetos al ruido de la muestra.

LA SOLUCION: CONTRACCION HACIA LA MEDIA GLOBAL
  En vez de usar el coeficiente de cada grupo de forma aislada, o el
  coeficiente global de forma ciega, el modelo combina ambos mediante
  un factor de ponderacion, siguiendo la logica de los modelos
  jerarquicos bayesianos: cada grupo se apoya parcialmente en la
  informacion del conjunto, en proporcion a la cantidad de datos propios
  de que dispone.

  La formula aplicada es:

      beta_grupo = lambda x beta_grupo_OLS + (1 - lambda) x beta_global

  donde lambda se calcula como:

      lambda = n_grupo / (n_grupo + tau)

  Aqui, n_grupo es el numero de registros del grupo, y tau es el
  parametro de contraccion, fijado en 80. Cuanto mayor es n_grupo,
  mas se acerca lambda a 1, y menos se contrae el coeficiente hacia la
  media global.

VALORES ACTUALES DEL MODELO
  Con los datos disponibles, el resultado es:

      Ejercitos      (n=769):  lambda = 0.906  (contraccion ligera)
      Cuerpos Comunes (n=354): lambda = 0.816  (contraccion algo mayor)

  Es decir, "Ejercitos" conserva casi intacto su propio patron estadistico,
  mientras que Cuerpos Comunes se apoya un poco mas en el comportamiento
  agregado de ambos bloques, al disponer de menos registros propios.

DONDE SE VE EN PANTALLA
  El recuadro "Modelo jerarquico bayesiano activo", situado junto a las
  restricciones ineludibles, muestra en todo momento el valor de lambda
  correspondiente a la categoria seleccionada. Si cambia de categoria y
  ese valor no varia, el modelo no esta aplicandose correctamente.

COMO COMPROBARLO DE FORMA MANUAL
  Abriendo la consola del navegador (tecla F12, pestana Console) puede
  escribir directamente:

      LAMBDA_GRUPO
      COEF_MODELOS

  El primero debe devolver los dos valores de lambda citados arriba. El
  segundo debe mostrar los coeficientes ya contraidos, distintos de los
  coeficientes brutos guardados en COEF_OLS.


--------------------------------------------------------------------------------
5. SISTEMA DE POSICIONES GUARDADAS (SNAPSHOTS A, B, C)
--------------------------------------------------------------------------------

La herramienta permite guardar hasta tres posiciones de calculo distintas
para comparar escenarios sin perder ninguno de ellos:

  Posicion A y B: se guardan pulsando los botones correspondientes tras
  ajustar los mandos de entrada a su gusto. Sirven para comparar dos
  escenarios cualquiera, por ejemplo "situacion actual" frente a
  "situacion tras modificar tiempo de servicio".

  Posicion C: guarda automaticamente la referencia oficial del BOE, para
  usarla como punto de comparacion fijo frente a A o B.

  Boton de intercambio: permite invertir A y B de forma instantanea, sin
  tener que volver a guardar ambas posiciones.

  Boton de borrado: limpia las tres posiciones guardadas de una sola vez.


--------------------------------------------------------------------------------
6. PANEL DE DIFERENCIAS ENTRE POSICIONES
--------------------------------------------------------------------------------

En cuanto hay al menos dos posiciones guardadas, aparece de forma
automatica un panel que calcula la diferencia entre ellas: variacion de
edad media, de puntuacion y de capacidad de captacion de perfiles
jovenes. El boton "Copiar" traslada ese resumen al portapapeles en
formato de texto, listo para pegar en un informe o correo.


--------------------------------------------------------------------------------
7. MOTOR MONTE CARLO: PROYECCION A FUTURO
--------------------------------------------------------------------------------

Este panel proyecta la evolucion de la edad media a lo largo de varios
anos, partiendo de cualquier posicion guardada (A, B o C).

ITERACIONES
  El numero de simulaciones independientes por posicion se controla con
  una barra deslizante, ajustable entre 5.000 y 15.000, con un valor por
  defecto de 10.000. A mas iteraciones, mayor estabilidad estadistica en
  los percentiles extremos (P10 y P90), a cambio de un tiempo de calculo
  algo mayor. El tiempo de ejecucion real se muestra siempre en el
  indicador "runtime" tras cada simulacion.

DERIVA ANUAL
  Controla cuanto se espera que varie la edad media cada ano, de forma
  independiente al ruido aleatorio de la simulacion.

HORIZONTE
  Numero de anos hacia adelante que se proyectan, ajustable entre 3 y 15.

LECTURA DEL GRAFICO
  Cada posicion activa aparece con su propio color y su propio trazado
  de linea (solido para A, discontinuo para B, punteado para C), de
  forma que se distinguen incluso cuando parten de valores muy proximos.
  Si solo hay una posicion activa, se muestran ademas las bandas de
  percentiles P10-P90 y P25-P75, que indican el rango de dispersion
  esperado.

COMO ACTIVARLO
  Marque la casilla correspondiente a cada posicion que desee simular
  (A, B y/o C) y pulse "Ejecutar simulacion". El boton permanece
  inactivo mientras no haya al menos una posicion guardada y marcada.


--------------------------------------------------------------------------------
8. LECTURA DE INDICADORES CLAVE (KPI)
--------------------------------------------------------------------------------

  Edad mediana al final del horizonte: valor central esperado tras el
  numero de anos proyectado.

  Rango P10-P90: intervalo dentro del cual se espera que caiga la edad
  media con alta probabilidad, descartando los extremos menos probables.

  Capacidad de captacion joven: porcentaje estimado de perfiles por
  debajo de 35 anos bajo el escenario proyectado.

  Runtime: tiempo real de calculo y numero total de rutas simuladas,
  util para verificar que la simulacion se ha ejecutado con el numero
  de iteraciones esperado.


--------------------------------------------------------------------------------
9. EXPORTACION DE DATOS
--------------------------------------------------------------------------------

Tras ejecutar una simulacion, dos botones permiten descargar los
resultados completos:

  Exportar CSV (formato Vensim): tabla con percentiles y media por ano
  y por posicion simulada, lista para importar en hojas de calculo o
  software de modelado de sistemas.

  Exportar JSON: mismo contenido en formato estructurado, incluyendo
  metadatos de la simulacion (limites de edad, tiempo de ejecucion,
  posicion de origen).


--------------------------------------------------------------------------------
10. ADVERTENCIAS Y LIMITES DEL MODELO
--------------------------------------------------------------------------------

  - Los resultados son estimaciones estadisticas, no valores oficiales
    del baremo. El BOE y la convocatoria vigente son siempre la
    referencia legal aplicable.

  - El parametro de contraccion (tau=80) del modelo bayesiano ha sido
    fijado a partir del volumen de datos disponible. Un cambio en el
    tamano de muestra de futuras convocatorias podria requerir su
    revision.

  - El motor Monte Carlo asume una deriva anual constante y un nivel de
    ruido calculado a partir de la dispersion historica observada; no
    incorpora cambios normativos futuros ni decisiones de planificacion
    de personal.

  - Las bandas de percentiles se degradan a lineas medias cuando hay mas
    de una posicion activa, para mantener la legibilidad del grafico.


--------------------------------------------------------------------------------
11. RESOLUCION DE INCIDENCIAS FRECUENTES
--------------------------------------------------------------------------------

  Pantalla en blanco o negro al abrir el archivo:
    Cierre el navegador por completo y reabra el archivo en pestana
    nueva. Compruebe que no esta abriendo una version antigua duplicada.

  El boton "Ejecutar simulacion" no responde:
    Compruebe que ha guardado al menos una posicion (A, B o C) y que la
    casilla correspondiente esta marcada. El boton permanece bloqueado
    por diseno hasta que se cumplan ambas condiciones.

  El panel de diferencias no aparece:
    Es necesario guardar al menos dos posiciones distintas (por ejemplo
    A y B) para que el panel se active de forma automatica.

  Los valores de lambda no cambian al cambiar de categoria:
    Abra la consola del navegador (F12) y compruebe el contenido de la
    variable LAMBDA_GRUPO. Si no varia entre categorias, comunique la
    incidencia junto con el navegador y sistema operativo empleado.


================================================================================
  FIN DEL MANUAL - VERSION 11 - DINOSAURIO - POR MANUEL HERNÁNDEZ
================================================================================
