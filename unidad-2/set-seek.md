# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1


Cuando se suman dos vectores, se combinan sus componentes individuales: las componentes en X se suman entre sí, al igual que las componentes en Y (y Z si son vectores tridimensionales). El resultado es un nuevo vector que representa la dirección y magnitud combinadas de los dos vectores originales.

No funciona porque el operador "+" no permite la suma entre un vector y un escalar

### Actividad 2

#### Walker

``` js
let walker;

function setup() {
  createCanvas(400, 400);
  walker = new Walker();  // ahora usa p5.Vector
}

function draw() {
  walker.step();  // movimiento aleatorio
  walker.display();
}

class Walker {
  constructor() {
    this.position = createVector(width/2, height/2);
  }
  step() {
    let step = p5.Vector.random2D();
    step.mult(1);
    this.position.add(step);
  }
  display() {
    stroke(0);
    point(this.position.x, this.position.y);
  }
}
```

#### pelota que rebota (2d)

``` js
let walker;

function setup() {
  createCanvas(400, 400);
  walker = new Walker();  // ahora usa p5.Vector
}

function draw() {
  walker.step();  // movimiento aleatorio
  walker.display();
}

class Walker {
  constructor() {
    this.position = createVector(width/2, height/2);
  }
  step() {
    let step = p5.Vector.random2D();
    step.mult(1);
    this.position.add(step);
  }
  display() {
    stroke(0);
    point(this.position.x, this.position.y);
  }
}
```

### Actividad 3

Espero que el vector position, que inicialmente tiene los valores (6, 9), sea modificado dentro de la función playingVector() a (20, 30). Como p5.Vector es un objeto, los cambios deberían reflejarse fuera de la función también.

Tal como se esperaba, el primer console.log() imprime:
coords (6, 9)
Y después de llamar a playingVector(position), el segundo console.log() muestra:
coords (20, 30)
Esto confirma que el objeto fue modificado dentro de la función y que los cambios se reflejaron afuera.

En el código original, se está realizando un paso por referencia, ya que se pasa un objeto p5.Vector a la función playingVector(). Al modificar sus propiedades (x y y), el vector original también cambia.

Los vectores en p5.js (como cualquier objeto en JavaScript) se pasan por referencia.
Cuando una función modifica un objeto recibido como parámetro, esos cambios son visibles fuera de la función.

### Actividad 4
mag() calcula la magnitud (longitud) de un vector, es decir, la distancia del origen al punto definido por sus componentes.

### Actividad 5

El codigo lo hice en clase pero no se guardo
El método lerp() (de linear interpolation) se usa para encontrar un valor intermedio entre dos valores, basándose en un parámetro amt que va de 0 a 1.

En el contexto de vectores, p5.Vector.lerp(v1, v2, amt) devuelve un nuevo vector que está en algún punto entre v1 y v2

Guarda el estado del sistema de coordenadas con push().

Establece el color de trazo y relleno.

Traslada el sistema al punto de origen de la flecha (base).

Dibuja la línea principal del vector usando line(0, 0, vec.x, vec.y).

Rota el sistema para alinear la punta de la flecha con la dirección del vector (vec.heading()).

Mueve el origen hasta casi el final del vector para dibujar la cabeza de la flecha.

Dibuja un triángulo como cabeza de flecha con triangle(...).

Restaura el estado original con pop().

### Actividad 6 

significa que en cada fotograma actualizamos la ubicación moviéndola en la dirección e intensidad indicada por el vector velocidad. En cada paso, el objeto "se desplaza" a lo largo del vector velocidad. E


### Actividad 7

El experimento demuestra cómo un pequeño cambio en la aceleración puede producir un comportamiento completamente distinto.

Geométricamente, la aceleración cambia la dirección y magnitud del vector velocidad, lo que a su vez altera la posición.

El principio "acceleration → velocity → position" muestra cómo el control del movimiento puede originarse desde una lógica simple de fuerzas o comportamientos.

Motion 101 es un marco que permite construir sistemas complejos de movimiento a partir de reglas simples.

                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           


