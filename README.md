# Lab 4 Robotica
## Integrantes:

Fabian Steven Galindo Peña  
Brian Alejandro Vásquez González  
William Arturo Sierra Díaz  

## Diseño de la herramienta.
Para el diseño de la herramienta se tuvieron en cuenta varios aspectos como
+ Dimensiones del marcador
+ Angulo de orientación de la herramienta
+ Maxima compresión del porta herramientas

Con esto claro se llegó a un diseño que contempla el uso de un resorte y se compone de dos partes hechas en PLA por medio de impresión 3D, el diseño detallado de estas piezas se puede ver en los [planos adjuntos al repo](Planos/)

A continuacion se detallan los planos finales del portaherramientas elaborado:  
Primero podemos observar la base que tiene un radio interno de 9.5mm para el acople del marcador, ademas se realizo una guia de 7x3mm cuadrada con la cual se buscaba que el marcador se deslizara unicamente en el eje z asi al marcador se le realizo una modificacion añadiendo las guías para el desplazamiento de este, adicionalmente vemos en la parte inferior el diseño para el acople con el portaherramientas del Robot que usa 4 tornillos M6 segun se describe en la hoja de especificaiones tecnicas de este.  
![image](https://user-images.githubusercontent.com/36159469/176569190-1656b5df-f36b-46f4-9bbc-88ab7dbace7e.png)  
A continuacion se muestra el diseño de la tapa que contempla una seccion interna de 19mm de diametro para que la punta del marcador pueda pasar y asi realizar la escritura sobre la superficie deseada.  
![image](https://user-images.githubusercontent.com/36159469/176569223-d9d7589e-c9c7-471c-a6b3-3f8279678861.png)  
Se eligio impresión 3D para un prototipado y rápida fabricacion, además de que por la aplicación no habáan grandes fuerzas que contemplar en la herramienta.   
De esta forma se logro fabricar tal como se ven en las siguiente imágenes  
![image](https://user-images.githubusercontent.com/36159469/176569928-822a9bc9-c05e-4f21-9f61-8c6d1521cc8b.png)   
Posteriormente se procedio a ensamblar como se detalla en la siguiente imágen  
![image](https://user-images.githubusercontent.com/36159469/176572696-23e8e904-629f-494d-bf4e-6083280b0929.png)


## Código en RAPID del módulo utilizado para el desarrollo de la práctica.
Para comenzar con el desarrollo del lab se planteo usar workobjects desde el principio de esta forma se facilitaba el cambio este un plano a un plano inclinado a 30°  
Asi lo primero que tenemos en nuestra programación es la definición de la posición y orientación de cada WorkObject que se guardan en las variables tipo wobjdata con nombres Cuadrante1 y Cuadrante2. 
Luego en la tercera línea del módulo se encuentra la posición y orientación de nuestra herramienta, definida por la calibración como se explica en la siguiente sección donde se podra ver el procedimiento realizado más a fondo.
```
MODULE ModuleFab
    PERS wobjdata Cuadrante1:=[FALSE,TRUE,"",[[200,600,10],[1,0,0,0]],[[0,0,0],[1,0,0,0]]];
    PERS wobjdata Cuadrante2:=[FALSE,TRUE,"",[[450,-400,110],[0.683012702,-0.183012702,-0.183012702,-0.683012702]],[[0,0,0],[1,0,0,0]]];
    PERS tooldata Marker:=[TRUE,[[-0.917808,-1.75486,202.667],[0.812949,-0.58005,0.0419499,-0.0299318]],[0.2,[0,0,1],[1,0,0,0],0,0,0]];
```
Posterior a esto vemos la definición de constantes, estos van a ser los puntos guía para nuestra trayectoria, para ver todo el código completo puede consultar el programa [adjunto al repo](ModuleFab.mod)
```
    CONST robtarget Target_5:=[[-54.660161362,322.568528773,705.32582312],[0.5,-0.683012702,-0.5,-0.183012702],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_10:=[[0.000029436,0.000002786,-0.00001954],[0,1,0,0],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_20:=[[85.00007128,0.000056558,-0.000067404],[0.000000052,1,0.00000008,0.000000048],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_30:=[[100,-40,0],[0,1,0,0],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    .
    .
    .
    CONST robtarget Target_230:=[[100.00010508,-280.000701552,-0.002614694],[-0.000000019,1,-0.000000044,0.000000209],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_240:=[[100.000114302,-280.00069038,19.997361362],[-0.000000031,1,-0.000000041,0.000000218],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_250:=[[0.000008509,0.000003577,-0.00001118],[0.000000006,1,0,-0.000000002],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_260:=[[-54.660161362,322.568528773,705.32582312],[0.5,-0.683012702,-0.5,-0.183012702],[0,0,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
```
Posterior a esto se definio el main el cual ejecutara dos procedimientos ya declarados que contendrán las isntrucciones para moverse en cada workObject
```
    PROC main10()
        Path_10;
        Path_20;
    ENDPROC
```
Por ultimo en cada procedimiento se declararon las instrucciones MOVL necesarias para realizar la trayectoria deseada siguiendo todos los puntos previamente definidos, en cada instrucción se definía la velocidad la cual es estandar para todos los movimiento y se estableció en 200 mm/s y el valor de Z en 10 para lograr realizar los trazos  
Para este procedimiento podemos ver que tenemos dos puntos que hacen referencia al Cuadrante2 esto es porque se seleccionó como Home el Home del Robot y se estableció este Home respecto a el segundo workObject.  
```
PROC Path_10()
        MoveL Target_5,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_10,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_20,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_30,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_40,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_50,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_60,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_70,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_75,v200,z10,Marker\WObj:=Cuadrante1;
        .
        .
        .
        MoveL Target_190,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_200,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_210,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_220,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_230,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_240,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_250,v200,z10,Marker\WObj:=Cuadrante1;
        MoveL Target_260,v200,z10,Marker\WObj:=Cuadrante2;
    ENDPROC
```
Y asi se definió tambien para el Path_20 que contiene las instrucciones para el movimiento en el plano inclinado
```
    PROC Path_20()
        MoveL Target_5,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_10,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_20,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_30,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_40,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_50,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_60,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_70,v200,z10,Marker\WObj:=Cuadrante2;
        .
        .
        .
        MoveL Target_180,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_190,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_200,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_210,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_220,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_230,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_240,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_250,v200,z10,Marker\WObj:=Cuadrante2;
        MoveL Target_260,v200,z10,Marker\WObj:=Cuadrante2;
    ENDPROC
```
De esta forma se completó el desarrollo del código y se procedió a realizar las pruebas en el laboratorio despues de haber realizado la simulación en RobotStudio
## Calibración de la herramienta

Ya construida la herramienta se procede a ensamblar el portaherramienta, y se procede a realizar la calibración. A partir del ensamble en el espacio de trabajo de un punto fijo para tener un punto de referencia y utilizar el método de acercamiento en el eje z y acercamiento de la herramienta por 3 diferentes configuraciones del robot. Se realizo una primera calibración como se ve en la imagen de la definición de la herramienta, aquí se observa que el error máximo es de 2,4 mm, error mínimo de 2,2 mm y un error promedio de 2,3 mm. Estos errores se podrían compensar con la distancia de retracción que aporta el resorte de la herramienta sin embargo se toma la decisión de repetir la calibración. De la cual el grupo de trabajo olvido tomar foto, pero su error promedio era alrededor de 0,5 mm. 

![photo_2022-06-24_17-26-40](https://user-images.githubusercontent.com/36159469/176571642-f9e8e15c-c4b8-4055-9782-ac1b769c593b.jpg)  
![photo_2022-06-24_17-26-39](https://user-images.githubusercontent.com/36159469/176571647-72999a6d-08e2-4c95-a0a6-ef888fffd367.jpg)  


## Simulación en RobotStudio
A continuacion se puede ver la simulación final en RobotStudio en la cual el Robot realiza las dos trayectorias cumpliendo los requerimientos de la práctica  
[![Practica 4: Simulación](https://img.youtube.com/vi/zRVmG34LNds/0.jpg)](https://youtu.be/zRVmG34LNds)
## Implementación de la práctica con los robots reales.
A continuacion se puede ver la implementacion con el robot en el laboratorio LabSir en la cual el Robot realiza las dos trayectorias cumpliendo los requerimientos de la practica.  
[![Practica 4: Implementación Real](https://img.youtube.com/vi/nFzFDuL_ORQ/0.jpg)](https://youtu.be/nFzFDuL_ORQ)

## Descripción de la solución planteada
Se describe la solución planteada:
-	En primer lugar, se diseña la herramienta, se desea usar una herramienta con resorte para darle algo de tolerancia al marcador. Se genera su diseño CAD, y se imprime en 3D, para luego ser ensamblada en el robot.
-	Esta herramienta en modelo CAD, se importa a RobotStudio, para realizar su calibración en el software. 
-	Se realiza la calibración de la herramienta, y este resultado se compara con el de RobotStudio, para verificar diferencias en distancias entre la simulación y el mundo real, tras la pieza ser implementada.
-	En el software, se crean los dos workobjects, para cada una de las escrituras, y así facilitar su desplazamiento de ser necesario.
-	Se diseña la trayectoria que debe realizar el robot, para trazar las iniciales de los tres integrantes Brian (B), Fabian (F) y William (W). Se programaron todos los robtargets, que posteriormente generan los paths. Estos paths se sincronizan con RAPID para generar el módulo, que será importado posteriormente al robot. Se tuvieron en cuenta todas las indicaciones del laboratorio, a la hora de crear estas trayectorias.
-	Se importa el módulo al controlador, y se realizan los trazos. Se verifican las distancias, y se cuadran las posiciones de los workobjects, para que todo este correcto, y así lograr las tareas deseadas.

