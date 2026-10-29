---
layout: page
title: "Práctica 3: Coche esquiva ostáculos"
---

# Práctica 3: Coche esquiva ostáculos

El objetivo de esta práctica es programar un coche de formula 1 que esquiva obsatculos y completa el circuito entero 

## Vídeo Demostrativo y resultados medios

Video circuito:

[![Vídeo Circuito](https://img.youtube.com/vi/GmHnPvGBCSg/0.jpg)](https://www.youtube.com/watch?v=GmHnPvGBCSg)

---

## Enfoque de la Solución y datos obtenidos

- Fuerza atractiva con absolute2relative y paso el obejtivo a sistema de ref del f1 donde el vector resultante apunta desde el morro al objetivo y si el módulo supera maxatr lo recorto para que un objetivo lejano no anule a laa fuerza repulsiva
- Fuerza repulsiva, el laser genera empuje en sentido contrario al obstaculo con modulo proporcional 1/d² asi que lo cercano pesa mucho mas que lo lejano  tambien ignoro lecturas invalidas como inf y nan demasiado pegadas (< 0.1 m) y lejanas (> 3 m) y uso un proporcional krep par modular
- sumo ambas fuerzas y añado un offset constante en x
- la componente x de la fuerza resultante define v = kv * fuerza_x y la componente y define w = kw * fuerza_y y limitandose con v_max y w_max
- cuando el coche está a menos de 1.5 m del objetivo, lo marco con setReached(True)

- Los parametros finales de controladores y velocidades las he definido mediante el fallo y erro quedandome con vmax = 7,wmax = 3,kv = 2.0, kw = 1.2,krep = 0.15,offset = 3.0, maxatr = 4.0

---
## Dificultades Encontradas y Soluciones

- Laser solo recibia 180 grados por delante asique la x salia negativa lo que frenaba el coche lo resolvi añadiendo el offset constante en x
- Cuando un rayo no detectaba nada el laser devolvía inf dando fuerzas raras dejando el coche parado, lo resolvi filtrando esas lecturas en la repulsiva.
- La atractiva dominaba a la repulsiva como estaba el objetivo lejos su modulo era la distancia real y el coche ignoraba los obstaculos que solucione añadiendo maxatr
- Sumarlos 180 rayos da valores mucho mayores que la atractiva asi que inclui krep y probe valores hasta equilibrarlas
- Hay curvas donde las fuerzas se igualan y el coche se queda avanzando un tramo a velocidad reducida, no consegui incluir una implementacion que resolviera esto sin afectar al resto del circuito assique como consigo promediar 2 minutos en recorrer el circuito sin choques he optado por dejarlo asi aunq duramente 4 segundos vaya un poco lento en la curva

---

## Conclusión
Pues este vff permite una gran adaptabilidad de coches a circuitos de todo tipo donde nos aseguraremos exito en la mayoria de caso obviamente viables, funciona bien con una media de 2 minutos por circuitos sin golpearse con nada, lo malo es que al depender de las fuerzas hay situacione raras donde se anulan como en una curva de este circuito donde circula a velocidad minima unos segundos sin obstaculos, pero es un caso aislado que no influye gravemente ya que consigue adaptarse seguir esquivando obstaculos y cumplir el circuito en el tiempo pedido. Otra cosa es que mi sistema esta extremadamente paretimizado entonces es bueno porque es muy ajustable en situaciones reales donde se podra adaptar a cambios en el entorno.
