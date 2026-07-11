MANUAL DE USUARIO
HERRAMIENTA DINOSAURIO - SIMULADOR DE EDAD MEDIA EN CONVOCATORIAS DE RESERVISTAS
Version orientada a personal de Defensa sin conocimientos tecnicos previos

===========================================================================
1. PROPOSITO DE LA HERRAMIENTA
===========================================================================

Esta herramienta permite estimar la edad media de un aspirante en funcion
de los meritos que aporte al baremo, y proyectar como podria evolucionar
esa edad en convocatorias futuras. No sustituye ningun calculo oficial.
Es un instrumento de apoyo al analisis, construido sobre datos reales de
las convocatorias 2023, 2024 y 2025.

Antes de operar, tenga presente esta norma basica: los resultados son
estimaciones estadisticas, no certezas. Trate cada cifra como una
referencia de trabajo, nunca como un dato definitivo.

===========================================================================
2. PRIMEROS PASOS
===========================================================================

2.1. Seleccion de categoria

En la parte superior del panel encontrara cuatro botones: Oficiales,
Suboficiales, Tropa y Cuerpos Comunes. Seleccione la categoria sobre la
que desea trabajar antes de introducir ningun dato. Cada categoria tiene
su propio modelo de calculo y sus propios limites de edad (suelo
biologico y techo legal).

2.2. Introduccion de meritos

Debajo del selector de categoria encontrara varios controles deslizantes,
organizados en seis bloques. Cada control representa un tipo de merito
del baremo oficial (tiempo de servicio, titulacion, cursos, idiomas,
etc.). Desplace cada control hasta el valor que corresponda a su
situacion real. La edad estimada se recalcula de forma automatica cada
vez que mueve un control.

Aviso importante: los controles de los Bloques 3 a 6 (idiomas, carnets,
deportista, funcionario, entre otros) son informativos. No suman a la
edad estimada. Esto es intencionado: dichos meritos ya estan recogidos
dentro del apartado "Meritos Generales" del Bloque 1, y evitar la doble
contabilidad es un requisito de correccion del modelo.

===========================================================================
3. EL COMPARADOR DE INSTANTANEAS (SNAPSHOTS)
===========================================================================

3.1. Que es una instantanea

Una instantanea, tambien llamada snapshot, es una fotografia congelada
de su configuracion de meritos en un momento dado. Sirve para comparar
distintos escenarios sin perder ninguno de ellos.

3.2. Como guardar una instantanea

Configure los controles deslizantes segun el escenario que desee
capturar y pulse el boton correspondiente:

- Guardar como Snapshot A
- Guardar como Snapshot B
- Ref. BOE (media 2025): esta opcion no requiere configuracion manual,
  carga automaticamente los valores medios de la convocatoria 2025.

3.3. Uso operativo recomendado

Guarde primero su situacion actual como Snapshot A. A continuacion,
modifique los controles para reflejar un escenario hipotetico (por
ejemplo, tras completar un curso o una titulacion pendiente) y guardelo
como Snapshot B. El panel le mostrara de forma automatica la diferencia
de edad, de puntos y de captacion joven entre ambos escenarios.

3.4. Boton Intercambiar A/B

Permite invertir el orden de comparacion sin tener que repetir la
configuracion. Utilicelo si desea ver el analisis desde el punto de
vista contrario.

3.5. Boton Borrar todo

Elimina las tres instantaneas guardadas. Utilicelo con precaucion, la
accion no se puede revertir.

===========================================================================
4. PANEL DE SENSIBILIDAD MARGINAL
===========================================================================

Debajo del comparador encontrara un panel que indica cuanto se moveria
la edad estimada si anadiera o quitara un punto en cada bloque, partiendo
del escenario del Snapshot B. Un valor mas alto en un bloque significa
que ese bloque tiene mayor capacidad de desplazar la edad, para bien o
para mal segun el signo del apartado real al que pertenezca.

Este panel es util para identificar en que meritos concretos merece la
pena concentrar el esfuerzo si su objetivo es modificar la edad estimada
resultante.

===========================================================================
5. PROYECCION A FUTURO (MOTOR DE SIMULACION)
===========================================================================

5.1. Concepto general

Esta seccion permite proyectar como podria evolucionar la edad media a
lo largo de los proximos anos, hasta un horizonte maximo de quince anos.
La proyeccion parte siempre de una instantanea guardada previamente. No
se puede ejecutar ninguna simulacion sin al menos un snapshot valido.

Advertencia de nomenclatura: el motor realiza una simulacion de tipo
Monte Carlo mediante paseo aleatorio con parametros configurables. No es
un remuestreo de trayectorias reales de altas, bajas y reenganches,
porque esos datos historicos todavia no estan disponibles. Trate esta
proyeccion como un ejercicio de sensibilidad, no como una prediccion
cerrada.

5.2. Seleccion de instantaneas a proyectar

