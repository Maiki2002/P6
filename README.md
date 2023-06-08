# Práctica 6

## Funcionamiento

Con base a la práctica 5, el código y la conexión de la protoboard es modificado para generar un contador con 10 leds y tendrá interrupción de dos botones en el cuál uno tendrá la función de cambiar la velocidad del contador y el segundo cambiar su modo de operación (contador en negativo). Para esta práctica hacemos uso del dispositivo trigger, esto se debe a los botones que produce los botones.

## Main.s

Con base en el libro de Zhu, primero debemos:

- Configurar el reloj y los pines de entrada y salida.
- Configurar EXTI para los pines de entrada.
- Configurar NVIC para atender la solicitud de los EXTI
- Configurar Systick con el ciclo de reloj
- Usar la lógica del programa para la ejecución del BluePill

Ejemplo de código en alto nivel:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e7f63d8e-530c-47de-9e65-d597c08f4f84/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/06dee19e-2102-42af-8d7a-ef73986329b8/Untitled.png)

El marco de la pila de main es el siguiente:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a8653ff0-c9dc-4113-8e3a-5ce3c6c6b682/Untitled.png)

Los registros r8 y r9 son destinados para las variables de mode y speed.

## Check_Speed.s

Recibe speed y devuelve wait.

Su funcionamiento se basa en identificar que velocidad operará al presionar el botón. Su pseudocódigo es el siguiente:

```cpp
	if(speed == 1)
		return 1000 ms
	else if(speed == 2)
		return 500 ms
	else if(speed == 3)
		return 250 ms
	else if(speed == 4)
		return 125 ms
	else
		 speed = 1
		 return 1000 ms
```

## EXTI_Handler.s

El EXTI_Handler permite realizar acciones específicas en respuesta a eventos externos. Para eso utilizamos los pines A10 y A11.

## Systick_Handler.s

Disminuye la variable TimeDelay en uno cuando se genera una interrupción de Systick hasta llegar a cero.

Para la inicialización del Systick hay que establecerlo para generar interrupciones de tiempo.

## Delay.s

Esta función espera a que la variable TimeDelay llegue a cero.

## Output.s

Se usa una mascara que determina los leds a encenderse, en este caso son 10 leds.

## Compilación y grabación a la bluepill

Necesitaremos de 3 comandos para la compilación desde el archivo Makefile:

- make clean
- make
- make write

## Mapa de cableado
![P6](https://github.com/Maiki2002/P6/assets/105370860/5d9f19d6-cff6-45ad-9b2f-dc262be8484d)


