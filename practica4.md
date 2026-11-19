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

- calcular_penalizacion donde con un radio definido en mi caso 4, se calcula asignado una mayor penalizacion cuanto mas cerca se esta de la pared ((radio + 2) - mejor) de esta manera creando un repulsion de la pared 

---
## Dificultades Encontradas y Soluciones

- Laser solo recibia 180 grados por delante asique la x salia negativa lo que frenaba el coche lo resolvi añadiendo el offset constante en x
- Cuando un rayo no detectaba nada el laser devolvía inf dando fuerzas raras dejando el coche parado, lo resolvi filtrando esas lecturas en la repulsiva.
- La atractiva dominaba a la repulsiva como estaba el objetivo lejos su modulo era la distancia real y el coche ignoraba los obstaculos que solucione añadiendo maxatr
- Sumarlos 180 rayos da valores mucho mayores que la atractiva asi que inclui krep y probe valores hasta equilibrarlas
- Hay curvas donde las fuerzas se igualan y el coche se queda avanzando un tramo a velocidad reducida, no consegui incluir una implementacion que resolviera esto sin afectar al resto del circuito assique como consigo promediar 2 minutos en recorrer el circuito sin choques he optado por dejarlo asi aunq duramente 4 segundos vaya un poco lento en la curva

---

## Conclusión
