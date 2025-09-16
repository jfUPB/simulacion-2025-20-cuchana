# Unidad 3


## 🛠 Fase: Apply

1. Diseña e implementa tu obra generativa interactiva en tiempo real.

- Debe ser interactiva.
  El mouse crea mas cuerpos y arrastra los existentes
- Debes usar al menos dos algoritmos diferentes de la unidad 1, además de random.
  Usé Verlet Integration, se usa para las posiciones de los cuerpos en cada frame.
  Y satisfaccion de restricciones, que asegura que los cuerpos conectados mantengan una distancia fija, corrigiendo la posición si se estiran o comprimen mucho
2. Explica cómo modelaste el problema de los n-cuerpos en tu obra.
El problema de los n-cuerpos consiste en simular cómo múltiples cuerpos se influyen mutuamente mediante fuerzas (en la física real, gravitatorias).

En mi obra cada cuerpo es un círculo con masa y radio calculados en función de esa masa.

Cada par de cuerpos ejerce una fuerza de atracción sobre el otro, proporcional al producto de sus masas e inversamente proporcional al cuadrado de la distancia, inspirado en la Ley de Gravitación Universal de Newton.

La base (root) es un cuerpo fijo que funciona como punto de anclaje, los conectores (constraints) entre cuerpos representan varillas rígidas que limitan el movimiento y producen un comportamiento oscilatorio similar a un móvil colgante.

Así, la obra combina la idea física de los n-cuerpos con la estética de los móviles de Calder: cuerpos de diferentes colores y masas interactuando con gravedad simulada y unidos por varillas que imitan la estructura de una escultura cinética.

3. Copia el enlace a tu simulación en p5.js.
 
https://editor.p5js.org/luciana.gp0531/sketches/pTbMziOrm

5. Copia el código.

```js
let bodies = [];
let constraints = [];
let selected = null;
let root; // referencia a la base

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100);
  initSystem();
}

function initSystem() {
  bodies = [];
  constraints = [];
  // cuerpo raíz (fijo, como anclaje del móvil)
  root = makeBody(width/2, height/3, 200, true);
  // crear ramas
  for (let i = 0; i < 5; i++) {
    let b = makeBody(width/2 + (i - 2) * 80, height/2, random(5, 20));
    connect(root, b, 150);
  }
}

function makeBody(x, y, m, fixed = false) {
  let b = {
    p: createVector(x, y),
    pPrev: createVector(x, y + random(-1, 1)),
    acc: createVector(0, 0),
    mass: m,
    r: sqrt(m) * 2 + 4,
    fixed: fixed,
    col: color(random(360), 80, 100)
  };
  bodies.push(b);
  return b;
}

function connect(a, b, rest) {
  constraints.push({ a, b, rest });
}

function applyForces() {
  let G = 0.2;
  for (let b of bodies) b.acc.set(0, 0);

  for (let i = 0; i < bodies.length; i++) {
    for (let j = i + 1; j < bodies.length; j++) {
      let a = bodies[i], b = bodies[j];
      let dir = p5.Vector.sub(b.p, a.p);
      let dist2 = dir.magSq() + 0.01;
      let force = (G * a.mass * b.mass) / dist2;
      dir.normalize().mult(force);
      a.acc.add(p5.Vector.div(dir, a.mass));
      b.acc.sub(p5.Vector.div(dir, b.mass));
    }
  }
}

function verletStep() {
  for (let b of bodies) {
    if (b.fixed) continue;
    let temp = b.p.copy();
    let vel = p5.Vector.sub(b.p, b.pPrev).mult(0.98);
    b.p.add(vel);
    b.p.add(b.acc);
    b.pPrev = temp;
  }
}

function satisfyConstraints() {
  for (let c of constraints) {
    let a = c.a, b = c.b;
    let delta = p5.Vector.sub(b.p, a.p);
    let d = delta.mag();
    if (d === 0) continue;
    let diff = (d - c.rest) / d;
    if (!a.fixed && !b.fixed) {
      a.p.add(delta.mult(0.5 * diff));
      b.p.sub(delta.mult(0.5 * diff));
    } else if (a.fixed) {
      b.p.sub(delta.mult(diff));
    } else if (b.fixed) {
      a.p.add(delta.mult(diff));
    }
  }
}

function draw() {
  background(20);

  // física
  applyForces();
  verletStep();
  for (let i = 0; i < 3; i++) satisfyConstraints();

  // dibujar barras
  stroke(200);
  strokeWeight(2);
  for (let c of constraints) {
    line(c.a.p.x, c.a.p.y, c.b.p.x, c.b.p.y);
  }

  // dibujar cuerpos
  noStroke();
  for (let b of bodies) {
    fill(b.col);
    ellipse(b.p.x, b.p.y, b.r * 2, b.r * 2);
  }

  // resaltar si hay cuerpo seleccionado
  if (selected) {
    stroke(0, 0, 100);
    noFill();
    ellipse(selected.p.x, selected.p.y, selected.r * 3);
  }
}

function mousePressed() {
  selected = getBodyAt(mouseX, mouseY);

  if (!selected) {
    // crear nuevo cuerpo conectado al más cercano
    let b = makeBody(mouseX, mouseY, random(5, 15));
    let near = nearestBody(b);
    if (near) connect(near, b, dist(near.p.x, near.p.y, b.p.x, b.p.y));
  } else if (!selected.fixed) {
    // solo permitir arrastrar si no es fijo
    selected.fixed = true;
  } else {
    selected = null; // si es fijo (ej: root), no se selecciona
  }
}

function mouseDragged() {
  if (selected) {
    selected.p.set(mouseX, mouseY);
  }
}

function mouseReleased() {
  if (selected && selected !== root) {
    selected.fixed = false; // solo liberar si no es la base
  }
  selected = null;
}

function getBodyAt(x, y) {
  for (let b of bodies) {
    if (dist(x, y, b.p.x, b.p.y) < b.r * 2) return b;
  }
  return null;
}

function nearestBody(target) {
  let best = null, bestd = 1e9;
  for (let b of bodies) {
    if (b === target) continue;
    let d = p5.Vector.dist(b.p, target.p);
    if (d < bestd) {
      bestd = d; best = b;
    }
  }
  return best;
}
```

7. Captura una imagen representativa de tu ejemplo.

<img width="837" height="615" alt="image" src="https://github.com/user-attachments/assets/1d047231-8484-4aa2-bfae-96e6f1103193" />
 
   
