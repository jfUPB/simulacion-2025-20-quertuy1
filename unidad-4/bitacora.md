# Evidencias de la unidad 4

## Explicación conceptual de la obra
La obra esta basada en el movimiento pendular con aceleracion aleatoria

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Tu respuesta aquí: Movimiento Pendular el cual se aplica en la oscilacion del pendulo
>

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> Tu respuesta aquí: Aceleracion y velocidad, se ven aplicados en el movimiento de la esfera ya que este se basa en la interaccion entre ellas
>

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Tu respuesta aquí: la posición del pendulo se calcula usando coordenadas basadas en ángulo y radio.
>

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Tu respuesta aquí: se aumenta la velocidad del pendulo aleatoriamente en un rango definido al presionar la tecla x
>

## ¿Cómo resolviste la interacción?
> Tu respuesta aquí: la interaccion esta en controlar la gravedad con el mouse, dependiendo la posicion del mouse va a variar la gravedad ademas de que si se presiona la x la esfera obtendra un in
>

## Enlace a la obra en el editor de p5.js

[Aquí está mi obra](https://editor.p5js.org/quertuy1/sketches/GNcA22eD_)

## Código de la obra 

``` js
let r = 150;       // largo del péndulo
let angle;         // ángulo del péndulo
let aVel = 0;      // velocidad angular
let aAcc = 0;      // aceleración angular
let gravity = 0.4; // gravedad base

function setup() {
  createCanvas(600, 400);
  angle = PI / 4; // arranca inclinado
}

function draw() {
  background(30, 30, 40, 50);

  // gravedad controlada con mouse (interactividad)
  gravity = map(mouseX, 0, width, 0.05, 1);

  // movimiento pendular
  aAcc = (-1 * gravity / r) * sin(angle);
  aVel += aAcc;
  angle += aVel;
  aVel *= 0.99; // amortiguación

  // posición del péndulo (vector implícito)
  let x = r * sin(angle) + width / 2;
  let y = r * cos(angle) + 50;

  // dibujar cuerda y bolita
  stroke(220);
  line(width / 2, 50, x, y);
  fill(200, 255, 150);
  ellipse(x, y, 20);

  // trazo generativo con aleatoriedad
  noStroke();
  fill(random(100, 255), random(100, 255), random(100, 255), 100);
  ellipse(x, y, 5);
}

// cada vez que se presiona "x", se da un impulso aleatorio
function keyPressed() {
  if (key === 'x' || key === 'X') {
    aVel += random(-0.2, 0.2); 
  }
}

```

## Captura de pantalla representativa

![Captura del péndulo](https://cdn.discordapp.com/attachments/716834985306488894/1413498889394323486/image.png?ex=68bc26ec&is=68bad56c&hm=436b7e9aa74eebf83dcd9b74b1f4be9b25a05fb73b2032aaff744e660368f321)





