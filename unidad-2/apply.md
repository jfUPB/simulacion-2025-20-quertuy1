# Unidad 2


## Descripción de la obra generativa
Título: Orbital Dance
Es una pieza visual en la que varias partículas giran y flotan alrededor del cursor del mouse, como si estuvieran en una órbita caótica. Cada partícula calcula su aceleración en función de una mezcla entre atracción hacia el mouse y una rotación suave, lo que crea patrones dinámicos e hipnóticos.

## Aplicación del marco Motion 101
Voy a usar Motion 101 porque es perfecto para describir cómo las partículas se mueven:

Aceleración: se calcula en cada frame para que las partículas giren y se acerquen al mouse.

Velocidad: se modifica sumando la aceleración.

Posición: se actualiza sumando la velocidad.

Este flujo simple me permite controlar fácilmente el movimiento con vectores y añadir comportamientos visuales interesantes.

## Algoritmo de aceleración
En vez de solo usar aceleración constante o aleatoria, voy a usar una aceleración orbital, que combina:

Un vector de atracción hacia el mouse.

Un vector perpendicular para crear un efecto de giro.

¿Por qué?
Porque así las partículas no solo se mueven hacia el cursor, sino que lo hacen describiendo trayectorias circulares y más artísticas.

## Interactividad
Mouse: cambia el centro de atracción de las partículas.

Tecla espacio: reinicia posiciones aleatorias.

La distancia al mouse afecta la fuerza de atracción.

### [Ver código en p5.js](https://editor.p5js.org/quertuy1/sketches/VTrfbONQn)


``` js 
let particles = [];

function setup() {
  createCanvas(600, 400);
  for (let i = 0; i < 50; i++) {
    particles.push(new Particle(random(width), random(height)));
  }
}

function draw() {
  background(20, 20, 30, 50);
  for (let p of particles) {
    p.update();
    p.show();
  }
}

class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);
  }

  update() {
    // Vector hacia el mouse
    let mouse = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(mouse, this.pos);
    let distSq = constrain(dir.magSq(), 50, 5000);
    dir.setMag(1 / distSq * 200); // fuerza de atracción

    // Vector perpendicular para órbita
    let orbit = dir.copy().rotate(HALF_PI).mult(0.5);

    // Aceleración final = atracción + órbita
    this.acc = p5.Vector.add(dir, orbit);

    // Motion 101
    this.vel.add(this.acc);
    this.vel.limit(4);
    this.pos.add(this.vel);
  }

  show() {
    noStroke();
    fill(200, 100, 255);
    ellipse(this.pos.x, this.pos.y, 6);
  }
}

function keyPressed() {
  if (key === ' ') {
    for (let p of particles) {
      p.pos = createVector(random(width), random(height));
      p.vel = p5.Vector.random2D();
    }
  }
}
```




## 🛠 Fase: Apply
