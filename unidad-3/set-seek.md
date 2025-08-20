# Unidad 3

## 🔎 Fase: Set + Seek


## Actividad 9


### Modelado de las Fuerzas
En esta obra generativa se modelaron las fuerzas a partir de los conceptos de **Motion 101**. Cada partícula (representada como una esfera) se ve afectada por diferentes tipos de fuerzas:

- **Atracción hacia el mouse:** se calculó una fuerza de aceleración que dirige las partículas hacia la posición del cursor. Esto genera un movimiento interactivo que responde en tiempo real al usuario.  
- **Fuerza de fricción:** se implementó una reducción proporcional de la velocidad para evitar que las partículas se muevan indefinidamente y darle un comportamiento más natural.  
- **Fuerza aleatoria:** pequeñas variaciones aleatorias en la aceleración aportan un comportamiento orgánico, haciendo que los movimientos no sean totalmente predecibles.

### Relación Conceptual
La fuerza es el motor que guía el comportamiento visual de la obra. Conceptualmente, cada partícula representa un “agente” sensible al entorno y al espectador. La interacción del usuario (posición del mouse) se traduce en una fuerza que modifica la aceleración, generando un paisaje visual dinámico y siempre cambiante.

### Link
https://editor.p5js.org/quertuy1/sketches/1fRo4SEyl


### Fricción
```
let particles = [];
let mu = 0.05; // coeficiente de fricción

function setup() {
  createCanvas(600, 400);
}

function draw() {
  background(30);
  
  for (let p of particles) {
    let friction = p5.Vector.mult(p.vel.copy().normalize(), -mu);
    p.applyForce(friction);
    p.update();
    p.display();
  }
}

function mousePressed() {
  let p = new Particle(mouseX, mouseY);
  p.vel = createVector(random(-3, 3), random(-3, 3));
  particles.push(p);
}

class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
  }
  
  applyForce(f) {
    this.acc.add(f);
  }
  
  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }
  
  display() {
    noStroke();
    fill(200, 100, 255);
    ellipse(this.pos.x, this.pos.y, 10);
  }
}
```

### Resistencia al aire y fluidos

```
let particles = [];
let c = 0.01; // coeficiente de resistencia del aire

function setup() {
  createCanvas(600, 400);
}

function draw() {
  background(20, 40, 80);
  
  for (let p of particles) {
    // Fuerza de resistencia cuadrática: -c * v^2 * dirección
    let drag = p.vel.copy();
    let speed = p.vel.mag();
    if (speed > 0) {
      let dragMag = c * speed * speed;
      drag.normalize();
      drag.mult(-dragMag);
      p.applyForce(drag);
    }
    
    p.update();
    p.display();
  }
}

function mousePressed() {
  let p = new Particle(mouseX, mouseY);
  p.vel = createVector(random(-5, 5), random(-5, 5));
  particles.push(p);
}

class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
  }
  
  applyForce(f) {
    this.acc.add(f);
  }
  
  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }
  
  display() {
    noStroke();
    fill(100, 200, 255);
    ellipse(this.pos.x, this.pos.y, 12);
  }
}
```

### Atraccion gravitacional

```
let particles = [];
let G = 1; // constante gravitacional
let attractor;

function setup() {
  createCanvas(600, 400);
  attractor = createVector(width/2, height/2);
}

function draw() {
  background(10, 0, 20);
  
  fill(255, 200, 0);
  ellipse(attractor.x, attractor.y, 30); // "sol"
  
  for (let p of particles) {
    // Vector hacia el atractor
    let force = p5.Vector.sub(attractor, p.pos);
    let distanceSq = constrain(force.magSq(), 25, 500);
    let strength = G * (1 * 1) / distanceSq;
    force.setMag(strength);
    p.applyForce(force);
    
    p.update();
    p.display();
  }
}

function mousePressed() {
  let p = new Particle(mouseX, mouseY);
  p.vel = createVector(random(-2, 2), random(-2, 2));
  particles.push(p);
}

class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
  }
  
  applyForce(f) {
    this.acc.add(f);
  }
  
  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }
  
  display() {
    noStroke();
    fill(255);
    ellipse(this.pos.x, this.pos.y, 8);
  }
}

```


