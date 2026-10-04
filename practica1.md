---
layout: page
title: "Práctica 1: Aspiradora"
---

# Práctica 1: Robot Aspiradora Autónomo

El objetivo de esta primera práctica es programar el cerebro de un robot aspiradora para que sea capaz de recorrer una casa virtual, esquivando obstáculos y maximizando el porcentaje de superficie limpiada, de forma similar a como lo haría una Roomba real.

## Vídeo Demostrativo

A continuación se muestra el resultado final de la ejecución, donde el robot logra limpiar la habitación alternando entre movimientos en línea recta y espirales para cubrir el centro de las salas.

*(Sustituye este bloque por el código de inserción "< >" de tu vídeo de YouTube oculto)*
<iframe width="560" height="315" src="https://www.youtube.com/embed/TU_ID_DE_VIDEO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## Enfoque de la Solución

Para dotar de inteligencia al robot, he implementado una **Máquina de Estados Finitos (FSM)**. En lugar de ejecutar acciones de forma secuencial y bloqueante, el robot evalúa continuamente los datos de su sensor láser frontal (a 50 Hz) y decide en qué estado debe estar.

La arquitectura se divide en 3 estados principales:
1. **ESTADO_AVANZAR (0):** El comportamiento por defecto. El robot avanza en línea recta a velocidad máxima mientras el láser central no detecte obstáculos cerca.
2. **ESTADO_EVITAR_OBSTACULO (1):** Cuando se detecta una pared, el robot frena y rota sobre sí mismo. Para evitar que se quede atrapado repitiendo el mismo ángulo en las esquinas, la velocidad y la duración del giro se calculan de forma aleatoria usando la librería `random`.
3. **ESTADO_GIROS_ALEATORIOS (Espiral) (2):** Tras esquivar una pared, el robot pasa a este estado durante unos segundos para trazar una espiral creciente. Esto es vital para cubrir los espacios abiertos en el centro de las habitaciones y no limitarse a rebotar de pared a pared.

---

## Dificultades Encontradas y Soluciones

Durante el desarrollo del algoritmo me encontré con varios retos técnicos importantes:

* **El problema de la "ceguera" temporal:** Inicialmente, utilizaba `time.sleep()` para controlar cuánto tiempo debía girar el robot. El problema es que esto congelaba el hilo de ejecución, impidiendo que el robot leyera los datos del láser durante el giro, lo que provocaba choques imprevistos.
* **La solución - Temporizadores no bloqueantes:** Sustituí los *sleeps* largos por comprobaciones continuas utilizando `time.time()`. Guardo una "hora límite" y permito que el bucle `while True` siga girando y leyendo sensores. 
* **Aceleración infinita en la espiral:** Al principio, el robot se estrellaba al retomar la espiral porque las variables de velocidad acumulaban los incrementos de ciclos anteriores. Lo solucioné asegurándome de reiniciar `v_espiral` y `w_espiral` a sus valores base justo en el momento de la transición de estado.

## Fragmento de Código Destacado

Esta es la lógica central de la transición segura hacia el movimiento en espiral, donde se comprueba el tiempo sin bloquear la lectura de los sensores:

```python
elif estadoActual == ESTADO_GIROS_ALEATORIOS:
    
    # Comprobamos el tiempo límite y aseguramos que no hay pared delante
    if (tiempo_actual < tiempo_limite and centroRobot >= 0.5):
        espiral(v_espiral, w_espiral)
        v_espiral = v_espiral + 0.0004
        w_espiral = w_espiral + 0.0006

    elif centroRobot < 0.5:
        # Si aparece un obstáculo imprevisto, abortamos la espiral
        estadoActual = ESTADO_EVITAR_OBSTACULO
    
    else:
        # Fin del tiempo de espiral, volvemos a explorar la sala
        estadoActual = ESTADO_AVANZAR
        tiempo_limite = time.time() + random.uniform(3.0, 6.0)
