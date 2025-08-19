# Unidad 3

## 🔎 Fase: Set + Seek

Inventa tres obras generativas interactivas, uno para cada una de las siguientes fuerzas:

- Fricción.
    
**Explica cómo modelaste cada fuerza.**

La fricción se opone al movimiento y hace que la bola se desacelere progresivamente:

μ = coeficiente de fricción (depende de la franja).

𝑣 = velocidad de la bola.

Cada frame, la velocidad de la bola se reduce en dirección contraria a su movimiento.

**Conceptualmente cómo se relaciona la fuerza con la obra generativa.**
En el juego del lanzador, las franjas con distintos valores de 
𝜇
μ representan superficies diferentes: algunas frenan mucho, otras poco. 


**CÓDIGO**

```JS
let ball;
let hole;
let isDragging = false;
let dragStart;
let muLow = 0.01;  // Fricción baja
let muHigh = 0.09; // Fricción alta
let zones = [];

function setup() {
  createCanvas(800, 400);
  
  // Bola inicial en esquina izquierda
  ball = {
    pos: createVector(100, height / 2),
    vel: createVector(0, 0),
    radius: 15,
    moving: false
  };
  
  // Meta/hueco
  hole = {
    pos: createVector(width - 80, height / 2),
    radius: 25
  };
  
  // Zonas con fricción distinta
  zones.push({x: 200, w: 100, mu: muHigh, col: color(255, 100, 100)}); // roja, alta fricción
  zones.push({x: 400, w: 100, mu: muLow,  col: color(100, 255, 100)}); // verde, baja fricción
  zones.push({x: 600, w: 100, mu: muHigh, col: color(255, 100, 100)});
}

function draw() {
  background(220);

  // Dibujar zonas de fricción
  noStroke();
  for (let z of zones) {
    fill(z.col);
    rect(z.x, 0, z.w, height);
  }

  // Dibujar meta
  fill(50, 50, 200);
  ellipse(hole.pos.x, hole.pos.y, hole.radius * 2);

  // Dibujar bola
  fill(255);
  ellipse(ball.pos.x, ball.pos.y, ball.radius * 2);

  // Físicas de la bola
  if (ball.moving) {
    let mu = getFrictionAt(ball.pos.x);
    let friction = p5.Vector.mult(ball.vel, -mu);
    ball.vel.add(friction);
    ball.pos.add(ball.vel);
    
    // Si la velocidad es muy pequeña, se detiene
    if (ball.vel.mag() < 0.1) {
      ball.vel.set(0, 0);
      ball.moving = false;
    }
  }

  // Línea de arrastre para lanzamiento
  if (isDragging) {
    stroke(0);
    line(ball.pos.x, ball.pos.y, mouseX, mouseY);
  }

  // Verificar victoria
  if (dist(ball.pos.x, ball.pos.y, hole.pos.x, hole.pos.y) < hole.radius) {
    fill(0, 200, 0);
    textSize(32);
    textAlign(CENTER);
    text("¡Ganaste!", width / 2, 50);
    ball.moving = false;
    ball.vel.set(0, 0);
  }
}

function mousePressed() {
  // Comenzar arrastre si clic en la bola
  if (dist(mouseX, mouseY, ball.pos.x, ball.pos.y) < ball.radius) {
    isDragging = true;
    dragStart = createVector(mouseX, mouseY);
    ball.moving = false;
    ball.vel.set(0, 0);
  }
}

function mouseReleased() {
  if (isDragging) {
    let dragEnd = createVector(mouseX, mouseY);
    let launchVec = p5.Vector.sub(dragStart, dragEnd).mult(0.1);
    ball.vel = launchVec;
    ball.moving = true;
    isDragging = false;
  }
}

function getFrictionAt(x) {
  for (let z of zones) {
    if (x >= z.x && x <= z.x + z.w) {
      return z.mu;
    }
  }
  return 0.01; // fricción por defecto
}

```

https://editor.p5js.org/luciana.gp0531/sketches/0aHPx5-ww

<img width="919" height="498" alt="image" src="https://github.com/user-attachments/assets/8834149b-368a-47c1-9f2c-254a9e2386c9" />

Ahi perdi por falta de impulso

- Resistencia del aire y de fluidos.
    
**Explica cómo modelaste cada fuerza.**
La resistencia de un fluido depende del cuadrado de la velocidad y es proporcional a la dirección opuesta:
𝑐 = coeficiente de resistencia (qué tan denso es el medio).
𝑣⃗ = velocidad.
𝑣 (^)= dirección de la velocidad.

Esto significa que mientras más rápido se mueve un pez, más rápido se frena.

**Conceptualmente cómo se relaciona la fuerza con la obra generativa.**
Cuando el usuario hace click sobre un pez, lo empuja, pero el agua lo frena rápidamente hasta volver a estar quieto.

**CÓDIGO**
```js
let fishes = [];
let numFishes = 6;
let dragCoeff = 0.02; // Coeficiente de resistencia

function setup() {
  createCanvas(800, 400);
  for (let i = 0; i < numFishes; i++) {
    fishes.push(new Fish(random(width), random(height)));
  }
}

function draw() {
  background(150, 200, 255); // Agua

  for (let f of fishes) {
    f.update();
    f.show();
  }
}

function mousePressed() {
  for (let f of fishes) {
    let d = dist(mouseX, mouseY, f.pos.x, f.pos.y);
    if (d < f.size / 2 + 10) {
      // Dirección opuesta al toque
      let dir = p5.Vector.sub(f.pos, createVector(mouseX, mouseY));
      dir.setMag(random(3, 6)); // Fuerza inicial
      f.vel = dir;
    }
  }
}

class Fish {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.size = 40;
  }

  update() {
    // Resistencia del fluido: -c * |v|^2 * dirección
    if (this.vel.mag() > 0) {
      let drag = this.vel.copy();
      drag.normalize();
      drag.mult(-1 * dragCoeff * this.vel.magSq());
      this.vel.add(drag);
    }

    // Movimiento
    this.pos.add(this.vel);

    // Limites de la pecera
    if (this.pos.x < 0) { this.pos.x = 0; this.vel.x *= -0.5; }
    if (this.pos.x > width) { this.pos.x = width; this.vel.x *= -0.5; }
    if (this.pos.y < 0) { this.pos.y = 0; this.vel.y *= -0.5; }
    if (this.pos.y > height) { this.pos.y = height; this.vel.y *= -0.5; }
  }

  show() {
    fill(255, 150, 50);
    ellipse(this.pos.x, this.pos.y, this.size, this.size / 2);
    // Cola
    triangle(
      this.pos.x - this.size / 2, this.pos.y,
      this.pos.x - this.size, this.pos.y - this.size / 4,
      this.pos.x - this.size, this.pos.y + this.size / 4
    );
    // Ojo
    fill(0);
    ellipse(this.pos.x + this.size / 4, this.pos.y - this.size / 8, 5, 5);
  }
}
```
https://editor.p5js.org/luciana.gp0531/sketches/FarsstfGG


<img width="911" height="475" alt="image" src="https://github.com/user-attachments/assets/bb433c68-196e-42fa-8403-6d625bdb0b5f" />


- Atracción gravitacional.
  
Explica cómo modelaste cada fuerza.

Conceptualmente cómo se relaciona la fuerza con la obra generativa.
Copia el enlace a tu ejemplo en p5.js.
Copia el código.
Captura una imagen representativa de tu ejemplo.

