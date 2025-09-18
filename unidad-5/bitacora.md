# Evidencias de la unidad 5

## Actividad 2: 
1. ¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?
2. Vas a modificar cada una de las simulaciones anteriores incluyen en cada una, al menos un concepto de las unidades anteriores, pero no repitas concepto, la idea es que repases al menos uno de cada unidad.
3. Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste (aunque es posible que la simulación ya lo haga, trata de identificarlo de nuevo y explicarlo con tus palabras).
4. Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
5. Incluye un enlace a tu código en el editor de p5.js.
6. Incluye el código fuente de cada una de las simulaciones.
7. Captura de pantallas de cada una de las simulaciones con las imágenes que más te gusten como resultado de la ejecución de cada una de las simulaciones.
   
### Ejemplo 4.2 
1. Se generan partículas nuevas en cada frame con el addParticle, normalmente es con un punto base o en referencia a la posicion del mouse, para desaparecerlas se usa un atributo en cada particula que se llama lifespan que decrece en cada frame, ya cuando llega a 0 se elimina del arreglo, la gestion de memora es con un arrayList que permite añadir o quitar patticulas dinamicamente, la memoria se libera cuando las referencias ya se borran del arreglo
2. Uso de random(), concepto de unidad 1, lo apliqué para que las particulas no salgan siempre del mismo color si no que aparezcan en colores aleatorios
3. Las partículas se crean dentro de un arreglo (particles[]) cada vez que se llama a addParticle(), se eliminan cuando su lifespan llega a 0, usando splice() para quitarlas del arreglo.
4. El concepto de random aporta diversidad visual en las particulas, como que siento que enriquece visualmente la simulacion y muestra la aleatoriedad controlada, la posicion la puse desde el mouse
5. [Enlace](https://editor.p5js.org/luciana.gp0531/sketches/hkkazZrK7)
6.
```js
let particles = [];

function setup() {
  createCanvas(600, 400);
}

function draw() {
  background(30);

  // crear partícula en posición del mouse
  particles.push(new Particle(createVector(mouseX, mouseY)));

  // actualizar y mostrar
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    p.update();
    p.show();
    if (p.isDead()) {
      particles.splice(i, 1);
    }
  }
}

class Particle {
  constructor(pos) {
    this.pos = pos.copy();
    this.vel = p5.Vector.random2D().mult(random(1, 3)); // dirección random
    this.acc = createVector(0, 0);
    this.lifespan = 255;
    this.c = color(random(255), random(255), random(255)); // color random
  }
  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.lifespan -= 4;
  }
  show() {
    noStroke();
    fill(this.c.levels[0], this.c.levels[1], this.c.levels[2], this.lifespan);
    ellipse(this.pos.x, this.pos.y, 12);
  }
  isDead() {
    return this.lifespan < 0;
  }
}
```
7. <img width="486" height="491" alt="image" src="https://github.com/user-attachments/assets/a9fdbf52-76cb-4e25-94cf-c687babf5205" />


### Ejemplo 4.4
1. En este caso no hay solo particulas sino varios sistemas de ellas, entonces se crean las particulas desde cada sistema, por lo cual cada sistema gestiona la vida de las mismas particulas, y ya cuanto todas se eliminan el sistema queda vacio, y para gestionar la memoria se usan listas anidadas.
2. Ruido Perlin, unidad 1, lo usé para que la posicion del origen del sistema se desplace suavemente por el canvas en vez de quedarse fijo.
3. Cada clic crea un nuevo ParticleSystem, cada sistema agrega partículas en cada frame y elimina las que mueren (lifespan < 0).
4. El Perlin Noise genera un movimiento suave y natural en la posición del sistema. Apliqué noise(offsetX) y noise(offsetY) para desplazar la posición del origin. Esto lo elegí porque da una estética más orgánica y menos caótica que el random.
5. [Enlace](https://editor.p5js.org/luciana.gp0531/sketches/fBYYDPltD)
6. 
```js
let systems = [];

function setup() {
  createCanvas(600, 400);
}

function draw() {
  background(20);

  for (let i = systems.length - 1; i >= 0; i--) {
    systems[i].update();
    systems[i].show();
    if (systems[i].isEmpty()) {
      systems.splice(i, 1);
    }
  }
}

function mousePressed() {
  systems.push(new ParticleSystem(createVector(mouseX, mouseY)));
}

class Particle {
  constructor(pos, col) {
    this.pos = pos.copy();
    this.vel = p5.Vector.random2D().mult(random(1, 2));
    this.acc = createVector(0, 0);
    this.lifespan = 255;
    this.col = col;
  }
  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.lifespan -= 3;
  }
  show() {
    noStroke();
    fill(red(this.col), green(this.col), blue(this.col), this.lifespan);
    ellipse(this.pos.x, this.pos.y, 10);
  }
  isDead() {
    return this.lifespan < 0;
  }
}

class ParticleSystem {
  constructor(origin) {
    this.origin = origin.copy();
    this.particles = [];
    this.col = color(random(255), random(255), random(255));
    this.noiseOffsetX = random(1000); // para Perlin Noise
    this.noiseOffsetY = random(2000);
  }
  addParticle() {
    this.particles.push(new Particle(this.origin.copy(), this.col));
  }
  update() {
    // mover con Perlin Noise
    let nX = noise(this.noiseOffsetX) * width;
    let nY = noise(this.noiseOffsetY) * height;
    this.origin.set(nX, nY);

    this.noiseOffsetX += 0.01;
    this.noiseOffsetY += 0.01;

    this.addParticle();
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.update();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
  show() {
    for (let p of this.particles) {
      p.show();
    }
  }
  isEmpty() {
    return this.particles.length === 0;
  }
}
```

7. <img width="487" height="496" alt="image" src="https://github.com/user-attachments/assets/14f2d764-2194-492d-b68b-2e45ce074e1e" />


### Ejemplo 4.5
1. Hay una clase base Particle y subclases, que hacen que el sistema genere distintos tipos de particulas, todas heredan el metodo isDead y update, que gestionan lifespan, gracias al polimorfismo todas las particulas estan en el mismo arreglo, cuando se borran se liberan referencias

### Ejemplo 4.6
1. La gestion de este es similar a los ejemplos anteriores con el addParticle, tambien mueren las particulas por el lifespan, la memoria funciona igual PERO se manejan fuerzas como gravedad mediante sumatoria de PVector sobre cada partícula.

### Ejemplo 4.7
1. Tambien funciona igual la creacion y desaparicion pero la memoria es un repulsor que se representa como un objeto que genera una fuerza de repulsion en funcion de la distancia a las particulas.
