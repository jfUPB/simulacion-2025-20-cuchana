# Evidencias de la unidad 6

## Actividad 1:
<img width="1919" height="1070" alt="image" src="https://github.com/user-attachments/assets/a60bb4e6-e012-496c-bdc8-2ba1ecee85cf" />
<img width="1067" height="1072" alt="image" src="https://github.com/user-attachments/assets/b19084ea-9529-4b4b-bcb6-d6f7ac15e1b9" />
Esta imagen me llamo la atencion basandome en un punto del video donde el artista mencionó que le gusta inspirar trazos y escencias del dibujo a mano para elementos de sus obras de arte generativo, que aunque son cosas netamente generadas y hechas por elementos técnologicos, cuando las vi se puede notar la naturalidad de algunos trazos, también por la parte de que quiere que disfrutemos su arte como el disfruta la música

## Actividad 2: 

1. ¿Qué es una fuerza de dirección (steering force)?

   Es una fuerza computacional usada en simulaciones que corrige la direccion y velocidad para que su movimiento sea de manera realista. A diferencia de una fuerza fisica como gravedad o friccion, la steering force no representa una ley natural, sino una regla de comportamiento que orienta al agente hacia un objetivo, evita obstaculo ajustando su trayectoria.
2. ¿Qué diferencia tiene este tipo de fuerza con las que ya hemos estudiado en el contexto de la simulación de agentes?

Hemos estudiado fuerzas físicas clásicas, que provienen de modelos matemáticos del mundi real, son fuerzas externas que afectan al objeto regidos por las leyes de Newton, la steering force que acabamos de ver son fuerzas artificiales diseñadas para modelar comportamientos, no buscan realismo físicp en si, sino credibilidad y coherencia.
3. ¿Qué relación tiene la steering force con Craig Reynolds y su trabajo en simulación de comportamiento animal?

Según lo que vi en su página tiene 3 reglas principales de steering para crear un comportamiento grupal emergente: 
1. Separación: evitar cohesiones
2. Alineación: moverse en la misma dirección que los vecinos
3. Cohesión: acercarse al grupo

Estas reglas se implementan con steering forces: cada una calcula un vector de fuerza y la suma de todas determina el nuevo rumbo del agente.


## Actividad 3: 

- Explica brevemente la estructura de datos usada para el campo de flujo y cómo se generan sus vectores.

     - Tamaño: cols*rows donde cols=floor(width/resolution) y rows=floor(height/resolution)
     - Cada celda es un vector y representa la dirección del flujo para esa región de la pantalla
     - Noise x00f,yoof,zoff se una para obtener un valor entre 0 y 1, se mapea a un angulo (noise+TWO_PI*K) y se crea el vector con p5.vector.fromAngle(angle)
- Describe con tus palabras cómo un agente utiliza el campo para calcular su fuerza de dirección.
     - Hace un mapeo de posicion con la cuadricula de filas y columnas, obitiene el vector deseado, convierte el vector direccion en el vector deseado de velocidad y calcula la fuerza de steering:
     - steer = desired - velocity
     - steer.limit(maxforce)
       Luego el apllyForce(steer) suma a la aceleracion y se pone en update
- Lista los parámetros clave identificados (resolución, maxspeed, maxforce).
     - resolution
     - maxspeed
     - maxforce
     - noise: xoof,yoff,zoff
     - framecount
- Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el movimiento y comportamiento colectivo de los agentes. Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.

Reemplacé el cálculo puramente basado en noise() por un campo espiral + ruido + rotación temporal
Cambios en el mapping
```js
let column = floor(constrain(lookup.x / this.resolution, 0, this.cols - 1));
let row = floor(constrain(lookup.y / this.resolution, 0, this.rows - 1));
let index = row * this.cols + column;
return this.field[index].copy();
```
  Cambios en el calculo del steering
