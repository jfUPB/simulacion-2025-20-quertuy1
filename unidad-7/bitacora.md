# Evidencias de la unidad 7


## ACTIVIDAD 1
1. "Moon"

En este diseño, la segunda “O” de la palabra “MOON” se eleva ligeramente, imitando la forma en que la luna se mueve en el cielo.
Conexión: El movimiento visual reproduce la idea del ciclo lunar, La letra se convierte en simbolo y narrativa al mismo tiempo.

2. "Elevator"

La palabra “ELEVATOR" esta aparece con la letra "V" y la letra "A" en color negro mientras que el resto en un color gris, para resaltar como si la "V" significara que se esta pidiendo un ascensor de bajada y con la letra "A" de subida.
Conexión: El concepto del ascensor se expresa literalmente a través de ademas del sonido al que aparece cuando se marca una flecha o direccion al pedir un asecensor.

3. "Mirror"

En este caso, la segunda mitad de la palabra “MIRROR” está reflejada horizontalmente.
Conexión: La palabra se convierte en el objeto que representa; la inversion de las letras comunica la idea de reflexión.

4. "Gravity"

Las letras de “GRAVITY” parecen caerse apenas empieza la animacion, dando la sensación de peso.
Conexión: El diseño usa la dirección y el equilibrio visual para evocar la fuerza de la gravedad de manera inmediata.


## ACTIVIDAD 2

1. "Silencio"
Las letras se van desvaneciendo gradualmente hacia el final, como si el sonido se apagara.

2. "Ruptura"
La palabra se parte en dos mitades, desplazadas y con una línea simulando una grieta entre ellas.

3. "Caída"
Las letras finales van descendiendo progresivamente, simulando que están cayendo.

```js
// --- Experimento 1: Gravedad básica con Matter.js y p5.js ---

// Variables de Matter.js
let Engine = Matter.Engine,
    World = Matter.World,
    Bodies = Matter.Bodies;

let engine;
let world;
let boxes = [];
let ground;

function setup() {
  createCanvas(800, 600);
  engine = Engine.create(); // Crear motor físico
  world = engine.world;

  // Crear el suelo
  ground = Bodies.rectangle(400, height - 20, 810, 40, { isStatic: true });
  World.add(world, ground);
}

function mousePressed() {
  // Crear nuevas cajas al hacer clic
  let box = Bodies.rectangle(mouseX, mouseY, 40, 40);
  World.add(world, box);
  boxes.push(box);
}

function draw() {
  background(51);
  Engine.update(engine);

  // Dibujar las cajas
  fill(127);
  stroke(200);
  for (let box of boxes) {
    push();
    translate(box.position.x, box.position.y);
    rotate(box.angle);
    rectMode(CENTER);
    rect(0, 0, 40, 40);
    pop();
  }

  // Dibujar el suelo
  fill(100, 255, 100);
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, 810, 40);
}
```
https://editor.p5js.org/quertuy1/full/6_XNAscTG

