# Evidencias de la unidad 6

### En la imagen 1 aunque el tenga un flujo predefinido y parece que tenga ciertas directrices, no se siente que sea forzado sino mas bien que todo fluye en la imagen de una forma natural y fluida lo cual es hasta medio hipnotico si se intenta seguir cada linea e intentar encontrar la trayectoria de cada una
![Imagen1](https://cdn.discordapp.com/attachments/716834985306488894/1423626324919980082/descarga.jpeg?ex=68e0fed5&is=68dfad55&hm=7876b82e2a0c670032497ccbd1ab887e71429bdd52027a9fe71ba294467c4fc8&)


### En la imagen 2 se siente que hay mas libertad, que son lineas que siguen su propio camino aunque de igual forma que en la imagen 1 tengan una direccion, no influye tan estrictamente en el movimiento o en el trazo de cada linea lo cual lo hace una obra que refleja cierta frescura y el hecho de que parece que los colores tuvieran cierto rango en la pantalla lo hace tambien interesate
![Imagen2](https://cdn.discordapp.com/attachments/716834985306488894/1423626325477953607/a7d505a47c94d7fc90afd09bc6362a3eebbad1b4-4001x2250.avif?ex=68e0fed5&is=68dfad55&hm=ae2480f31b34edc617bfb42b1b56517b7a2f23a81e68bdfc6ca24e404dae9570&)


### la fuerza de direccion decide hacia dónde se movera el agente en cada momento que interactue con el  para lograr una accion deseada, como seguir un camino o mantener distancia de otros.

### Las fuerzas tratadas anteriormente (como gravedad, fricción o resistencia) suelen ser fisicas y actúan siempre de la misma forma sin importar la intención del agente.

### Las fuerzas de direccion son fuerzas de comportamiento, osea que  dependen de la posición, dirección y velocidad relativa del agente y tienen la ventaja de que pueden cambiar en cada frame para generar movimientos más inteligentes y fluidos.
### en pocas palabras A diferencia de las fuerzas físicas que surgen naturales las fuerzas de direccion siguen reglas de decision.

### Su trabajo mostro que usando fuerzas de direccion en cada agente sin necesidad de que fueran las mas complejas se podia obtener patrones emergentes muy realistas, lo cual influyó en la animación, los videojuegos y la inteligencia artificial para simulaciones de multitudes.

```javascript

// Holdin' Flow + Paletas de Pinceles
// Requiere p5.sound. Sube "theres_nothing.mp3" al sketch o presiona 'm' para mic.
// Interacciones:
//  - mouse move: desplaza centro del field
//  - mouse wheel: ajusta fuerza del flow
//  - space: play/pause audio
//  - z: disparo manual tipo bombo + onda de ATRACCIÓN + invierte colores
//  - c: onda de REPULSIÓN + cambia a otra paleta
//  - m: alternar mic input
//  - 1..4: presets de cantidad de partículas / boids

let flow;
let particles = [];
let boids = [];
let cols, rows;
let scl = 20;
let zoff = 0;
let flowStrength = 1.0;

let soundFile;
let fft, amp;
let useMic = false;
let mic;
let playing = false;

let waves = []; // Ondas activas

// 🔹 Paletas de colores
let palettes = [
  {bass: [200,90,100], treble: [50,90,100], vocal: [0,90,100]},
  {bass: [120,90,100], treble: [300,90,100], vocal: [30,90,100]},
  {bass: [0,90,100], treble: [180,90,100], vocal: [240,90,100]}
];
let currentPaletteIndex = 0;
let invertColors = false;

function preload() {
  if (typeof loadSound === 'function') {
    try {
      soundFile = loadSound('theres_nothing.mp3');
    } catch(e) {
      soundFile = null;
    }
  }
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100, 1);
  background(0);
  cols = floor(width / scl) + 1;
  rows = floor(height / scl) + 1;
  flow = new Array(cols * rows);

  for (let i = 0; i < 400; i++) {
    particles.push(new FlowParticle(random(width), random(height)));
  }

  for (let i = 0; i < 80; i++) {
    boids.push(new Boid(random(width), random(height)));
  }

  fft = new p5.FFT(0.8, 128);
  amp = new p5.Amplitude();

  if (soundFile) {
    fft.setInput(soundFile);
    amp.setInput(soundFile);
  }
}

function draw() {
  // fondo translúcido
  noStroke();
  fill(0, 0.0, 0.05);
  rect(0, 0, width, height);

  updateFlow();

  let spectrum = fft.analyze ? fft.analyze() : new Array(128).fill(0);
  let level = amp.getLevel ? amp.getLevel() : 0;

  // 🔹 Rangos para pinceles
  let bass = fft.getEnergy("bass");      // Bajos
  let treble = fft.getEnergy("treble");  // Agudos
  let vocalBand = fft.getEnergy(300, 1200); // voz

  // 🔹 Mostrar y actualizar ondas
  for (let w of waves) {
    w.update();
    w.display();
    for (let p of particles) w.applyTo(p.pos, p);
    for (let b of boids) w.applyTo(b.pos, b);
  }
  waves = waves.filter(w => !w.finished);

  // 🔹 Paleta actual
  let palette = palettes[currentPaletteIndex];

  // Partículas con pinceles
  for (let p of particles) {
    let v = lookupFlow(p.pos.x, p.pos.y).copy();
    v.mult(flowStrength);

    p.applyForce(v);
    p.update();
    p.edges();

    // Selección de color y tamaño según pincel
    let col, sz;
    if (bass > 180) {
      col = palette.bass;
      sz = 4;
    } else if (treble > 180) {
      col = palette.treble;
      sz = 2;
    } else if (vocalBand > 150) {
      col = palette.vocal;
      sz = 6;
    } else {
      col = [180, 20, 50];
      sz = 1;
    }

    // Invertir colores
    let finalColor = invertColors ? [(col[0] + 180) % 360, col[1], col[2]] : col;

    p.displayCustom(finalColor, sz);
  }

  // Boids
  for (let b of boids) {
    b.flock(boids);
    let fv = lookupFlow(b.pos.x, b.pos.y).copy().mult(0.5 * flowStrength);
    b.applyForce(fv);
    b.cohesionFactor = map(bass, 0, 255, 0.005, 0.05);
    b.update();
    b.edges();
    b.display(bass, treble);
  }

  zoff += 0.003;
}

function updateFlow() {
  let mx = mouseX || width/2;
  let my = mouseY || height/2;
  for (let y = 0; y < rows; y++) {
    for (let x = 0; x < cols; x++) {
      let i = x + y * cols;
      let nx = x * 0.1;
      let ny = y * 0.1;
      let angle = noise(nx, ny, zoff) * TWO_PI * 2;
      let dx = mx - x * scl;
      let dy = my - y * scl;
      let d2 = dx*dx + dy*dy;
      let attract = map(1.0 / (d2 + 1), 0, 1, -0.6, 0.6);
      let v = p5.Vector.fromAngle(angle + attract);
      flow[i] = v;
    }
  }
}

function lookupFlow(px, py) {
  let x = constrain(floor(px / scl), 0, cols - 1);
  let y = constrain(floor(py / scl), 0, rows - 1);
  return flow[x + y * cols];
}

// ---------------- CLASE PARA LAS ONDAS ----------------
class Wave {
  constructor(x, y, type) {
    this.x = x;
    this.y = y;
    this.radius = 0;
    this.maxRadius = max(width, height);
    this.type = type; // 'attract' o 'repel'
    this.finished = false;
  }

  update() {
    this.radius += 12;
    if (this.radius > this.maxRadius) {
      this.finished = true;
    }
  }

  display() {
    noFill();
    stroke(this.type === 'attract' ? color(200, 80, 100, 0.5) : color(0, 80, 100, 0.5));
    strokeWeight(2);
    ellipse(this.x, this.y, this.radius * 2);
  }

  applyTo(pos, obj) {
    let d = dist(this.x, this.y, pos.x, pos.y);
    if (abs(d - this.radius) < 40) {
      let dir = p5.Vector.sub(pos, createVector(width/2, height/2));
      dir.normalize();
      if (this.type === 'attract') dir.mult(-3);  // hacia el centro
      else dir.mult(3);                           // hacia afuera
      if (obj.applyForce) obj.applyForce(dir);
    }
  }
}

// ---------------- FlowParticle ----------------
class FlowParticle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(1.5);
    this.acc = createVector(0, 0);
    this.prev = this.pos.copy();
  }
  applyForce(f) { this.acc.add(f); }
  update() {
    this.vel.add(this.acc);
    this.vel.limit(4);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }
  edges() {
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.y < 0) this.pos.y = height;
    if (this.pos.y > height) this.pos.y = 0;
  }
  displayCustom(col, size) {
    strokeWeight(size);
    stroke(col[0], col[1], col[2], 0.6);
    line(this.prev.x, this.prev.y, this.pos.x, this.pos.y);
    this.prev.set(this.pos);
  }
}

// ---------------- Boid ----------------
class Boid {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);
    this.maxForce = 0.08;
    this.maxSpeed = 3.0;
    this.perception = 50;
    this.cohesionFactor = 0.02;
    this.h = random(0, 360);
  }

  applyForce(f) { this.acc.add(f); }

  flock(boids) {
    let alignment = createVector();
    let cohesion = createVector();
    let separation = createVector();
    let total = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.pos, other.pos);
      if (other != this && d < this.perception) {
        alignment.add(other.vel);
        cohesion.add(other.pos);
        let diff = p5.Vector.sub(this.pos, other.pos);
        diff.div(d * d + 1);
        separation.add(diff);
        total++;
      }
    }
    if (total > 0) {
      alignment.div(total);
      alignment.setMag(this.maxSpeed);
      alignment.sub(this.vel);
      alignment.limit(this.maxForce);

      cohesion.div(total);
      cohesion.sub(this.pos);
      cohesion.setMag(this.maxSpeed);
      cohesion.sub(this.vel);
      cohesion.limit(this.maxForce);

      separation.div(total);
      separation.setMag(this.maxSpeed);
      separation.sub(this.vel);
      separation.limit(this.maxForce);

      alignment.mult(1.0);
      cohesion.mult(this.cohesionFactor);
      separation.mult(1.5);

      this.applyForce(alignment);
      this.applyForce(cohesion);
      this.applyForce(separation);
    }
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  edges() {
    if (this.pos.x < -10) this.pos.x = width + 10;
    if (this.pos.x > width + 10) this.pos.x = -10;
    if (this.pos.y < -10) this.pos.y = height + 10;
    if (this.pos.y > height + 10) this.pos.y = -10;
  }

  display(mid, high) {
    noStroke();
    fill((this.h + mid*60) % 360, 80, map(high, 0, 255, 40, 100), 0.9);
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.vel.heading());
    ellipse(0, 0, 6, 3);
    pop();
  }
}

// ---------------- Controls ----------------
function mouseWheel(event) {
  flowStrength = constrain(flowStrength + event.delta * -0.001, 0.1, 4);
}

function keyPressed() {
  if (key === ' ') {
    if (soundFile && !useMic) {
      if (!soundFile.isPlaying()) {
        soundFile.play();
        playing = true;
      } else {
        soundFile.pause();
        playing = false;
      }
    }
  }
  if (key === 'z') {
    waves.push(new Wave(mouseX, mouseY, 'attract'));
    invertColors = !invertColors; // 🔹 invertir colores
  }
  if (key === 'c') {
    waves.push(new Wave(mouseX, mouseY, 'repel'));
    currentPaletteIndex = (currentPaletteIndex + 1) % palettes.length; // 🔹 cambiar paleta
  }
  if (key === 'm') {
    if (!useMic) {
      mic = new p5.AudioIn();
      mic.start();
      fft.setInput(mic);
      amp.setInput(mic);
      useMic = true;
    } else {
      if (mic) mic.stop();
      useMic = false;
      if (soundFile) {
        fft.setInput(soundFile);
        amp.setInput(soundFile);
      }
    }
  }
  if (key >= '1' && key <= '4') {
    let p = int(key);
    if (p === 1) { particles.splice(0); for(let i=0;i<200;i++) particles.push(new FlowParticle(random(width), random(height))); }
    if (p === 2) { particles.splice(0); for(let i=0;i<400;i++) particles.push(new FlowParticle(random(width), random(height))); }
    if (p === 3) { for(let i=0;i<50;i++) boids.push(new Boid(random(width), random(height))); }
    if (p === 4) { boids.splice(0); for(let i=0;i<80;i++) boids.push(new Boid(random(width), random(height))); }
  }
}

