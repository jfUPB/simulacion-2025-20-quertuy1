# Unidad 3

## 🔎 Fase: Set + Seek


## Actividad 9

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
