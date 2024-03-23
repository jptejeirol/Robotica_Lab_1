# Robotica_Lab_1
 Robótica Industrial - Trayectorias, Entradas y Salidas Digitales

### Integrantes:  
Juan Pablo Tejeiro Londoño  

## Descripción de la solución planteada  

El objetivo de la práctica es, en principio, lograr que uno de los robot ABB IRB 140 del LabSIR ejecute con exito, presición y seguridad una trayectoria capaz de dibujar el logo de una empresa reconocida, su nombre y las inciiales del integrante del equipo. Para obtener este resultado fué necesario utilizar una serie de herramientas tecnológicas requeridas por los lineamientos de la guía de laboratorio.  

En primer lugar se inicia la curva de aprendizaje ejecutando las guías prácticas proveidas por el curso para una correcta familiarización con el entorno y sus diversas aplicaciones. Seguido a esto se define la configuración a utilizar para la simulación de la práctica, para esto se selecciona el archivo de tipo estación y se selecciona el robot IRB140 de la biblioteca de RobotStudio, el siguiente paso fué generar el controlador del robot mediante la herramienta de diseño de controladores virtuales y el software de controladores RobotWare de ABB con su versión respectiva.

Para ejecutar la rutina propuesta por la guía se exige el diseño de una herramienta  desde cero que pemita crear el dibujo, este proceso se va a describir a detalle en la sección "Diseño de la herramienta" mas adelante. Ya teniendo la herramienta diseñada se procede a incorporarla en la estación de trabajo de RobotStudio y se crea el correspondiente dato de tipo ToolData que busca codificar los datos asociados a la herramienta para ser procesados por el controlador del robot y dar a entender al robot que cualquier rutina ejecutada deberá tener en cuenta a la herramienta instalada en el flanche. En un principio se seleccionó la masa de la herramienta para el ToolData como 120 gramos que era un cálculo aproximado real basado en las propiedades arrojadas pir el software CAD de diseño utilizado para la herramienta, pero tras varias pruebas se determinó que este valor es demasiado bajo y causa un error de "Carga dinámica excesiva" durante la simulación, para solucionar esto se guarda la masa con un valor de 300 gramos, de igual manera por convenciencia se dejan los valores por defecto de momentos de inercia y centro de masa. Para ubicar el TCP se ubica el origen de la herramienta de manera que este sea el mismo punto de coincidencia del flanche del robot, en el centro de la base circular de la herramienta, seguido a esto se orienta la herramienta en la dirección que se busca con respecto al HOME del robot para facilitar la orientación del TCP, teniendo en cuenta que se diseñó la herramienta con un ángulo de inclinación de 30° que permita que se posicione de manera totalmente vertical en el HOME del robot, de esta manera bastó con seleccionar el punto de la punta de la herramienta y reorientar el sistema de coordenadas con un ángulo de 60° (90° - 30° de la herramienta) para conseguir el resultado esperado.  

<p align="center">
  <img width="500" height="400" src="https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/887e05b1-2b3e-4b1b-951c-4d57bddd1350">
</p>

Teniendo ya la estación, el robot, el controlador y el ToolData, queda definir el WorkObject para conseguir todos los elementos necesarios para estructurar una rutina de trabajo para el robot. Basado en los planos inclinados presentes en el laboratorio y las opciones de la guía de laboratorio, se opta por diseñar la rutina de modo que la trayectoria se ejecute en un plano inclinado. Para esto, por facilidad, se crea un modelado con el logo, la amarca y las inciales, enmarcadas en un plano inclinado de 45° y se importa a la estación. También, por la distribución de los elementos a dibujar y el objetivo de acomodar todo en una hoja fomato A4, se orienta horizontalmente la trayectoria como se observa en la figura. Para definir el WorkObject simplemente se define los 3 puntos del sistema de coordenadas de usuario del dato tipo WorkObject y se seleeciona 2 puntos de la linea superior del plano como eje X y 1 punto de la linea lateral del plano como eje Y, de esta manera el eje Z queda orientado en la dirección ortogonal al plano inclinado.  

