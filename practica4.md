---
layout: page
title: "Práctica 4: Taxi Autonomo Gpp"
---

# Práctica 4: Taxi Autonomo Gpp

Programa un coche autónomo, equipado con un sensor GPS y un mapa de la ciudad, para que ejerza de taxi autónomo siendo capaz de planificar su navegación desde el lugar en el que se encuentra hasta otro punto de la ciudad como destino, utilizando el algoritmo de Gradient Path Planning.

## Vídeo Demostrativo y resultados medios

Video del mapeo y navegacion:

[![Vídeo mapeo y navegacion](https://img.youtube.com/vi/_plpmOx02Z0/0.jpg)](https://youtu.be/_plpmOx02Z0)

---

## Enfoque de la Solución y datos obtenidos
He implementado una ejecucion secuencial donde declaro variables a None, obtengo el mapa y crear el campo de penalizacion, dentro del while saco posiciones y en un if destino is not None and destino != ultimo_destino: para saber si es la primera vez que se corre el while o se cambia el destino y si es una celda segura y es carretera, entoncs pongo la velocidad a 0 mientrs se hacen los calculos necesarios, y inicializo un showpath a la poscion del robot para que no salga ruta porsi acaso reiniciar el mostrar ruta si no es la primera iteracion, depues calculo el campo, muestro la ruta y campo y debajo donde si if campo is not None and not destino_llegado: entonces se llama a la funcion que comprueba si se ha llegado al objetivo si es asi de pone a true destino_llegado, y sino se hace calcular siguiente paso y se manda a la funcion e mover. Y aqui estan explcadas las funciones mas importantes que implementé:

- calcular_penalizacion donde con un radio definido en mi caso 4, se calcula asignado una mayor penalizacion cuanto mas cerca se esta de la pared ((radio + 2) - mejor) de esta manera creando un repulsion de la pared hacia el centro de la carretera
- calcular_campo_propagacion que al seleccionar un destino se lanza una busqueda wave front con una cola de prioridad calculando el coste de la celda como el coste que ya habia en la casilla , mas el coste del apso 1 en N S E W y raiz de 2 si era en diagonal apollandome tambien en el mapa de penalizacion sumandole a los costes de penalizacion a las casillas, parando la ejecucion cuando se encuentra la casilla del robot, y aprovechando para que la expansion del mapa se haga de manera progresiva, hago un if con un contador para que cada 175 iteraciones se cojan las casillas con valor se pase de inf a -1 y hago el mostrar campo para mostrar la evolucion poco a poco 
-  calcular_siguiente_paso, en vez de mirar justo a la casilla en la que estoy miramos 5 pasos por delante para evitar zig zags, utilizando el campo haciendo lo que se dice en el eneunciado de descenso de gradiente para obtener la mejor casilla como opcion
-  mover, en esta funcion hago un control adaptativo, de velocidad sacando el angulo y distantica con math.atan2(math.sin(error), math.cos(error)) y hago     w = max(-WMAX, min(WMAX, K * error))    v = max(VMIN, VMAX * (1 - abs(error) / 1.0)) para sacar las velocidades, declarando K VMAX WMAX VMIN y peso_penalizacion para controlar mejor los valores
-  destino_conseguido donde se miran las casillas del al rededor de robot con un radio 1 y si coinciden con la casilla del objetivo devuelve true
-  mostrar_ruta, esta funcion la cree para mostrar la supuesta ruta que se deberia seguir en caso de crearla, pero NO se usa para que el robot la siga solo para mostrarse por pantalla la ruta idilica que se deberia seguir, pero el robor navega en cada iteracion

---

## Dificultades Encontradas y Soluciones
- Tenia los datos mal cogidos en filas y columnas porque worldToGrid([x, y]) devuelve columna, fila y yo lo cogia al reves, lo que me daban resultados no deseados
- Al principio intente hacerlo sin una heapq sino con una queue normal y al sumar costes cerca de esquinas se me creaban minimos locales que hacia que se chocara cuando no debia, implemente la heapq y se soluciono al instante
- Tenia errores graves tambien al crear el mapa donde tardaba mucho porque iteraba todas las casillas del mapa cada vez que se necesitaba y se mostraba por pantalla de manera erronea, asique imlemnte un break con un if en el mapa de propagacion donde sale al si la posicion del robot coincide con el destino a llegar

---

## Conclusión
El taxi consigue llegar eficientemente a todos los destinos de manera autonoma y navegando, tomando decisiones constantemente teniendo en cuenta los calculos previos teniendo en cuenta distancia a objetivo y los obstaculos en el mapa evitando choques y pasar cerca de paredes o esquinas para evita situaciones de peligro.
