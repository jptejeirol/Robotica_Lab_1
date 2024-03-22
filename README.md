# Robotica_Lab_1
 Robótica Industrial - Trayectorias, Entradas y Salidas Digitales

### Integrantes:  
Juan Pablo Tejeiro Londoño  

## Descripción de la solución planteada  

El objetivo de la práctica es, en principio, lograr que uno de los robot ABB IRB 140 del LabSir ejecute con exito, presición y seguridad una trayectoria capaz de dibujar el logo de una empresa reconocida, su nombre y las inciiales del integrante del equipo. Para obtener este resultado fué necesario utilizar una serie de herramientas tecnológicas requeridas por los lineamientos de la guía de laboratorio.  

En primer lugar se inicia la curva de aprendizaje ejecutando las guías prácticas proveidas por el curso para una correcta familiarización con el entorno y sus diversas aplicaciones. Seguido a esto se define la configuración a utilizar para la simulación de la práctica, para esto se selecciona el archivo de tipo estación y se selecciona el robot IRB140 de la biblioteca de RobotStudio, el siguiente paso fué generar el controlador del robot mediante la herramienta de diseño de controladores virtuales y el software de controladores RobotWare de ABB con su versión respectiva.

Para ejecutar la rutina propuesta por la guía se exige el diseño de una herramienta  desde cero que pemita crear el dibujo, este proceso se va a describir a detalle en la sección "Diseño de la herramienta" mas adelante. Ya teniendo la herramienta diseñada se procede a incorporarla en la estación de trabajo de RobotStudio y se crea el correspondiente dato de tipo ToolData que busca codificar los datos asociados a la herramienta para ser procesados por el controlador del robot y dar a entender al robot que cualquier rutina ejecutada deberá tener en cuenta a la herramienta instalada en el flanche. En un principio se seleccionó la masa de la herramienta para el ToolData como 120 gramos que era un cálculo aproximado real basado en las propiedades arrojadas pir el software CAD de diseño utilizado para la herramienta, pero tras varias pruebas se determinó que este valor es demasiado bajo y causa un error de "Carga dinámica excesiva" durante la simulación, para solucionar esto se guarda la masa con un valor de 300 gramos, de igual manera por convenciencia se dejan los valores por defecto de momentos de inercia y centro de masa. Para ubicar el TCP se ubica el origen de la herramienta de manera que este sea el mismo punto de coincidencia del flanche del robot, en el centro de la base circular de la herramienta, seguido a esto se orienta la herramienta en la dirección que se busca con respecto al HOME del robot para facilitar la orientación del TCP, teniendo en cuenta que se diseñó la herramienta con un ángulo de inclinación de 30° que permita que se posicione de manera totalmente vertical en el HOME del robot, de esta manera bastó con seleccionar el punto de la punta de la herramienta y reorientar el sistema de coordenadas con un ángulo de 60° (90° - 30° de la herramienta) para conseguir el resultado esperado.  

![image](https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/887e05b1-2b3e-4b1b-951c-4d57bddd1350)  

Teniendo ya la estación, el robot, el controlador y el ToolData, queda definir el WorkObject para conseguir todos los elementos necesarios para estructurar una rutina de trabajo para el robot. Basado en los planos inclinados presentes en el laboratorio y las opciones de la guía de laboratorio, se opta por diseñar la rutina de modo que la trayectoria se ejecute en un plano inclinado. Para esto, por facilidad, se crea un modelado con el logo, la amarca y las inciales, enmarcadas en un plano inclinado de 45° y se importa a la estación. También, por la distribución de los elementos a dibujar y el objetivo de acomodar todo en una hoja fomato A4, se orienta horizontalmente la trayectoria como se observa en la figura. Para definir el WorkObject simplemente se define los 3 puntos del sistema de coordenadas de usuario del dato tipo WorkObject y se seleeciona 2 puntos de la linea superior del plano como eje X y 1 punto de la linea lateral del plano como eje Y, de esta manera el eje Z queda orientado en la dirección ortogonal al plano inclinado.  

Como parte del proceso iterativo que se llevó a cabo para dar con una trayectoria exitosa, se toma la desición de utilizar instrucciones simples de baja velocidad y poca variabilidad en la aceleración del robot. Teniendo en cuenta esto último la velocidad de todas las instrucciones fue definida en v100 (basado en el límite de velcoidad mínimo requerido por la práctica), por la misma línea todas las instrucciones se definen con una zona de tolerancia fina para minimizar cualquier deformación en el dibujo final. En la definición todas las isntrucciones fueron puestas como movimiento lineal y como post proceso se redefinen las instrucciones de traslación de la herramienta, como saltos entre letras y retorno a HOME a instrucciones de movimiento Jump y así no restringir el movmiento del robot en momentos innecesarios de la trayectoria y previniendo errores de singularidades y alcance del robot observados en simulaciones previas. Finalmente se redefinen las trayectorias rectas entre puntos de la trayectoria que deben ser curvas, simplemente convirtiendo 2 trayectorias contiguas en instrucciones de movimiento circular, cuidando que los puntos no se encuentren demasiado cerca o lejos entre si. Para las transciiones entre letras se definió un desfase en Z de -20 milímetros.  

## Diagrama de flujo de las acciones del robot  

## Plano de planta  

## Descripción de las funciones utilizadas  

## Diseño de la herramienta  

## Simulación en RobotStudio  

https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/4c1a2998-2781-41d5-81e9-d786c137cee9

<p align="center">
  <img width="460" height="300" src="[https://www.pexels.com/photo/photo-of-grey-tabby-kitten-lying-down-2558605/](https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/4c1a2998-2781-41d5-81e9-d786c137cee9)">
</p>

## Implementación práctica en el LabSir

https://github.com/jptejeirol/Robotica_Lab_1/assets/164267794/897dcec8-3eaa-4413-9d8c-0e81b65b51f6







