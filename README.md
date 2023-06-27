# Parqueadero Automatizado 

Autores:

- Manuel Felipe Carranza Montenegro
- Alejandro Mahecha Arango 
- Paula Daniela Ortiz Cuervo


# Contexto 
En Colombia existe una falta de cultura ciudadana en cuanto al respeto de aquellos lugares que se designaron para el estacionamiento exclusivo de personas en condición de discapacidad, ya que, a pesar de que estos lugares están señalizados, existen muchas personas que evaden esta normativa y estacionan sus vehículos en dichos lugares evitando una buena convivencia ciudadana. Además, las luces aun cuando no hay objetos en movimiento estan encendidas generando un desperdicio de energía


# Objetivos 

El objetivo principal del proyecto es crear un sistema que de acuerdo al tipo de población permita estacionarse o no en un espacio para personas con movilidad reducida o discapacidad.

Los objetivos secundarios son: 

1. Facilitar el encuentro de un estacionamiento tanto para personas con condición de discapacidad o movilidad reducida y para personas sin condición de discapacidad o sin movilidad reducida por medio de un sistema de conteo de estacionamientos vacíos de acuerdo a el tipo de población al que se pertenece.

2. Implementar un sistema que encienda las luces cuando detecte la presencia de un automóvil.    

 # Video Funcionamiento

Un video mostrando el funcionamiento del parqueadero es el siguiente :

https://github.com/ProyectoElectronicaDigitalI/Parqueadero/assets/136981880/a993ca85-be51-4a79-aad9-e6c09c588d87

# Video explicando el funcionamiento

