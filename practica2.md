---
layout: page
title: "Práctica 2: Siguelineas"
---

# Práctica 2: Robot Siguelineas

El objetivo de esta práctica es programar un coche de formula 1 que siga una linea para recorrer un circuito en el menor tiempo posible.

## Vídeo Demostrativo

A continuación los video de los ciruitos

[![Vídeo Circuit Simple](https://img.youtube.com/vi/Dfyiy2HQqYU/0.jpg)](https://www.youtube.com/watch?v=Dfyiy2HQqYU)

Hay que hacer click en la imagen superior para acceder al video

<img width="1920" height="1080" alt="Captura desde 2026-10-05 11-41-37" src="https://github.com/user-attachments/assets/6c2cd873-3f8a-4779-80b8-16d7d0420145" />

---

## Enfoque de la Solución

Para buscar el mejor funcionamiento de el sistema hice ciertas implemntaciones como las siguientes:
- Funciones de calcular_w con el pid implementado, en este caso es un pd solo pero tambien inclui el factor de integración para que sea un codigo general y reusable.
- calcular_v donde uso un controlador p donde la formula resumidamente es v = vmax - kv * abs*(error/320), kv = vmax - vmin, esto lo hago asi para que cuando el error sea muy bajo, se le reste muy poco a la velocidad maxima como en una recta, y cuando error sea muy alto la velocidad se vera muy reducida
- hago la mascara de colores de rojo por debajo y arriba entre 0 y 10 y entre 170 y 179 y las sumo asi solo vere la linea roja
- Saco los momentos de la mascara y saco el centro, ahi saco el error restantole a 320 que es el centro de la linea, y con el error y el error anterior saco la w y v en las funciones con el pid
- Para cuando el coche sale de la linea y la pierde de vista hago la función de else donde if busca a ver si hay deteccionees de lineas en M["m00"] y si no las hay en el else se actualiza la velocidad v a 0 y w a 1 osea que girara lentamente hasta que encuentre la linea y M["m00"]>0

## Dificultades Encontradas y Soluciones

- Al principio cuando solo tenia un controlador p pues las curvas se me iban rapidisimo y se salian, entonces implementé un controlador d empezando a restar el erro con el anterior y multiplicandolo por mi kd ahi resolvi el primer problema
- despues estuve probando valores y con unos se me salia demasiado de las curvas y con otros me temblaba en las rectas asique acabe optando por u punto medio donde mass rapido era con 0,38
- Tambien intenté implementar un integrador pero vi que primeramente para esta practica no era necesario y que me daba un error que era que ha sumar todos los erros rapidamente me escalaba a un numero muy alto y me daban giros exagerados llegandose a salir ed la pista, asique acabe reiniciando cada 1000 iteraciones summa_errores = 0

## Conclusión
El sistema con el tiempo suficiente (30 minutos aproximadmente) puede cubrir casi el 100% del terreno a rellnar en el ejemplo pero pasando muchas veces por el mismo sitio, pero esto significa que la aleatoriedad si ayuda a cubrir muchas zonas en terros irregulares y descnocidos, es una buena solucion temporal, pero para poder implemntar un robot mas rápido y eficaz habria que aplicarle una memoria al robot y el uso de mas sensores para unas mayor orientcacion en el entorno.