![cajas cayendo en una superficie](https://cdn.discordapp.com/attachments/716834985306488894/1426181898446835732/image.png?ex=68ea4ae5&is=68e8f965&hm=423dae78845a966a3d4a9a7ac5d5d4e57ce85b947eca07700147d8e9ad1efe70&)

Este experimento demuestra como los cuerpos rigidos y solidos reaccionan a la gravedad dentro del motor físico de Matter.js. Cada caja creada con el mouse cae y colisiona con el suelo estatico.



```js
// --- Experimento 2: Restricciones y movimiento con el mouse ---

let Engine = Matter.Engine,
    World = Matter.World,
    Bodies = Matter.Bodies,
    Constraint = Matter.Constraint,
    Mouse = Matter.Mouse,
    MouseConstraint = Matter.MouseConstraint;

let engine;
let world;
let pendulum;
let rope;
let mConstraint;

function setup() {
  createCanvas(800, 600);
  engine = Engine.create();
  world = engine.world;

  // Crear un péndulo
  pendulum = Bodies.circle(400, 300, 30, { restitution: 0.9 });
  let fixedPoint = { x: 400, y: 100 };
  
  // Crear cuerda (constraint)
  rope = Constraint.create({
    pointA: fixedPoint,
    bodyB: pendulum,
    stiffness: 0.05,
    length: 200
  });

  World.add(world, [pendulum, rope]);

  // Control con el mouse
  let canvasMouse = Mouse.create(canvas.elt);
  let options = {
    mouse: canvasMouse
  };
  mConstraint = MouseConstraint.create(engine, options);
  World.add(world, mConstraint);
}

function draw() {
  background(30);
  Engine.update(engine);

  // Dibujo del péndulo
  stroke(255);
  strokeWeight(2);
  line(rope.pointA.x, rope.pointA.y, pendulum.position.x, pendulum.position.y);
  fill(255, 0, 0);
  ellipse(pendulum.position.x, pendulum.position.y, 60);

  // Texto guía
  noStroke();
  fill(255);
  textAlign(CENTER);
  text("Arrastra el péndulo con el mouse", width / 2, 50);
}
```
https://editor.p5js.org/quertuy1/full/0CG3h0QQw

![cajas cayendo en una superficie](https://cdn.discordapp.com/attachments/716834985306488894/1426184216466686002/image.png?ex=68ea4d0e&is=68e8fb8e&hm=11c4d46d7fff3525c02ed39fa6f84ccc98046a97113cea704bee77ca80cc0c86&)

Aquí se demuestra como las restricciones (Constraint) pueden unir cuerpos, simulando una cuerda o articulación. El uso de MouseConstraint permite interactuar con el objeto arrastrándolo en tiempo real.


Engine: el motor de fisica que actualiza las posiciones, velocidades y colisiones de todos los cuerpos.

World: el “mundo” que contiene todos los objetos fisicos del entorno.

Bodies: los cuerpos físicos que interactúan mediante colisiones y fuerzas.

Constraint: crea conexiones entre cuerpos, simulando cuerdas, resortes o uniones.

MouseConstraint: permite interactuar con los cuerpos usando el mouse, como arrastrarlos o empujarlos.


### Dificultades 

1. Inicialmente hubo confusión al cargar correctamente las librerias ya que Matter.js debe incluirse antes del script principal que esta en el index.html 

2. Otra dificultad fue sincronizar el sistema de coordenadas entre p5.js y Matter.js, ya que Matter.js trabaja con valores fisicos continuos y p5 con pixeles.

3. Se agregaron variables de stiffness y restitution  para obtener un comportamiento mas natural y suavizado en los cuerpos.



## Actividad 3

### Palabra "ROMPER"

**Concepto:** las letras están construidas por pequeños bloques unidos por constraints. Un impacto rompe las uniones y la palabra se descompone en fragmentos.

**Controles**
- Click: aplica impacto local.
- `b`: lanza una bola que choca contra la palabra.
- `r`: reinicia la escena.

```js
// ROMPER — tipografía semántica con Matter.js + p5.js
// Interacciones:
//  - click: impacto local (implode/explode) -> rompe constraints
//  - b: lanzar bola que golpea las letras
//  - r: reiniciar

// Matter aliases
let Engine = Matter.Engine;
let World = Matter.World;
let Bodies = Matter.Bodies;
let Body = Matter.Body;
let Composite = Matter.Composite;
let Constraint = Matter.Constraint;
let Mouse = Matter.Mouse;
let MouseConstraint = Matter.MouseConstraint;

let engine, world;
let ground;
let letters = []; // cada letter = {blocks:[], constraints:[]}
let blockSize = 20; // tamaño de cada pieza
let colsToTry = 40; // para posicionamiento en pantalla
let breakThreshold = 18; // distancia (px) a partir de la cual se rompe un constraint
let balls = [];

function setup() {
  createCanvas(900, 400);
  engine = Engine.create();
  world = engine.world;
  world.gravity.y = 1.0;

  // suelo
  ground = Bodies.rectangle(width/2, height + 30, width * 2, 60, { isStatic: true });
  World.add(world, ground);

  buildWordROMPER();

  // mouse constraint para arrastrar (opcional)
  let m = Mouse.create(canvas.elt);
  let mc = MouseConstraint.create(engine, { mouse: m, constraint: { stiffness: 0.2 }});
  World.add(world, mc);
}

function draw() {
  background(15);

  Engine.update(engine);

  // dibujar suelo
  noStroke();
  fill(40);
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, width*2, 60);

  // Update: chequear ruptura de constraints
  for (let L of letters) {
    // copiar lista para iterar removibles
    for (let i = L.constraints.length - 1; i >= 0; i--) {
      let c = L.constraints[i];
      if (!c) continue;
      // calcular distancia actual entre bodies
      let a = c.bodyA.position;
      let b = c.bodyB.position;
      let d = dist(a.x, a.y, b.x, b.y);
      if (d > breakThreshold && !c.broken) {
        // romper la constraint
        Composite.remove(world, c);
        c.broken = true;
        // opcional: remueve de array
        L.constraints.splice(i, 1);
      }
    }
  }

  // dibujar bloques
  for (let L of letters) {
    for (let b of L.blocks) {
      push();
      translate(b.position.x, b.position.y);
      rotate(b.angle);
      rectMode(CENTER);
      noStroke();
      fill(200, 120, 80);
      rect(0, 0, blockSize - 1, blockSize - 1);
      pop();
    }
  }

  // dibujar constraints (solo los que quedan)
  stroke(120, 200, 255, 120);
  strokeWeight(2);
  for (let L of letters) {
    for (let c of L.constraints) {
      if (!c || !c.bodyA || !c.bodyB) continue;
      line(c.bodyA.position.x, c.bodyA.position.y, c.bodyB.position.x, c.bodyB.position.y);
    }
  }

  // dibujar bolas
  for (let i = balls.length -1; i >=0; i--) {
    let ball = balls[i];
    push();
    fill(255, 100, 100);
    noStroke();
    ellipse(ball.position.x, ball.position.y, ball.circleRadius*2);
    pop();
    // remove offscreen
    if (ball.position.y > height + 200) {
      Composite.remove(world, ball);
      balls.splice(i,1);
    }
  }

  // instrucciones
  noStroke();
  fill(200);
  textSize(12);
  text("Click = impacto local / b = lanzar bola / r = reiniciar", 10, 18);
}

// Construir palabra ROMPER con rejillas por letra
function buildWordROMPER() {
  // limpiar mundo previo
  Composite.clear(world, false);
  World.add(world, ground); // re-add ground

  letters = [];
  let word = "ROMPER";
  // starting x
  let startX = 60;
  let y = 120;

  // definición simple de "plantilla" para letras en una rejilla 5x7
  // Cada letra aquí es una matriz de 0/1 para formar la letra (ajusta según necesites)
  let templates = {
    R: [
      "1110",
      "1010",
      "1110",
      "1100",
      "1010",
      "10010"
    ],
    O: [
      "0110",
      "1001",
      "1001",
      "1001",
      "1001",
      "0110"
    ],
    M: [
      "10001",
      "11011",
      "10101",
      "10001",
      "10001",
      "10001"
    ],
    P: [
      "1110",
      "1010",
      "1110",
      "1000",
      "1000",
      "1000"
    ],
    E: [
      "1111",
      "1000",
      "1110",
      "1000",
      "1000",
      "1111"
    ],
    // Ajuste rápido para la R final (usar mismo R)
  };

  // Como plantilla simple es difícil escribir matrices inline; en vez de eso, voy a construir cada letra
  // con rectángulos usando formas aproximadas: bar vertical, barras horizontales, diagonales donde convenga.
  // Para simplicidad crearé formas compuestas por arrays de rects.

  let spacing = 10;
  let x = startX;

  // función helper para crear letra approximada con bloques
  function createLetterBlocks(letter, xOffset) {
    let blocks = [];
    let constraints = [];
    // según letter crear posiciones relativas (esto es manual/simple)
    let pattern = [];
    if (letter === 'R') {
      // bloque vertical izquierdo y partes horizontales + diagonal
      for (let ry = 0; ry < 6; ry++) {
        pattern.push([0, ry]);
      }
      // parte superior horizontal (x=1..2, y=0,1)
      pattern.push([1,0]); pattern.push([2,0]);
      pattern.push([1,1]); pattern.push([2,1]);
      // medio
      pattern.push([1,2]); pattern.push([2,2]);
      // diagonal lower
      pattern.push([1,3]); pattern.push([2,4]);
    } else if (letter === 'O') {
      // marco rectangular 4x5 hollow
      for (let rx=0; rx<4; rx++) {
        pattern.push([rx,0]); pattern.push([rx,5]);
      }
      for (let ry=1; ry<5; ry++) {
        pattern.push([0,ry]); pattern.push([3,ry]);
      }
    } else if (letter === 'M') {
      // two verticals and center Vs
      for (let ry=0; ry<6; ry++) {
        pattern.push([0,ry]); pattern.push([4,ry]);
      }
      pattern.push([1,1]); pattern.push([2,2]); pattern.push([3,1]);
    } else if (letter === 'P') {
      for (let ry=0; ry<6; ry++) pattern.push([0,ry]);
      pattern.push([1,0]); pattern.push([2,0]); pattern.push([1,1]); pattern.push([2,1]); pattern.push([1,2]); pattern.push([2,2]);
    } else if (letter === 'E') {
      for (let ry=0; ry<6; ry++) pattern.push([0,ry]);
      pattern.push([1,0]); pattern.push([2,0]); pattern.push([1,2]); pattern.push([2,2]); pattern.push([1,5]); pattern.push([2,5]);
    } else {
      // fallback: single block
      pattern.push([0,0]);
    }

    // crear cuerpos
    for (let p of pattern) {
      let bx = xOffset + p[0] * (blockSize + 1);
      let by = y + p[1] * (blockSize + 1);
      let b = Bodies.rectangle(bx, by, blockSize, blockSize, {
        friction: 0.1,
        restitution: 0.2,
        density: 0.002
      });
      World.add(world, b);
      blocks.push(b);
    }

    // crear constraints entre bloques cercanos (adyacentes)
    for (let i = 0; i < blocks.length; i++) {
      for (let j = i+1; j < blocks.length; j++) {
        let a = blocks[i], b = blocks[j];
        let dx = abs(a.position.x - b.position.x);
        let dy = abs(a.position.y - b.position.y);
        // si están pegados en la rejilla (adyacentes)
        if ((dx <= blockSize + 2 && dy <= 2) || (dy <= blockSize + 2 && dx <= 2) || (dx <= blockSize+2 && dy <= blockSize+2 && (dx+dy)<= (blockSize+4))) {
          let c = Constraint.create({
            bodyA: a,
            bodyB: b,
            length: dist(a.position.x, a.position.y, b.position.x, b.position.y),
            stiffness: 0.25,
            damping: 0.1
          });
          World.add(world, c);
          constraints.push(c);
        }
      }
    }

    return {blocks: blocks, constraints: constraints};
  }

  // crea letras en fila
  let gap = 40;
  let lettersOrder = ['R','O','M','P','E','R'];
  let xpos = 60;
  for (let L of lettersOrder) {
    let obj = createLetterBlocks(L, xpos);
    letters.push(obj);
    // shift xpos by approximate width
    xpos += (5 * (blockSize + 1)) + gap;
  }
}

// interacción: click = impulso local que ayuda a romper
function mousePressed() {
  // aplicar impulso radial a cuerpos cercanos
  let impactPos = {x: mouseX, y: mouseY};
  for (let L of letters) {
    for (let b of L.blocks) {
      let d = dist(impactPos.x, impactPos.y, b.position.x, b.position.y);
      if (d < 120) {
        let dir = Matter.Vector.sub(b.position, impactPos);
        Matter.Body.applyForce(b, b.position, { x: dir.x * 0.0008, y: dir.y * 0.0008 });
      }
    }
  }
}

// lanzar bola con 'b'
function keyPressed() {
  if (key === 'b' || key === 'B') {
    let ball = Bodies.circle(50, 50, 24, { restitution: 0.6, density: 0.01 });
    Body.setVelocity(ball, { x: 15, y: -2 });
    World.add(world, ball);
    balls.push(ball);
  }
  if (key === 'r' || key === 'R') {
    // reiniciar
    // limpiar mundo (remueve todo excepto ground)
    World.clear(world, false);
    // re-add engine/world and ground
    ground = Bodies.rectangle(width/2, height + 30, width * 2, 60, { isStatic: true });
    World.add(world, ground);
    letters = [];
    buildWordROMPER();
  }
}

```
https://editor.p5js.org/quertuy1/full/fBhXuvruz
![ROMPER](https://cdn.discordapp.com/attachments/716834985306488894/1426187634992218293/image.png?ex=68ea503d&is=68e8febd&hm=d96a7c22e336bc3c5eda39cd3c56562568772cb84df38bb4d93812a310089a4c)

# AUTOEVALUACION 
# 5.0