```js
let desired = flow.lookup(this.pos); // dirección del campo
desired.mult(this.maxspeed);         // vector velocidad deseada
let steer = p5.Vector.sub(desired, this.vel);
steer.limit(this.maxforce);
this.applyForce(steer);
```
Cambio en el init
```js
const cx = width / 2;
const cy = height / 2;
for (let i = 0; i < this.cols; i++) {
  for (let j = 0; j < this.rows; j++) {
    let index = j * this.cols + i;
    let x = i * this.resolution + this.resolution / 2;
    let y = j * this.resolution + this.resolution / 2;

    let angleToCenter = atan2(y - cy, x - cx);
    let radius = dist(x, y, cx, cy);
    let maxR = max(cx, cy);
    let swirlAmount = map(radius, 0, maxR, 0, PI * 1.8);
    let n = noise(i * 0.18, j * 0.18, zoff);
    let angle = angleToCenter + swirlAmount * (n - 0.5) + frameCount * 0.007;

    let v = p5.Vector.fromAngle(angle);
    v.setMag(1);
    this.field[index] = v;
  }
}
zoff += 0.006;
```
<img width="566" height="311" alt="image" src="https://github.com/user-attachments/assets/e56cad22-d006-4d57-8db1-8e84080ec2ce" />

Con cada click se asentua mas el espiral

## Actividad 4:

- Explica con tus palabras el objetivo y la lógica general de cálculo de cada una de las tres reglas de Flocking (Separación, Alineación, Cohesión).
     - Separacion:
          1. Objetivo: evitar que los agentes se amontonen demasiado
          2. Logica general de cálculo: cada agentge revisa que particulas vecinas estan demasiado cerca, para que cada uno calcule un vector que apunta lejos de su vecino. Estos vectores se suman, promedian y se convierten en una fuerza de steering q lo empuja en direccion contraria.
             
    - Alineación:
         1. Objetivo: moverse en la misma dirección promedio de los vecinos cercanos
         2. Lógica de calculo: el agente suma los vectores de velocidad de todos sus vecinos y calcula el promedio. Ese vector promedio es la dirección deseada.

   - Cohesión:
        1. Objetivo: Acercarse a la posición promedio de los vecinos, osea mantener el grupo unido
        2. Lógica de cálculo: el agente calcula el promedio de las posiciones de sus vecinos y luego genera un vector que apunta hacia ese punto promedio
           
- Lista los parámetros clave identificados (radio de percepción, pesos de las reglas, maxspeed, maxforce).
     - Perception radius
     - Pesos de las reglas
     - Velocidad máxima
     - Fuerza máxima
- Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el comportamiento colectivo del enjambre (¿Se dispersan? ¿Forman grupos compactos? ¿se mueven caóticamente?). Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.

## Actividad 5: 

- Documenta todo el proceso de diseño y creación en tu bitácora, incluyendo bocetos y decisiones de diseño.
  Primero empecé por elegir la canción, elegí luces de tecno que es una canción de feid que me gusta demasiado y empece a desarrollar la parte de seet y seek simultaneo a esto, primero me senté a escuchar la canción con los ojos cerrados y me imaginé en un concierto o una disco donde la canción estuviera sonando y las visuales respondieran a esto, como técnicamente es una mezcla entre tecno y reggeton me imagine el lugar a oscuras con verde que es el color caracteristico de feid y este albúm, como en la cancion habla de alguien, una mujer que no ve si no en una discoteca despues de un encuentro me gusto que el flow pueda cambiar entre disperso a junto, ya ahi empecé a mirar como lo quería representar, y como las "luces de tecno" tienden a ser lasers hice que las particulas medio simularan eso sin perder movimiento natural y que el verde responda a la voz y el morado a los bajos
  
