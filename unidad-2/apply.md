# Unidad 2


## 🛠 Fase: Apply

Describe el concepto de tu obra generativa.

Es una nube de particulas que se mueven segun un campo de viento, la interaccion es con el click, artrayendo p repelando las particulas del mouse.

¿Cómo piensas aplicar el marco MOTION 101 y por qué?

Voy a aplicar el MOTION101 literalmente, sin cambiarlo, para mantener el sistema estable y modular, y me ayuda a añadir las fuerzas nuevas sin dañar la integracion.

¿Qué algoritmo de aceleración vas a utilizar? ¿Por qué?

Voy a usar un proceso Ornstein–Uhlenbeck (O-U) para la aceleración de cada partícula, tiende a volver gradualmente a una media (memoria/inercia) en lugar de ser ruido blanco. Combino eso con un campo vectorial Perlin y una fuerza de atracción al mouse.

El código de la aplicación.
 ```Js
let particles = [];
let flowScale = 0.01;
let showField = false;

let params = {
  k_field: 0.6,
  k_mouse: 1.2,
  k_OU: 0.9,
  damping: 0.02,
  OU_theta: 0.05,
  OU_mu: 0,
  OU_sigma_base: 0.6
};

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100, 100);
  for (let i = 0; i < 200; i++) {
    particles.push(new Particle(random(width), random(height)));
  }
}

function draw() {
  background(220, 20, 6, 12);
  if (showField) drawFlowField();

  for (let p of particles) {
    let field = getFlowField(p.pos.x, p.pos.y).mult(params.k_field);

    let mouseF = createVector(0, 0);
    let m = createVector(mouseX, mouseY);
    let toMouse = p5.Vector.sub(m, p.pos);
    let d = toMouse.mag();
    if (d < 220) {
      let dir = toMouse.copy().normalize();
      let strength = map(d, 0, 220, 1.8, 0);
      if (mouseIsPressed) strength *= -1; // repeler si hay click
      mouseF = dir.mult(params.k_mouse * strength);
    }

    p.updateOU(params.OU_theta, params.OU_mu, params.OU_sigma_base);
    let OUvec = p.OUvec.copy().mult(params.k_OU);

    p.acc.set(0, 0);
    p.acc.add(field);
    p.acc.add(mouseF);
    p.acc.add(OUvec);
    p.acc.add(p.vel.copy().mult(-params.damping));

    p.update();
    p.edges();
    p.display();
  }
}

function drawFlowField() {
  stroke(210, 20, 60, 40);
  strokeWeight(1);
  let step = 25;
  for (let x = 0; x < width; x += step) {
    for (let y = 0; y < height; y += step) {
      let a = noise(x * 0.01, y * 0.01, frameCount * 0.003) * TWO_PI * 2;
      let v = p5.Vector.fromAngle(a).setMag(12);
      push();
      translate(x, y);
      line(0, 0, v.x, v.y);
      pop();
    }
  }
}

function getFlowField(x, y) {
  let a = noise(x * flowScale, y * flowScale, frameCount * 0.001) * TWO_PI * 2;
  return p5.Vector.fromAngle(a).setMag(0.9);
}

class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(0.5, 1.4));
    this.acc = createVector(0, 0);
    this.h = random(0, 360);
    this.OUvec = p5.Vector.random2D().mult(0.2);
    this.dt = 1.0;
    this.size = random(2, 5);
  }

  updateOU(theta, mu, sigma) {
    let muVec = createVector(mu, mu);
    let det = p5.Vector.sub(muVec, this.OUvec).mult(theta * this.dt);
    let sto = createVector(
      randomGaussian() * sigma * sqrt(this.dt),
      randomGaussian() * sigma * sqrt(this.dt)
    );
    this.OUvec.add(det).add(sto);
    if (this.OUvec.mag() > 6) this.OUvec.setMag(6);
  }

  update() {
    this.vel.add(this.acc);
    if (this.vel.mag() > 6) this.vel.setMag(6);
    this.pos.add(this.vel);
    this.h = (this.h + this.vel.heading() * 0.5) % 360;
  }

  edges() {
    if (this.pos.x < -20) this.pos.x = width + 20;
    if (this.pos.x > width + 20) this.pos.x = -20;
    if (this.pos.y < -20) this.pos.y = height + 20;
    if (this.pos.y > height + 20) this.pos.y = -20;
  }

  display() {
    noStroke();
    let alpha = map(this.vel.mag(), 0, 6, 30, 100);
    fill(this.h, 80, 90, alpha);
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.vel.heading());
    beginShape();
    vertex(this.size * 2, 0);
    vertex(-this.size, this.size * 0.7);
    vertex(-this.size, -this.size * 0.7);
    endShape(CLOSE);
    pop();
  }
}
```

Un enlace al proyecto en el editor de p5.js.
https://editor.p5js.org/luciana.gp0531/sketches/Z1bEcHu8p

Una captura de pantalla representativa de tu pieza de arte generativo.

<img width="904" height="617" alt="image" src="https://github.com/user-attachments/assets/7a2b37e4-edcc-4d67-a864-557b10bc6515" />