Como parte del proceso iterativo que se llevó a cabo para dar con una trayectoria exitosa, se toma la desición de utilizar instrucciones simples de baja velocidad y poca variabilidad en la aceleración del robot. Teniendo en cuenta esto último la velocidad de todas las instrucciones fue definida en v100 (basado en el límite de velcoidad mínimo requerido por la práctica), por la misma línea todas las instrucciones se definen con una zona de tolerancia fina para minimizar cualquier deformación en el dibujo final. En la definición todas las isntrucciones fueron puestas como movimiento lineal y como post proceso se redefinen las instrucciones de traslación de la herramienta, como saltos entre letras y retorno a HOME a instrucciones de movimiento Jump y así no restringir el movmiento del robot en momentos innecesarios de la trayectoria y previniendo errores de singularidades y alcance del robot observados en simulaciones previas. Finalmente se redefinen las trayectorias rectas entre puntos de la trayectoria que deben ser curvas, simplemente convirtiendo 2 trayectorias contiguas en instrucciones de movimiento circular, cuidando que los puntos no se encuentren demasiado cerca o lejos entre si. Para las transciiones entre letras se definió un desfase en Z de -20 milímetros.  

## Diagrama de flujo de las acciones del robot  

<p align="center">
  <img width="371" height="1924" src="https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/3ff6a48e-6d6f-4e67-83ac-69dc099e3857">
</p>

## Plano de planta  

En esta sección se presenta una vista de planta del laboratorio, específicamente la estación de trabajo, se planteó a escala y de amnera aproximada la distribución principal de la estación utilizada del laboratorio de robótica LabSIR. Se presenta el robot como elemento principal, sobre su respectiba base de trabajo. En la esquina inferior derecha se encuentra el controlador, donde también se encuentra ubicado el FlewPendant, en la parte inferior esta la mesa de trabajo del operador, desde donde se minitoreo y controló el robot para la ejecución de las rutinas. Finalmente se muestra la posición de la herramienta diseñada instalada en la brida del robot y el workobject con el logo e iniciales definidas para la trayectoria.  

<p align="center">
  <img width="1000" height="800" src="https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/0149ec80-2925-4d13-97bd-084d9b22a255">  
</p>


## Descripción de las funciones utilizadas  

## Diseño de la herramienta  

Para poder diseñar la herramienta, se hizo un modelado de ingeniería basado completamente en el marcador a utilizar. Con ayuda de un pie de rey se tomó las dimensiones del marcador y de estas se desarrollo el porta marcador de la herramienta. Para el diseño de la pieza de fijación en la brida del robot se utilizó el plano de esta pieza presente en el manual del robot como se puede observar en la siguiente figura.

<p align="center">
  <img width="600" height="300" src="https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/3cd43ddb-e65d-4677-86e6-c26ef3b47a6d">  
</p>  

La herramienta se modeló en 2 piezas utilizando el software CAD Inventor de Autodesk, luego de esto los dos archivos se exportaron en formato STEP y fueron impresos en la impresora 3D presente en la oficina de la empresa para la cuál se trabaja actualemente, lo cuál facilitó el acceso pero también generó ciertos problemas pues la máquina no estaba bien calibrada y fue necesario un proceso de precalibrado para obtener los resultados deseados. La pieza superior del porta marcador se diseño con un ajuste estrecho para que el marcador no requiriera de ningún otro tipo de elemento de agarre sino que pudiera dejarse fijo con algo de presión. La unión entre las dos piezas se realizó mediante un tornillo y tuerca simples, la impresión por facilidad y presición se ejecutó sin estos agujeros, por lo cuál los agujeros fueron realizados de manera rápida con ayuda de un taladro.

<p align="center">
  <img width="1277" height="781" src="https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/6c10946c-ac81-4955-aa9e-8830bcfeca03">  
</p>  