- El código fuente completo de tu sketch en p5.js.
```js
let song, fft;
let flowField = [];
let cols, rows;
let inc = 0.1;
let scl = 20;
let zoff = 0;
let bassParticles = [];
let voiceParticles = [];
let flowIntensity = 0.3;
let speedFactor = 1;
let fadeAmount = 30; // cuánto se borra el fondo

function preload() {
  song = loadSound("Feid - LUCES DE TECNO [ Letra Lyrics ].mp3");
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  cols = floor(width / scl);
  rows = floor(height / scl);

  fft = new p5.FFT();

  for (let i = 0; i < 200; i++) {
    bassParticles[i] = new Particle("bass");
  }

  for (let i = 0; i < 200; i++) {
    voiceParticles[i] = new Particle("voice");
  }
}

function draw() {
  // Fondo con efecto fantasma
  background(0, fadeAmount);

  let spectrum = fft.analyze();
  let bass = fft.getEnergy("bass");
  let voice = fft.getEnergy("mid");

  // Campo dinámico
  let yoff = 0;
  for (let y = 0; y < rows; y++) {
    let xoff = 0;
    for (let x = 0; x < cols; x++) {
      let angle = noise(xoff, yoff, zoff) * TWO_PI * 4;
      let v = p5.Vector.fromAngle(angle);

      let intensity = map(bass + voice, 0, 510, 0.05, flowIntensity);
      v.setMag(intensity);

      flowField[x + y * cols] = v;
      xoff += inc;
    }
    yoff += inc;
  }
  zoff += 0.003;

  // Dibujar partículas
  for (let p of bassParticles) {
    p.follow(flowField);
    p.update(speedFactor, bass);
    p.edges();
    p.show(color(180, 0, 255, 150), bass); // morado
  }

  for (let p of voiceParticles) {
    p.follow(flowField);
    p.update(speedFactor, voice);
    p.edges();
    p.show(color(0, 255, 100, 150), voice); // verde
  }
}

function mousePressed() {
  if (song.isPlaying()) {
    song.pause();
  } else {
    song.play();
  }
}

// ==== Interacciones en vivo ====
function keyPressed() {
  if (key === 'F' || key === 'f') {
    flowIntensity = (flowIntensity === 0.3) ? 1.0 : 0.3;
  }

  if (keyCode === UP_ARROW) speedFactor += 0.2;
  if (keyCode === DOWN_ARROW) speedFactor = max(0.2, speedFactor - 0.2);

  if (keyCode === 32) { // espacio limpia pantalla
    background(0);

  }
}

// Clase Partícula
class Particle {
  constructor(type) {
    this.type = type;
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.maxSpeed = 2;
    this.prevPos = this.pos.copy();
  }

  follow(vectors) {
    let x = floor(this.pos.x / scl);
    let y = floor(this.pos.y / scl);
    let index = x + y * cols;
    let force = vectors[index];
    if (force) this.applyForce(force);
  }

  applyForce(force) {
    this.acc.add(force);
  }

  update(speedFactor, energy) {
    let extraSpeed = map(energy, 0, 255, 0.5, 3);
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed * speedFactor * extraSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  show(col, energy) {
    stroke(col);
    strokeWeight(map(energy, 0, 255, 0.5, 3));
    line(this.pos.x, this.pos.y, this.prevPos.x, this.prevPos.y);
    this.updatePrev();
  }

  updatePrev() {
    this.prevPos.set(this.pos);
  }

  edges() {
    if (this.pos.x > width) { this.pos.x = 0; this.updatePrev(); }
    if (this.pos.x < 0) { this.pos.x = width; this.updatePrev(); }
    if (this.pos.y > height) { this.pos.y = 0; this.updatePrev(); }
    if (this.pos.y < 0) { this.pos.y = height; this.updatePrev(); }
  }
}
```
- Un enlace a tu sketch en el editor de p5.js.
  https://editor.p5js.org/luciana.gp0531/sketches/O7zH5ZBsG

- Capturas de pantalla mostrando tu pieza en acción.

<img width="1800" height="791" alt="image" src="https://github.com/user-attachments/assets/9c2c1814-5253-437e-bd57-63735a96c8fe" />

<img width="1841" height="803" alt="image" src="https://github.com/user-attachments/assets/e7edeeb9-6428-4110-9c91-c602404852f6" />

<img width="1826" height="804" alt="image" src="https://github.com/user-attachments/assets/0e2bc242-b1dc-480c-a401-cddf861d6d23" />


## Auto evaluación: 
Actividad 1,2,3 y 5: 5, hice a conciencia el proceso de diseño y las actividades propuestas, finalicé al 100% la documentacion
Actividad 4: 3.5, me falto el numeral 3
Nota: 4,7
Justificación: No logré finalizar la actividad 4 pero hice 2/3 puntos de esta actividad.

