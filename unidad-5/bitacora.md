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
2. Motion 101, unidad 2, lo uso para que las particulas tengan un movimiento más orgánico y dinámico
3. Se crean nuevas partículas en cada frame desde el origen, cada partícula aplica aceleración → actualiza velocidad → actualiza posición (Motion 101). La desaparición se maneja igual
4. El motion 101 lo use aplicando this.acc.add(p5.Vector.random2D().mult(0.05));en el update, qie hace que la particula nunca se mueva de forma recta completamente si no que va cambiando suavemente de direccion, lo elegí porque siento que encaja muy bien con herencia y polimorfismos.
5. [Enlace](https://editor.p5js.org/luciana.gp0531/sketches/8GZqCCMVg)
6. 
```js
let systems = [];

function setup() {
  createCanvas(600, 400);
}

function draw() {
  background(30);

  for (let ps of systems) {
    ps.addParticle();
    ps.run();
  }
}

function mousePressed() {
  systems.push(new ParticleSystem(createVector(mouseX, mouseY)));
}

class Particle {
  constructor(position) {
    this.pos = position.copy();
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);
    this.lifespan = 255;
  }

  applyMotion101() {
    // Motion 101: acelerar un poquito en dirección aleatoria
    let jitter = p5.Vector.random2D().mult(0.05);
    this.acc.add(jitter);
  }

  update() {
    this.applyMotion101(); 
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 2;
  }

  display() {
    stroke(200, this.lifespan);
    fill(127, this.lifespan);
    ellipse(this.pos.x, this.pos.y, 12);
  }

  isDead() {
    return this.lifespan < 0;
  }
}

class CrazyParticle extends Particle {
  display() {
    stroke(255, 0, 0, this.lifespan);
    fill(255, 100, 100, this.lifespan);
    rect(this.pos.x, this.pos.y, 16, 16);
  }

  update() {
    super.update();
    // comportamiento extra: rota un poco con seno
    this.pos.x += sin(frameCount * 0.1) * 0.5;
  }
}

class ParticleSystem {
  constructor(position) {
    this.origin = position.copy();
    this.particles = [];
  }

  addParticle() {
    if (random(1) < 0.5) {
      this.particles.push(new Particle(this.origin));
    } else {
      this.particles.push(new CrazyParticle(this.origin));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.update();
      p.display();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
```
7. <img width="474" height="493" alt="image" src="https://github.com/user-attachments/assets/fdd56bd8-5699-4760-802e-ce14950eafe6" />


### Ejemplo 4.6
1. La gestion de este es similar a los ejemplos anteriores con el addParticle, tambien mueren las particulas por el lifespan, la memoria funciona igual PERO se manejan fuerzas como gravedad mediante sumatoria de PVector sobre cada partícula.
2. Vectores y fuerza de viento, unidad 2, añadí que cada partícula esté influenciada por una fuerza diferente
3. Partículas se crean en cada frame con una posición inicial en el origen del sistema. Reciben un applyForce() que las hace caer distinto.
4. El concepto de vectores se aplicó para representar la fuerza que se suma a la aceleración de cada partícula, lo elegí porque ayuda a comprender cómo diferentes condiciones afectan particulas en un mismo espacio. La fuerza del viento es con las flechas de los lados
5. [Enlace](https://editor.p5js.org/luciana.gp0531/sketches/u6GrdDsz6)
6. 
```js
let particles = [];
let gravity;

function setup() {
  createCanvas(600, 400);
  gravity = createVector(0, 0.2);
}

function draw() {
  background(30);

  particles.push(new Particle(createVector(mouseX, mouseY)));

  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];

    // siempre gravedad
    p.applyForce(gravity);

    // viento con teclas
    if (keyIsDown(LEFT_ARROW)) {
      p.applyForce(createVector(-0.2, 0));
    }
    if (keyIsDown(RIGHT_ARROW)) {
      p.applyForce(createVector(0.2, 0));
    }

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
    this.vel = p5.Vector.random2D().mult(random(1, 3));
    this.acc = createVector(0, 0);
    this.lifespan = 255;
  }
  applyForce(force) {
    this.acc.add(force);
  }
  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 3;
  }
  show() {
    noStroke();
    fill(100, 150, 255, this.lifespan);
    ellipse(this.pos.x, this.pos.y, 12);
  }
  isDead() {
    return this.lifespan < 0;
  }
}
```
7. <img width="461" height="495" alt="image" src="https://github.com/user-attachments/assets/4fe1e0ad-a96a-4353-ab87-c0e05ffed4a2" />

### Ejemplo 4.7
1. Tambien funciona igual la creacion y desaparicion pero la memoria es un repulsor que se representa como un objeto que genera una fuerza de repulsion en funcion de la distancia a las particulas.
2. Usé la posicion del mouse para mover el Repeller. Así, el usuario controla directamente el campo de repulsión
3. Las partículas siguen el ciclo normal de creación en el origen y eliminación con lifespan. La memoria se gestiona quitando las partículas muertas del arreglo.
4. El concepto de interacción se aplicó haciendo que Repeller reciba como posición mouseX, mouseY. Lo elegí porque introduce control directo del usuario en el sistema, haciéndolo más inmersivo.
5. [Enlace](https://editor.p5js.org/luciana.gp0531/sketches/CfCDlLTWs)
6. 
```js
let particles = [];
let repeller;

function setup() {
  createCanvas(600, 400);
  repeller = new Repeller(width/2, height/2);
}

function draw() {
  background(10);

  particles.push(new Particle(createVector(width/2, 50)));

  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];

    let force = repeller.repel(p);
    p.applyForce(force);

    p.update();
    p.show();

    if (p.isDead()) {
      particles.splice(i, 1);
    }
  }

  repeller.pos.set(mouseX, mouseY);
  repeller.show(particles);
}

class Particle {
  constructor(pos) {
    this.pos = pos.copy();
    this.vel = p5.Vector.random2D().mult(random(1, 2));
    this.acc = createVector(0, 0);
    this.lifespan = 255;
  }
  applyForce(force) {
    this.acc.add(force);
  }
  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 3;
  }
  show() {
    noStroke();
    fill(255, this.lifespan);
    ellipse(this.pos.x, this.pos.y, 8);
  }
  isDead() {
    return this.lifespan < 0;
  }
}

class Repeller {
  constructor(x, y) {
    this.pos = createVector(x, y);
  }
  repel(p) {
    let dir = p.pos.copy().sub(this.pos);
    let d = constrain(dir.mag(), 5, 100);
    let strength = -100 / (d * d);
    dir.normalize();
    dir.mult(strength);
    return dir;
  }
  show(particles) {
    // el tamaño depende de cuántas partículas están cerca
    let nearCount = particles.filter(p => dist(p.pos.x, p.pos.y, this.pos.x, this.pos.y) < 80).length;
    let size = map(nearCount, 0, 50, 20, 80);
    fill(255, 0, 0, 150);
    noStroke();
    ellipse(this.pos.x, this.pos.y, size);
  }
}
```
7. <img width="376" height="183" alt="image" src="https://github.com/user-attachments/assets/04a10760-a9c6-4a22-872b-1d301d207f42" />


