---
layout: page
title: "Práctica 1: Aspiradora"
---

# Práctica 1: Robot Aspiradora Autónomo

El objetivo de esta práctica es programar una aspiradora automática que recorra el mayor espacio posible.

## Vídeo Demostrativo

A continuación se muestra el resultado final de la ejecución, donde el robot logra limpiar la habitación alternando entre movimientos en línea recta y espirales para cubrir el centro de las salas. He conseguido un 47% en 12 minutos de vídeo y, dejándolo un rato más, he llegado a un 84,72%.

[![Vídeo demostrativo de la Práctica 1](https://img.youtube.com/vi/Dfyiy2HQqYU/0.jpg)](https://www.youtube.com/watch?v=Dfyiy2HQqYU)

Hay que hacer click en la imagen superior para acceder al video

<img width="1920" height="1080" alt="Captura desde 2026-10-05 11-41-37" src="https://github.com/user-attachments/assets/6c2cd873-3f8a-4779-80b8-16d7d0420145" />

---

## Enfoque de la Solución

Para hacer el mejor recorrido posible he utiizado dos ideas principales primero una maquina de estados finitos con las siguiente estructura.
La arquitectura se divide en 4 estados principales:
1. **ESTADO_AVANZAR:** Es el comportamiento deffault, pero para que no recorra de punta a punta la casa entera y esquive zonas si barrer le incluí un if con una condicion de tiempo para que evite ir demasiado tiempo en linea recta, y
2. **RETROCEDER:** Al detectar un obstáculo cerca (< 0.5m), el robot invierte los motores brevemente para separarse de la pared y ganar espacio de maniobra.
3. **GIRAR:** Rotación pseudoaleatoria para cambiar la trayectoria, controlada por un temporizador no bloqueante.
4. **ESPIRAL:** Tras esquivar una pared, el robot pasa a este estado durante unos segundos para trazar una espiral creciente. Esto es vital para cubrir los espacios abiertos en el centro de las habitaciones y no limitarse a rebotar de pared a pared.

Y la segunda implementación que hice fue meter la libreria random para muchas decision añadiendo un grado de alietorialidad para que el sistema no sea repetitivo y a la larga pueda llegar a evitar el mayor numero de atascos y situaciones sin salida.

---

## Dificultades Encontradas y Soluciones

Durante el desarrollo me encontré con varios retos :

* **El problema de la "ceguera" temporal:** Inicialmente, utilizaba `time.sleep()` para controlar cuánto tiempo debía girar el robot. El problema es que esto congelaba el hilo de ejecución, impidiendo que el robot leyera los datos del láser durante el giro, lo que provocaba choques imprevistos.
* **La solución - Temporizadores no bloqueantes:** Sustituí los *sleeps* largos por comprobaciones continuas utilizando `time.time()`. Guardo una "hora límite" y permito que el bucle `while True` siga girando y leyendo sensores. 
* **Aceleración infinita en la espiral:** Al principio, el robot se estrellaba al retomar la espiral porque las variables de velocidad acumulaban los incrementos de ciclos anteriores. Lo solucioné asegurándome de reiniciar `v_espiral` y `w_espiral` a sus valores base justo en el momento de la transición de estado.


## Conclusión
El sistema con el tiempo suficiente (30 minutos aproximadmente) puede cubrir casi el 100% del terreno a rellnar en el ejemplo pero pasando muchas veces por el mismo sitio, pero esto significa que la aleatoriedad si ayuda a cubrir muchas zonas en terros irregulares y descnocidos, es una buena solucion temporal, pero para poder implemntar un robot mas rápido y eficaz habria que aplicarle una memoria al robot y el uso de mas sensores para unas mayor orientcacion en el entorno.
