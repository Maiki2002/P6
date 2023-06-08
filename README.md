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
![1](https://github.com/Maiki2002/P6/assets/105370860/fd31a6a7-ec02-467a-b0ad-4a4956885bc6)

![2](https://github.com/Maiki2002/P6/assets/105370860/9e4ee44b-8eda-40f3-a963-db4c7b2afe68)

El marco de la pila de main es el siguiente:

![3](https://github.com/Maiki2002/P6/assets/105370860/ec80a50b-e0ac-4ff1-8125-874ce0fb563b)


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


