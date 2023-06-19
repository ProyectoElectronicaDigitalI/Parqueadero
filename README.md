# Parqueadero 

# Contexto 
En Colombia existe una falta de cultura ciudadana en cuanto al respeto de aquellos lugares que se designaron para el estacionamiento exclusivo de personas en condición de discapacidad, ya que, a pesar de que estos lugares están señalizados, existen muchas personas que evaden esta normativa y estacionan sus vehículos en dichos lugares evitando una buena convivencia ciudadana. Además, las luces aun cuando no hay objetos en movimiento estan encendidas generando un desperdicio de energía


# Objetivos 

El objetivo principal del proyecto es crear un sistema que de acuerdo al tipo de población permita estacionarse o no en un espacio para personas con movilidad reducida o discapacidad.

Los objetivos secundarios son: 

1. Facilitar el encuentro de un estacionamiento tanto para personas con condición de discapacidad o movilidad reducida y para personas sin condición de discapacidad o sin movilidad reducida por medio de un sistema de conteo de estacionamientos vacíos de acuerdo a el tipo de población al que se pertenece.

2. Implementar un sistema que encienda las luces cuando detecte la presencia de un automóvil.    


 # Cómo se realizó el proyecto 
 
 Para el proyecto se realizó una maquina de estados algoritmica como se observa en la siguiente imagen 
 
 
 
 
 
 
 
 
 
De manera que, para realizar la implementación de cada periferico se hizo lo siguiente:

A. Sensor de ultrasonido

Para el sensor de ultra sonido, lo primero que se hace es crear un divisor de frecuencia, esto con el fin de tener una frecuencia de     microsegundos, luego de esto, se mira que cada vez que halla un franco de subida de la señal de microsegundos, se va a revisar una       serie de condiciones que son las siguientes:

1. Existe un contador auxiliar, mientras este contador esté entre 0 y 10 se manda un tren de pulsos desde el trigger, para enviar          este tren de pulsos el trigger se pone en 1. 

2. Mientras el contador inicial está en 0 el contador principal se inicializa en 0. 

3. Si el Echo recibe un 1 que significa que captó la señal del trigger reflejada, se aumenta el contador principal. 

4. Si el contador auxiliar llega a 249999 se debe reiniciar el contador auxiliar para enviar de nuevo un tren de pulsos, es decir,        si han pasado 0.249 segundos, en los que no se cumplen las condiciones anteriores (0.247 segundos en que no se recibe la señal           del trigger por el echo se vuelve a mandar la señal del trigger).

5. El contador auxiliar debe ir aumentando mientras no cumpla las anteriores condiciones.

De acuerdo a esas condiciones, luego nos interesa mirar según la señal del Echo como está el contador principal, entre menor sea el número de estee, más alejado está el objeto, de manera análoga, entre más cerca este el objeto mayor será el contador. Y conforme a eso se toma una señal de salida. 

B. Pantalla LCD 

Para la pantalla LCD se descargó la librería para vhdl de 8 bits. Luego de ello, se definieron el número de instrucciones a utilizar, siendo las dos primeras la inicialización de la pantalla, luego los caracteres que se querían utilizar y por último se indica donde acaban las instrucciones. 

C. ServoMotor

El servomotor funciona en base a una señal que depende del ciclo util, este ciclo util es el que varia la posición del Servo Motor. Luego, en el codigo se relizó un pwm deacuerdo a una señal que se ingresa (selector) con el cual se cambia el ciclo util de una señal, que será la salida (PWM), y deacuerdo a esta salida se generará el movimiento del servo motor. 

D. Lector RFID

E. Luces Led

Deacuerdo a los sensores de ultrasonido, se realiza una pequeña cadena de if, en la cuál dependiendo de si se detecta un objeto o no en el sensor del ultradonido se prenden ciertos leds. 

F. Siete segmentos

Para los siete segmentos, fue necesario crear dos tierras, para lo cual se utilizó un multiplexor, cuya señal de control es la tierra y deacuerdo a ello se enciende uno u otro siete segmentos. Deacuerdo con esto ya se hizó la parte de una cadena de if para verificar cuales de los sensores de ultrasonido detectan un objeto, con esto se le da un valor en codigo BCD y luego se realiza la decodificación para llevarlo a los siete segmentos. 



 
 # Avances del proyecto

Un avance del proyecto es el funcionamiento de los sensores de ultrasonido. En este caso se estan probando dos de los sensores. 
Esto se puede observar en el video llamado "Avances de los sensores de ultrasonido.mp4". Como en el video se explica lo que se hace es que el sensor verifica si hay un objeto y deacuerdo a ello cambia el contador, un contador para estacionamientos vacios para personas con discapacidad y otro contador para estacionamientos vacios para personas sin condición de discapacidad. 


https://github.com/ProyectoElectronicaDigitalI/Parqueadero/assets/136981880/e90f4291-cd2a-4f11-b272-2a990b862f99



Otro avance se observa en el video "Avance del servoMotor.mp4" en el cual, dependiendo de una configuración de pines en la fpga se mueve el servomotor, este servomotor se usará para permitir o no el paso o ingreso al parqueadero. 




https://github.com/ProyectoElectronicaDigitalI/Parqueadero/assets/136981880/0990db3b-26b5-465f-880e-21ee5bcb1639



Otro avance es el video "Avance de la pantalla LCD.mp4" en este video se ve como se escribe good en la pantalla, donde dependiendo del capacitor se varía el brillo de esta y se permite ver mejor o no la letra que está escrita. 



https://github.com/ProyectoElectronicaDigitalI/Parqueadero/assets/136981880/143900f9-2931-4657-9060-2d843d8b933f


# Implementación 

Para la solución de esta problematica, se realizó un codigo en vhdl por medio del software QUARTUS. Para la ejecución de este código se utilizó una una fpga altera cyclon 4, con la siguiente disposición de pines:

-- el clock del programa será el de la fpga que es de 50MHz
    
    CLK => PIN_23
-- Pines para la pantalla LCD (Para esta conexión se recomienda revisar el esquematico del circuito de la FPGA)

    DATA_LCD[7] => PIN_112
    DATA_LCD[6] => PIN_111
    DATA_LCD[5] => PIN_110
    DATA_LCD[4] => PIN_106
    DATA_LCD[3] => PIN_105
    DATA_LCD[2] => PIN_104
    DATA_LCD[1] => PIN_103
    DATA_LCD[0] => PIN_101
    
    ENA => PIN_100
    RS => PIN_85
    RW => PIN_99

-- Pines para los sensores de ultrasonido 

    EchoSenUlt[3] => PIN_34
    EchoSenUlt[2] => PIN_28
    EchoSenUlt[1] => PIN_33
    EchoSenUlt[0] => PIN_30
            
    TrigSenUlt[3] => PIN_39
    TrigSenUlt[3] => PIN_31
    TrigSenUlt[3] => PIN_38
    TrigSenUlt[3] => PIN_32
    
-- Pines para el servo Motor 

    servoMotor => PIN_52
    
 -- Pines para los siete segmentos siendo ti1 y ti2 las tierras de los siete segmantos
    
    S0 => PIN_119
    S1 => PIN_120
    S2 => PIN_121
    S3 => PIN_124
    S4 => PIN_125
    S5 => PIN_126
    S6 => PIN_127
    
    ti1 => PIN_129
    ti2 => PIN_128
      
-- Pines de el lector RFID

    senalLectorRFTD => PIN_54
    senalLectorRFTD1 => PIN_55


 
 


