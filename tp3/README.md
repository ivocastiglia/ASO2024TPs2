Arquitectura y Sistemas Operativos TP3
1a) 
El tiempo de ejecución del programa con hilos  fue menor al del programa sin hilos, El tiempo de ejecución se puede predecir, mas no siempre, ya que por un lado, podemos ver que el tiempo de ejecución del programa con hilos no varían de 5 segundos, pero los decimales cambian, lo mismo sucede en el otro programa pero con 8 seg.
b) Con mi compañero encontramos que hay menos variación de tiempo de ejecución en nuestros resultados del programa con hilos, que en los resultados del programa sin hilos ya que a él su programa sin hilos se le ejecutaba un poco más rápido que a mí.
c) 
Al des comentar el código es más lento y el valor final cambia, pasando de valer 0 a dar valores muy altos o muy bajos. Eso se debe a unos bucles que no hacen nada aumentando el tiempo de ejecución y el valor cambia, porque los dos hilos entran a “acumulador”, cambiando sus resultados.

2) 
```
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#define NUMBER_OF_THREADS 2
#define CANTIDAD_INICIAL_HAMBURGUESAS 20
int cantidad_restante_hamburguesas = CANTIDAD_INICIAL_HAMBURGUESAS;
int turno = 0;

void *comer_hamburguesa(void *tid)
{
	while (1 == 1)
	{ 
		while(turno!=(int)tid);

    // INICIO DE LA ZONA CRÍTICA
		if (cantidad_restante_hamburguesas > 0)
		{
			printf("Hola! soy el hilo(comensal) %d , me voy a comer una hamburguesa ! ya que todavia queda/n %d \n", (int) tid, cantidad_restante_hamburguesas);
			cantidad_restante_hamburguesas--; // me como una hamburguesa
		}
		else
		{
			printf("SE TERMINARON LAS HAMBURGUESAS :( \n");
			turno = (turno + 1)% NUMBER_OF_THREADS;
			pthread_exit(NULL); // forzar terminacion del hilo
		}
    // SALIDA DE LA ZONA CRÍTICA   
		turno = (turno + 1)% NUMBER_OF_THREADS;
	}
}

int main(int argc, char *argv[])
{
	pthread_t threads[NUMBER_OF_THREADS];
	int status, i, ret;
	for (int i = 0; i < NUMBER_OF_THREADS; i++)
	{
		printf("Hola!, soy el hilo principal. Estoy creando el hilo %d \n", i);
		status = pthread_create(&threads[i], NULL, comer_hamburguesa, (void *)i);
		if (status != 0)
		{
			printf("Algo salio mal, al crear el hilo recibi el codigo de error %d \n", status);
			exit(-1);
		}
	}

	for (i = 0; i < NUMBER_OF_THREADS; i++)
	{
		void *retval;
		ret = pthread_join(threads[i], &retval); // espero por la terminacion de los hilos que cree
	}
	pthread_exit(NULL); // como los hilos que cree ya terminaron de ejecutarse, termino yo tambien.
}
```