https://drive.google.com/file/d/1fwyLbfMGOduUfkKHl8Czfe0cvFzkYFSm/view?usp=sharing
 
 # Diseño
 
 Para el proyecto se realizó una maquina de estados algoritmica como se observa en la siguiente imagen 
 
 
 
 ![Diagrama sin título drawio](https://github.com/ProyectoElectronicaDigitalI/Parqueadero/assets/136981880/418529f1-3701-472a-95b0-7b152010935e)

 Y se tiene el siguiente diagrama de bloques. 
 
 ![WhatsApp Image 2023-06-27 at 4 42 29 PM](https://github.com/ProyectoElectronicaDigitalI/Parqueadero/assets/136981880/87abee96-b723-4b72-abe8-f2197886aadb)

 
 
De manera que, para realizar la implementación de cada periferico se hizo lo siguiente:

A. Sensor de ultrasonido

Para el sensor de ultraonido se usó una libreria, debido a que se pensaba  que el sensor funcionara de la siguiente maneraa: primero que se hace es crear un divisor de frecuencia, esto con el fin de tener una frecuencia de     microsegundos, luego de esto, se mira que cada vez que halla un flanco de subida de la señal de microsegundos, se va a revisar una       serie de condiciones que son las siguientes:

1. Existe un contador auxiliar, mientras este contador esté entre 0 y 10 se manda un tren de pulsos desde el trigger, para enviar          este tren de pulsos el trigger se pone en 1. 

2. Mientras el contador inicial está en 0 el contador principal se inicializa en 0. 

3. Si el Echo recibe un 1 que significa que captó la señal del trigger reflejada, se aumenta el contador principal. 

4. Si el contador auxiliar llega a 249999 se debe reiniciar el contador auxiliar para enviar de nuevo un tren de pulsos, es decir,        si han pasado 0.249 segundos, en los que no se cumplen las condiciones anteriores (0.247 segundos en que no se recibe la señal           del trigger por el echo se vuelve a mandar la señal del trigger).

5. El contador auxiliar debe ir aumentando mientras no cumpla las anteriores condiciones.

De acuerdo a esas condiciones, luego nos interesa mirar según la señal del Echo como está el contador principal, entre menor sea el número de estee, más alejado está el objeto, de manera análoga, entre más cerca este el objeto mayor será el contador. Y conforme a eso se toma una señal de salida. 

Sin embargo, presentaba errores debido a que se estaba presentando que no se enviaba un 0 digital sino que lo tomaba aleatoriamente. Con la librería se solucionó este problema, tomando un 0 y un 1 digital. 

B. Pantalla LCD 

Para la pantalla LCD se descargó la librería para vhdl de 8 bits. Luego de ello, se definieron el número de instrucciones a utilizar, siendo las dos primeras la inicialización de la pantalla, luego los caracteres que se querían utilizar y por último se indica donde acaban las instrucciones. 

C. ServoMotor

El servomotor funciona en base a una señal que depende del ciclo util, este ciclo util es el que varia la posición del Servo Motor. Luego, en el codigo se relizó un pwm deacuerdo a una señal que se ingresa (selector) con el cual se cambia el ciclo util de una señal, que será la salida (PWM), y deacuerdo a esta salida se generará el movimiento del servo motor. 

D. Lector RFID

El código utiliza la biblioteca MFRC522 para el lector de tarjetas RFID (RC522) donde controla el nivel alto o bajo en función de las tarjetas RFID detectadas o no. El funcionamiento es el siguiente:

1. Se incluyen las bibliotecas necesarias para el uso del lector de tarjetas RFID (RC522) y la comunicación SPI.
   
2. Se definen los pines utilizados y los pines de salida para la fpga.

3. Se crea un objeto MFRC522 llamado mfrc522 utilizando los pines definidos anteriormente.
   
4. En la función setup(), se inicia la comunicación serial y SPI, se inicializa el RC522 y se configuran los pines de la fpga como salidas.
  
5. En la función loop(), se verifica si hay una tarjeta presente y se le asigna 1 a la salida (nivel alto) enviando un tren de dos pulsos.


E. Luces Led

Deacuerdo a los sensores de ultrasonido, se realiza una pequeña cadena de if, en la cuál dependiendo de si se detecta un objeto o no en el sensor del ultrasonido se prenden ciertos leds. 

F. Siete segmentos

Para los siete segmentos, fue necesario crear dos tierras, para lo cual se utilizó un multiplexor, cuya señal de control es la tierra y deacuerdo a ello se enciende uno u otro siete segmentos. Deacuerdo con esto ya se hizó la parte de una cadena de if para verificar cuales de los sensores de ultrasonido detectan un objeto, con esto se le da un valor en codigo BCD y luego se realiza la decodificación para llevarlo a los siete segmentos. 

Finalmente, teniendo ya todos los sensores se realizó lo siguiente:

1. Se crearon todas las señales necesarias, como entradas de los sensores y salidas del sistema, como los echo y trigger del sensor de ultrasonido, las señales del RFID, el clock, los leds. entre otras.
   
2. Se crearon todos los componentes, señales internas e instanciaciones.

3. se creó un process donde se observan los flancos de subida de las señales RFID y dependiendo de ello se abre el servomotor y se prenden las luces del camino. y tambien, se observan los flancos de subida del clock de la fpga para actualizar el siete segmentos.

4. Para el siete segmentos, se creó un modulo diferente donde se leen los flancos de subida de los sensores y con ellos se aumenta un contador, luego se crea un contador auxiliar donde se suman o restan los otros contadores y ese resultado se pasa por un case donde se le asigna el valor BCD que se pasará a los siete segmentos.

5. se crea otro proceso para hacer un multiplexor para las tierras de los siete segmentos.
  

# Simulaciones

se hizo una simulacion de los sensores con el código BCD que es la siguientes:
    
 ![WhatsApp Image 2023-06-27 at 4 57 17 PM](https://github.com/ProyectoElectronicaDigitalI/Parqueadero/assets/136981880/593562d8-504c-4ece-a9ff-fc6b0c6476fd)
 
 y la simulación del servo con el lector RFID


 ![Servo simulación](https://github.com/ProyectoElectronicaDigitalI/Parqueadero/assets/136981880/bfb33e4c-c161-417d-bda9-88c2c15fc8c7)

 # Avances del proyecto

Un avance del proyecto es el funcionamiento de los sensores de ultrasonido. En este caso se estan probando dos de los sensores. 
Esto se puede observar en el video llamado "Avances de los sensores de ultrasonido.mp4". Como en el video se explica lo que se hace es que el sensor verifica si hay un objeto y deacuerdo a ello cambia el contador, un contador para estacionamientos vacios para personas con discapacidad y otro contador para estacionamientos vacios para personas sin condición de discapacidad. 


https://github.com/ProyectoElectronicaDigitalI/Parqueadero/assets/136981880/e90f4291-cd2a-4f11-b272-2a990b862f99



Otro avance se observa en el video "Avance del servoMotor.mp4" en el cual, dependiendo de una configuración de pines en la fpga se mueve el servomotor, este servomotor se usará para permitir o no el paso o ingreso al parqueadero. 




https://github.com/ProyectoElectronicaDigitalI/Parqueadero/assets/136981880/0990db3b-26b5-465f-880e-21ee5bcb1639



Otro avance es el video "Avance de la pantalla LCD.mp4" en este video se ve como se escribe good en la pantalla, donde dependiendo del capacitor se varía el brillo de esta y se permite ver mejor o no la letra que está escrita. 



https://github.com/ProyectoElectronicaDigitalI/Parqueadero/assets/136981880/143900f9-2931-4657-9060-2d843d8b933f



Otro avance es un video donde se ven los sensores de ultrasonido en la maqueta que se realizó para simular el parqueadero. 



https://github.com/ProyectoElectronicaDigitalI/Parqueadero/assets/136981880/00607ac1-56f2-44cb-8115-d2d194ee64b0



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

-- Pines para el camino de leds

    LedsCamino[0]=>PIN_46
    LedsCamino[1]=>PIN_43
    LedsCamino[3]=>PIN_44
    LedsCamino[4]=>PIN_42
 
 


