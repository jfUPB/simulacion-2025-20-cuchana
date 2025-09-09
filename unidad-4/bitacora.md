# Evidencias de la unidad 4

## Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> De la unidad 4 tomé el concepto de ondas, lo aplique en la obra en las palpitaciones de las "personas" 

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> De la unidad 3 use el concepto de atraccion gravitacional para que los hilos de memorias viajen alrededor de las personas

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Use el motion 101 para condicionar el movimiento de las lineas, garantizando un movimiento constante

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> El perlin noise, para la generacion aleatoria de los recuerdos

## ¿Cómo resolviste la interacción?
> Dejé que las personas decidan cuantas personas van a haber en el mapa y que puedan moverlas

## Enlace a la obra en el editor de p5.js

https://editor.p5js.org/luciana.gp0531/full/DH6uInUXz

## Código de la obra 

``` js
let movers = []; // recuerdos
let attractors = []; // personas
let flowScale = 0.0025; // tamaño de los recuerdos
let flowStrength = 0.45; //
let useFlow = true;
let wavesOn = true;
let trail;
let grabbing = null;
let uiHidden = false;

const NUM_MOVERS = 15;   // los recuerdos que se generan
const MAX_SPEED = 2;     // velocidad

function setup(){
  createCanvas(window.innerWidth, window.innerHeight);
  pixelDensity(1);
  trail = createGraphics(width, height);
  trail.pixelDensity(1);
  trail.clear();
  for(let i=0;i<NUM_MOVERS;i++){
    movers.push(new Mover(createVector(random(width), random(height))));
  } //crear el recuerdo
  attractors.push(new Attractor(width*0.5, height*0.5, 12)); // crear persona
}

function draw(){
  fill(11,14,26, 15);
  noStroke();
  rect(0,0,width,height);

  // energía preestablecida con onda seno
  let energy = 0.6 + 0.4 * sin(frameCount * 0.01);

  for(const a of attractors){
    a.update(energy);
    a.display();
  }

  for(const m of movers){
    if(useFlow){
      const angle = noise(m.pos.x*flowScale, m.pos.y*flowScale, frameCount*0.003)*TWO_PI*2;
      const flow = p5.Vector.fromAngle(angle).mult(flowStrength);
      m.applyForce(flow);
    }
    for(const a of attractors){
      const f = a.attract(m);
      m.applyForce(f);
    }
    m.update();
    m.edges();
    m.show(trail, wavesOn, energy);
  }

  image(trail, 0, 0);
  drawUI(energy);
}

class Mover{
  constructor(pos){
    this.pos = pos.copy();
    this.vel = p5.Vector.random2D().mult(random(0.5, 2));
    this.acc = createVector(0,0);
    this.mass = random(0.75, 1.75);
    this.hue = random(360);
  }
  applyForce(f){
    this.acc.add(p5.Vector.div(f, this.mass));
  }
  update(){
    this.vel.add(this.acc);
    this.vel.limit(MAX_SPEED);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }
  edges(){
    if(this.pos.x < 0) this.pos.x = width;
    if(this.pos.x > width) this.pos.x = 0;
    if(this.pos.y < 0) this.pos.y = height;
    if(this.pos.y > height) this.pos.y = 0;
  }
  show(pg, wavesOn, energy){
    let t = frameCount * 0.05 + this.hue * 0.01;
    let wave = wavesOn ? (sin(t) * 0.5 + 0.5) : 0.5;
    let sz = (0.6 + wave * 1.4) * energy * this.mass;

    pg.push();
    pg.noStroke();
    pg.fill(255, 255, 255, 25);
    pg.circle(this.pos.x, this.pos.y, sz*2);
    pg.pop();
  }
}

class Attractor{
  constructor(x, y, mass=20){
    this.pos = createVector(x,y);
    this.mass = mass;
    this.G = 1.4;
    this.strength = 1.0;
    this.pulse = 0;
  }
  attract(m){
    let force = p5.Vector.sub(this.pos, m.pos);
    let d = constrain(force.mag(), 12, 240);
    force.normalize();
    let magnitude = (this.G * this.mass * m.mass * this.strength) / (d*d);
    force.mult(magnitude);
    return force;
  }
  update(energy){
    this.pulse = 1 + 0.25 * sin(frameCount*0.05) * energy;
  }
  display(){
    push();
    translate(this.pos.x, this.pos.y);
    noFill();
    stroke(255, 255, 255, 80);
    strokeWeight(1);
    const r = this.mass * 2.2 * this.pulse;
    for(let i=0;i<2;i++){
      const rr = r + i*8 + 2*sin(frameCount*0.08 + i*0.9);
      ellipse(0,0, rr*2, rr*2);
    }
    noStroke();
    fill(255, 255, 255, 160);
    circle(0,0, r*0.6);
    pop();
  }
}

function mousePressed(){
  let found = null;
  for(const a of attractors){
    if(p5.Vector.dist(createVector(mouseX, mouseY), a.pos) < a.mass*2.5){
      found = a; break;
    }
  }
  if(found){
    grabbing = found;
  } else {
    attractors.push(new Attractor(mouseX, mouseY, random(10, 18)));
  }
}
function mouseDragged(){ if(grabbing){ grabbing.pos.set(mouseX, mouseY); } }
function mouseReleased(){ grabbing = null; }


function resetSketch(){
  trail.clear();
  movers.length = 0;
  for(let i=0;i<NUM_MOVERS;i++) movers.push(new Mover(createVector(random(width), random(height))));
}

function drawUI(energy){
  const readout = document.getElementById('readout');
  const s = `Partículas: ${movers.length} · Portales: ${attractors.length} · Flujo Perlin: ${useFlow?'on':'off'} · Ondas: ${wavesOn?'on':'off'} · Energía: ${energy.toFixed(2)}`;
  if(readout) readout.textContent = s;
}
function keyPressed(){
  if(key === 'q' || key === 'Q'){ 
    movers.length = 0;   // primero los borramos
    trail.clear();       // limpiar estelas
    resetSketch();       // y luego volvemos a generarlos
  }
}
```

## Captura de pantalla representativa
<img width="1852" height="813" alt="image" src="https://github.com/user-attachments/assets/5d1c2ba7-8ac7-4729-8cb2-49ac06c90140" />










