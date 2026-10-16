---
layout: page
title: "Práctica 2: Coche Siguelineas"
---

# Práctica 2: Coche Siguelineas

El objetivo de esta práctica es programar un coche de formula 1 que siga una linea para recorrer un circuito en el menor tiempo posible.

## Vídeo Demostrativo y resultados medios

A continuación los video de los ciruitos:

Simple Circuit:

[![Vídeo Circuit Simple](https://img.youtube.com/vi/UazcKyjHYI4/0.jpg)](https://www.youtube.com/watch?v=UazcKyjHYI4)

Montreal Circuit:

[![Vídeo Montreal Circuit](https://img.youtube.com/vi/w6VvSaDyZV0/0.jpg)](https://www.youtube.com/watch?v=w6VvSaDyZV0)

Montmelo Circuit:

[![Vídeo Montmelo Circuit](https://img.youtube.com/vi/yc_btENONeo/0.jpg)](https://www.youtube.com/watch?v=yc_btENONeo)

Nurburgring Circuit:

[![Vídeo Nurburgring Circuit](https://img.youtube.com/vi/vdcb4LT5Eo4/0.jpg)](https://www.youtube.com/watch?v=vdcb4LT5Eo4)

---

## Enfoque de la Solución y datos obtenidost

Para buscar el mejor funcionamiento de el sistema hice ciertas implemntaciones como las siguientes:
- Funciones de calcular_w con el pid implementado, en este caso es un pd solo pero tambien inclui el factor de integración para que sea un codigo general y reusable.
- calcular_v donde uso un controlador p donde la formula resumidamente es v = vmax - kv * abs*(error/320), kv = vmax - vmin, esto lo hago asi para que cuando el error sea muy bajo, se le reste muy poco a la velocidad maxima como en una recta, y cuando error sea muy alto la velocidad se vera muy reducida
- hago la mascara de colores de rojo por debajo y arriba entre 0 y 10 y entre 170 y 179 y las sumo asi solo vere la linea roja
- Saco los momentos de la mascara y saco el centro, ahi saco el error restantole a 320 que es el centro de la linea, y con el error y el error anterior saco la w y v en las funciones con el pid
- Para cuando el coche sale de la linea y la pierde de vista hago la función de else donde if busca a ver si hay deteccionees de lineas en M["m00"] y si no las hay en el else se actualiza la velocidad v a 0 y w a 1 osea que girara lentamente hasta que encuentre la linea y M["m00"]>0
- Todo esto me ha hecho tener un sistema bastante estable donde se promedian los siguientes resultados:
    - Simple circuit 40,81 segundos
    - Montreal Circuit 127,28 segundos
    - Montmelo Circuit 71,20 segundos
    - Nurburgring Circuit 65,66 segundos


---

## Dificultades Encontradas y Soluciones

- Al principio cuando solo tenia un controlador p pues las curvas se me iban rapidisimo y se salian, entonces implementé un controlador d empezando a restar el error con el anterior y multiplicandolo por mi kd con el que se ajusta la velocidad de giro a la par que la diferencia de error ahi resolvi el primer problema
- Despues estuve probando valores y con unos se me salia demasiado de las curvas y con otros me temblaba en las rectas asique acabe optando por u punto medio donde mass rapido era con 0,25 que no tiembla en rectas y solo un poco en alguna curva si la linea hace algun dibujo un poco desuniforme, pero se ajusta muy bien a las curvas  y recupera el centro del carril muy rapido
- Tambien intenté implementar un integrador pero vi que primeramente para esta practica no era necesario y que me daba un error que era que ha sumar todos los erros rapidamente me escalaba a un numero muy alto y me daban giros exagerados llegandose a salir ed la pista, asique acabe reiniciando cada 1000 iteraciones summa_errores = 0
- Por ultimo cuando el coche me hacia las vueltas entera intente aumentar la velocidad maxima que la tenia en 6 para hacer pruebas, y lo mejor que consegui de velocidad evitando respuestas raras y desvios descontrolados fue 12 moviendo tambien los controladores hasta la distribucion que tengo ahora, llegue a obtener buenos resultados con 18 y un mayor kd pero habia circuitos en los que no trazaba del todo bien asique decidí dejar la implemntacion actual que no me da para nada malos tiempos de resultado.

---

## Conclusión
El coche hace tiempos considerablemente buenos, haciendo curva ajustadas aunque con un poco de oscilacion en curvas muy cerradas, pero girando con precisión y velocidad mientras aminora la velocidad depende de lo cerrada que sea la curva, y acelerando el rectas para una mayor fluidez del coche