En la siguiente figura se muestra la herramienta al ser importada al entorno de RobotStudio y siendo generado el ToolData Correspondiente. Como se puede observar el sistema coordenado del TCP esta orientado de manera tal que el eje Z positivo se encuentre con el eje del marcador y el eje X se encuentre orientado en sentido diametral inferior del marcador de manera que coincida con el sistema coordenado de la brida del robot.

<p align="center">
  <img width="331" height="429" src="https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/e0cf8da1-ccfe-4c77-9b7b-e72fe46500cf">  
</p>  

El resultado final puede observarse en la siguiente figura, el ángulo de ensamble entre las dos piezas fue medido con transportador y se comparó con la vista lateral del modelado en Inventor y directamente con el robot, pues el ángulo correcto se observaba si el marcador se encontraba perfectamente vertical en posición de HOME. La pieza se imprimió en PLA en una impresora CREALITY Ender 3.

<p align="center">
  <img width="600" height="1000" src="https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/0061de48-3364-489f-adc3-05b73b57fdfe">  
</p>

En la siguiente imágen se puede ver con claridad la posición buscada de la herramienta diseñada en el robot.

<p align="center">
  <img width="1000" height="1000" src="https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/a2998b89-ef5b-4143-b568-5bd734ee4ffb">  
</p>

## Simulación en RobotStudio  

En el siguiente video se hace la presentación de la simulación desarrollada en RobotStudio, de la cuál se obteniene el programa que fué cargado en el controlador del robot en el LabSIR para la realización de la práctica real. En primer lugar se creao la simulación sin tener en cuenta entradas y sálidas, y cuando ya se verificó que la trasyectorianera adecuada y no presentaba errores en ejecución, se procede a incluir y programar las entradas y salidas digitales. En el video se observa a la derecha el panel de entradas y salidas y como al manipular las entradas durante la simulación se obtnien la ejecución de cada rutina y la salida corresponsiente.

https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/4c1a2998-2781-41d5-81e9-d786c137cee9

## Implementación práctica en el LabSir  

Finalmente en este video se muestra la grabación en tiempo real de las rutinas ejecutadas con el robot en el laboratorio. Primero se toma el FlexPendant y se inserta la USB para cargar el programa. Luego de cargado el programa se ejecuta la rutina de mantenimiento para instalar la herramienta en el robot utilizando la herramienta disponible en el laboratorio. Luego se lleva el robot a posición de HOME y se procede a poner el workobject en la zona de trabajo. Ene ste caso se utilizó uno de los planos inclinados, por lo que con cinta se pega una hoja desde la esquina superior del plano inclinado. El paso siguiente fué definir el workobject, para esto se crea un nuevo dato tipo workobject en la pestaña de program data y se define los 3 puntos del sistema de coordenadas de usuario, 2 sobre el vértice superior del plano como eje X y uno sobre el vértice lateral izquierdo como eje Y, utilizando la función de Jogging del robot. Ya con el workobject definido se verifica la que el dato de herramienta tipo ToolData importado de la simulación funcione correctamente ejecutando paso a paso la rutina de dibujo desde el FlexPendant. Ya verificado el TCP de la herramienta se hizo una serie de pruebas para calibrar manualmente el plano inclinado teniendo en cuenta la presencia de diversas descontinuidades en el mismo y la estructura que lo sostiene. Luego de un par de pruebas se cambia la hoja por una hoja limpia y se ejecuta la rutina de dibujo final para ser grabada, que es la que se evidencia en el video.

https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/897dcec8-3eaa-4413-9d8c-0e81b65b51f6  

En la siguiente imágen se exponen las distintas pruebas realizadas de la rutina de dibujo y a la derecha se presenta la prueba final, a pesar de que no existe un espacio muy evidente entre las letras, se evidencia que esta ahí presente y que los trazos realizados son lo suficientemente precisos para que se vea claramente tanto el logo como las letras.

![WhatsApp Image 2024-03-22 at 7 03 49 PM](https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/afe27b7c-0eb7-4caa-ad0b-46dd76b35983)





