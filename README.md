# Lab 4 Robotica
## Integrantes:

Fabian Steven Galindo Peña  
Brian Alejandro Vásquez González  
William Arturo Sierra Díaz  

## Diseño de la herramienta.
Para el diseño de la herramienta se tuvieron en cuenta varios aspectos como
+ Dimesiones del marcador
+ Angulo de orientacion de la herramienta
+ Maxima compresión del porta herramientas

Con esto claro se llego a un diseño que contempla el uso de un resorte y se compone de dos partes hechas en PLA por medio de impresion 3D, el diseño detallado de estas piezas se puede ver en los [planos adjuntos al repo](Planos/)

A continuacion se detallan los planos finales del portaherramientas elaborado:  
Primero podemos observar la base que tiene un radio interno de 9.5mm para el acomple del marcador, ademas se realizo una guia de 7x3mm cuadrada con la cual se buscaba que el marcador se deslizara unicamente en el eje z asi al marcador se realizo una modificacion añadiendo las guias para el desplazamiento de este, adicionalmente vemos en la parte inferior el diseño para el acople con el portaherramientas del Robot que usa 4 tornillos M6 segun se describe en la hoja de especificaiones tecnicas de este.  
![image](https://user-images.githubusercontent.com/36159469/176569190-1656b5df-f36b-46f4-9bbc-88ab7dbace7e.png)  
A continuacion se muestra el diseño de la tapa que contempla una seccion interna de 19mm de diametro para que la punta del marcador pueda pasar y asi realizar la escritura sobre la superficie deseada.  
![image](https://user-images.githubusercontent.com/36159469/176569223-d9d7589e-c9c7-471c-a6b3-3f8279678861.png)  
Se eligio impresion 3D para un prototipado y rapida fabricacion, ademas de que por la aplicacion no habian grandes fuerzas que contemplar en la herramienta.   
De esta forma se logro fabricar tal como se ven en las siguiente imágenes  
![image](https://user-images.githubusercontent.com/36159469/176569928-822a9bc9-c05e-4f21-9f61-8c6d1521cc8b.png)   
Posteriormente se procedio a ensamblar como se detalla en la siguiente imágen  
![image](https://user-images.githubusercontent.com/36159469/176572696-23e8e904-629f-494d-bf4e-6083280b0929.png)


## Código en RAPID del módulo utilizado para el desarrollo de la práctica.
Para comenzar con el desarrollo del lab se planteo usar workobject desde el principio de esta forma se facilitaba el cambio este un plano a un plano inclinado a 30°  
Asi lo primero que tenemos en nuestra programacion es la defincion de la posicion y orientacion de cada WorkObject que se guardan en las variables tipo wobjdata con nombres Cuadrante1 y Cuadrante2. 
Luego en la tercera linea del modulo se encuentra la posicion y orientacion de nuestra herramienta, definida por la calibracion como se explica en la siguiente seccion donde se podra ver el procedimiento realizado mas a fondo.
```
MODULE ModuleFab
    PERS wobjdata Cuadrante1:=[FALSE,TRUE,"",[[200,600,10],[1,0,0,0]],[[0,0,0],[1,0,0,0]]];
    PERS wobjdata Cuadrante2:=[FALSE,TRUE,"",[[450,-400,110],[0.683012702,-0.183012702,-0.183012702,-0.683012702]],[[0,0,0],[1,0,0,0]]];
    PERS tooldata Marker:=[TRUE,[[-0.917808,-1.75486,202.667],[0.812949,-0.58005,0.0419499,-0.0299318]],[0.2,[0,0,1],[1,0,0,0],0,0,0]];
```
Posterior a esto vemos las definicion de constantes, estos van a ser los puntos guia para nuestra trayectoria, para ver todo el codigo completo puede consultar el programa [adjunto al repo] (ModuleFab.mod)
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

## Calibracion de la herramienta

![photo_2022-06-24_17-26-40](https://user-images.githubusercontent.com/36159469/176571642-f9e8e15c-c4b8-4055-9782-ac1b769c593b.jpg)  
![photo_2022-06-24_17-26-39](https://user-images.githubusercontent.com/36159469/176571647-72999a6d-08e2-4c95-a0a6-ef888fffd367.jpg)  


## Simulación en RobotStudio
## Implementación de la práctica con los robots reales.

[Practica 4: Implementación Real](https://youtu.be/nFzFDuL_ORQ)
## Descripción de la solución planteada