Marque las casillas de Snapshot A, Snapshot B y Snapshot C (Referencia
BOE) segun desee proyectar uno, dos o los tres escenarios de forma
simultanea. Cada uno se dibujara en el grafico con un color propio:

- Snapshot A: rojo, trazo continuo
- Snapshot B: azul, trazo discontinuo
- Snapshot C: verde, trazo punteado

Si dos escenarios parten de una edad muy parecida, sus lineas pueden
casi coincidir en el grafico. Por ese motivo, ademas del color, cada uno
utiliza un patron de trazo distinto, para que siempre sean
distinguibles.

5.3. Iteraciones

Este control determina cuantas trayectorias individuales se simulan por
cada instantanea antes de calcular el resultado final. Un numero mayor
de iteraciones produce una linea media mas estable y fiable, a costa de
un tiempo de calculo ligeramente superior. Se recomienda no bajar de
2000 iteraciones para un resultado solido.

5.4. Deriva anual

Este control representa una tendencia sistematica que usted decide
imponer sobre la evolucion de la edad media, ano tras ano, mas alla del
ruido estadistico propio de la simulacion. Un valor positivo simula un
escenario en el que la edad media tiende a subir con el tiempo (por
ejemplo, si prev mas competencia entre aspirantes con mas meritos
acumulados). Un valor negativo simula lo contrario.

El rango actual de este control, entre menos cero coma cinco y mas cero
coma cinco anos por ano, es una referencia de trabajo provisional, no un
limite derivado del historico real de convocatorias. Si necesita un
rango calibrado con los datos observados entre 2023, 2024 y 2025,
solicitelo al equipo de mantenimiento del panel.

5.5. Horizonte de proyeccion

Determina cuantos anos hacia el futuro se extiende la simulacion, con un
maximo de quince anos. A mayor horizonte, mayor es tambien la
incertidumbre acumulada, tal y como refleja el ensanchamiento de la
banda de percentiles en el grafico.

5.6. Interpretacion del grafico

El grafico muestra, para cada instantanea seleccionada:

- Una nube de lineas finas de fondo: representan trayectorias
  individuales simuladas, y sirven para apreciar la dispersion real del
  resultado.
- Una banda de color mas solido (solo visible si hay una unica
  instantanea activa): representa el rango donde se concentra la mayoria
  de los resultados posibles.
- Una linea gruesa destacada: representa el promedio de todas las
  trayectorias simuladas para esa instantanea, ano por ano. Es la
  referencia principal de lectura.

5.7. Indicadores numericos (KPI)

Encima del grafico encontrara cuatro indicadores: edad mediana al final
del horizonte, banda de percentil diez a percentil noventa, porcentaje
de captacion joven (aspirantes por debajo de treinta y cinco anos), y
tiempo de computo empleado. Con varias instantaneas activas a la vez,
estos indicadores se muestran de forma resumida para cada una.

5.8. Exportacion de resultados

Al final de la seccion encontrara dos botones de exportacion:

- Exportar CSV: genera un archivo de valores separados por comas, con
  los resultados ano por ano de cada instantanea proyectada. Es el
  formato recomendado para su tratamiento en hojas de calculo o en
  software de modelado como Vensim.
- Exportar JSON: genera un archivo con la misma informacion en formato
  estructurado, recomendado para su tratamiento mediante programacion o
  integracion con otras herramientas.

===========================================================================
6. LIMITACIONES QUE DEBE CONOCER
===========================================================================

- El modelo se basa en una regresion estadistica sobre datos reales de
  tres convocatorias (2023, 2024 y 2025). No captura factores como el
  orden de peticion de plaza, la disponibilidad de vacantes en cada
  convocatoria concreta, ni la autoseleccion de candidatos.
- Los resultados son tendencias centrales estimadas, no una prediccion
  exacta para ningun aspirante individual.
- La proyeccion a futuro es un ejercicio de sensibilidad de tipo Monte
  Carlo con parametros ajustables a mano, no una prediccion basada en
  datos historicos de trayectorias reales de personal.
- Consulte siempre el aviso metodologico completo, disponible en la
  parte inferior del panel, antes de emplear estos resultados en
  cualquier informe o decision.

===========================================================================
7. GLOSARIO BREVE
===========================================================================

Baremo: conjunto de criterios oficiales por los que se puntuan los
meritos de un aspirante.

Snapshot o instantanea: fotografia congelada de una configuracion de
meritos, guardada para su comparacion posterior.

Deriva: tendencia sistematica de cambio anual que se impone de forma
manual a una simulacion.

Percentil: valor por debajo del cual se situa un porcentaje determinado
de los resultados simulados. Por ejemplo, el percentil diez es el valor
que solo supera al diez por ciento de los casos mas bajos.

Captacion joven: porcentaje estimado de aspirantes con edad inferior a
treinta y cinco anos, empleado como indicador de renovacion
generacional.

===========================================================================
FIN DEL MANUAL
===========================================================================
