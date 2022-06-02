# Práctica 4a

El objetivo de la practica es comprender el funcionamiento de un sistema operativo en tiempo
Real.

## Código:

```
#include <Arduino.h>
 
void anotherTask(void * parameter);

void setup()
{
Serial.begin(112500);
/* we create a new task here */
xTaskCreate(anotherTask, "another Task",10000, NULL, 1, NULL); 
}
 
/* the forever loop() function is invoked by Arduino ESP32 loopTask */
void loop(){
    Serial.println("this is ESP32 Task");
    delay(1000);
}
 
/* this function will be invoked when additionalTask was created */
void anotherTask( void * parameter ){
    /* loop forever */
    for(;;)
    {
        Serial.println("this is another Task");
        delay(1000);
    }
    /* delete a task when finish,
    this will never happen because this is infinity loop */
    vTaskDelete( NULL );
}
```

## Salida por el puerto serie:

La salida que mostrará el puerto serie serán dos mensajes a la vez, 1 sale des de loop del programa y el otro con el uso de una tasca. Saldrá siempre lo mismo ya que son bucles infinitos.

Mensaje por pantalla:

![Terminal_4](Terminal_4.PNG)


## Funcionamiento:

En el siguiente código hemos creado la tasca:
```
void setup(){
Serial.begin(112500);
/* we create a new task here */
xTaskCreate(anotherTask, "another Task",10000, NULL, 1, NULL); 
}
```

Donde:

    * anotherTask: Subprograma que llamamos.
    * "another thask": Nombre del subprograma.
    * 10000: Tamaño en bytes.
    * NULL: Parámetro a pasar.
    * 1: Prioridad de la tasca.
    * NULL:Identificador de tareas.

Dentro del loop ponemos la frase que queremos que imprima en la pantalla (en este caso:"this is ESP32 Task") y después un delay para que no lo escriba a cada instante.

Despues usamos la funcion "anotherTask" y le ponemos el mismo delay, este subprograma imprimirá por pantalla "this is another Task" y lo hará infinitamente con mayor prioridad que el propio loop del programa (ya que tiene prioridad 1).