# Parqueadero 

# Contexto 

En Colombia existe una falta de cultura ciudadana en cuanto al respeto de aquellos lugares que se designaron para el estacionamiento exclusivo de personas en condición de discapacidad, ya que, a pesar de que estos lugares están señalizados, existen muchas personas que evaden esta normativa y estacionan sus vehículos en dichos lugares evitando una buena convivencia ciudadana. Además, las luces aun cuando no hay objetos en movimiento estan encendidas generando un desperdicio de energía.  

# Objetivos 

El objetivo principal del proyecto es crear un sistema que de acuerdo al tipo de población permita estacionarse o no en un espacio para personas con movilidad reducida o discapacidad.

Los objetivos secundarios son: 

1. Facilitar el encuentro de un estacionamiento tanto para personas con condición de discapacidad o movilidad reducida y para personas sin condición de discapacidad o sin movilidad reducida por medio de un sistema de conteo de estacionamientos vacíos de acuerdo a el tipo de población al que se pertenece.

2. Implementar un sistema que encienda las luces cuando detecte la presencia de un automóvil.    

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

    servoMotor=> PIN_52
    
 # Terminar lo de los pines 
 
 
 # Avances del proyecto
 
 # Avances del proyecto

Un avance del proyecto es el funcionamiento de los sensores de ultrasonido. En este caso se estan probando dos de los sensores. 
Esto se puede observar en el video llamado "Avances de los sensores de ultrasonido.mp4". Como en el video se explica lo que se hace es que el sensor verifica si hay un objeto y deacuerdo a ello cambia el contador, un contador para estacionamientos vacios para personas con discapacidad y otro contador para estacionamientos vacios para personas sin condición de discapacidad. 








