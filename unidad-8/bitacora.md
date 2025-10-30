# Evidencias de la unidad 8

## Actividad 1
Usé de referencia Sónar+D CCCB 2020 y Dimension N

1. Describe tus observaciones sobre la conexión sonido-imagen en al menos dos de las performances vistas.

En el Sonar la relación entre la música y los visuales es muy armoniosa, el piano se refleja en movimientos suaves y muy orgánicos, como si las imágenes rspondieran directamente a las notas. Tambien noté que Alba usó tonos frios y lineas, comunicando esa fluidez y calma, aun asi cuando la inyensidad de la musica aumentaba, los visuales respondian a esto. Noté que la imagen no sigue directamente el beat sino la atmosfera que genera la presentación.

En Dimension N me pareció que la imagen era mas envolvente y dinamica, como la musica que se estaba reproduciendo en ese momento. Me dio la sensación de que la cpnexión imagen-sonido era más potente y ritmica, pues se nota como claramente las frecuencias afectan los visuales.
   
2. Explica qué elementos te parecieron generativos y por qué crees que cada visualización sería única.

- Los patrones cambiaban constantemente, no habia repeticiones exactas
- Las formas se transforman y desaparecen de una forma muy organica, no se ve forzado
- Las texturas y colores evolucionan con la música

3. Comparte tu reflexión sobre la sensación de “liveness”.

Al ver los performance de Alba es muy facil vivir esta sensación, al saber que esos visuales son generados y controlados en vivo hace que toda la experiencia sea mas autentica, se entiende que ella esta haciendo arte y "tocando" estos visuales en vivo. 

## Actividad 2

1. La pieza musical elegida (con enlace/archivo si es posible).

Elegí CHIHIRO de Billie Eilish, aquí esta el enlace: https://youtu.be/BY_XwvKogC8?si=CrI6sCDFcNBMx6Lm

2. La descripción de tu concepto visual.

En si la cancion habla como de una relación que se esta tornando toxica, me da muchas vibras de tonos oscuros, como azules, quiero que haya ondas que reaccionen al beat, y como en si la canción y el album evocan esa sensación de estar bajo agua

3. Los inputs seleccionados y la justificación de por qué los elegiste.

El input que seleccioné es el mouse para manejar las estrellas, su aceleracion, me parecio que era una forma más fluida de manejar esta variable y se veia más natural


4. ¿Qué algoritmos o técnicas planeas usar (ej: flow fields, flocking, física, partículas, etc.) y por qué?

Sistema de partículas para las estrellas y burbujas, porque quiero que sean independientes para poder que una reaccione a la voz de billie y la otra al mouse

Use WEBGL, para el efecto 3d de las estrellas, esto me lo recomendó chatgpt para hacer el efecto de que las estrellas se alejan

FFT para analizar las frecuencias de la cancion y usar estos datos para controlar lo visual

Ruido perlín para las ondas

5. Tus bocetos y una explicación de cómo los inputs influirán en los visuales.
   
Bocetos: 
- Fondo va a ser una imagen opaca, como transparente de dos personas que representen la dualidad y la conexion emocional
- Las estrella quiero que sea como una metafora de viaje en el espacio
- Las ondas van a ser como líquidas, que se vean muy fluidas que se muevan con la voz
- Las burbujas para el agua, mostrando como la perdida de aire y como emergen

Inputs: 
1. El audio: Las frecuencias graves hacen subir las burbujas, las nedias y altas controlan el brillo e intensidad de las estrellas y el beat las ondas
2. Mouse: La velocidad modifica la aceleración de las estrellas haciendo que sea lento o rápido

   
## Actividad 3

1. El código fuente completo de tu sketch en p5.js.
```js
// Visual reactivo a "Chihiro" - Billie Eilish
// Luciana Gutiérrez 


let song;
let fft;
let bgImage;
let bubbles = [];
let stars = [];
let zOffset = 0;

// 🔄 alternancia visual
let showStars = true;
let showBubbles = false;

// variables para control del mouse
let mouseSpeed = 0;
let prevMouseX = 0;
let prevMouseY = 0;

function preload() {
  song = loadSound("chihiro.mp3");
  bgImage = loadImage("personas.jpg");
}

function setup() {
  createCanvas(windowWidth, windowHeight, WEBGL);
  noFill();
  fft = new p5.FFT();
  song.loop();

  for (let i = 0; i < 30; i++) {
    bubbles.push(new Bubble());
  }

  for (let i = 0; i < 800; i++) {
    stars.push(new Star());
  }
}

function draw() {
  // 🌫 fondo translúcido suave (sin rastros fuertes)
  push();
  noStroke();
  fill(10, 20, 40, 50);
  rect(-width / 2, -height / 2, width, height);
  pop();

  // fondo base con textura
  push();
  noStroke();
  tint(255, 5);
  texture(bgImage);
  plane(width, height);
  pop();

  // 🔊 energía del sonido
  let spectrum = fft.analyze();
  let energyBass = fft.getEnergy("bass");
  let energyMid = fft.getEnergy("mid");
  let energyHigh = fft.getEnergy("treble");
  let voiceEnergy = (energyMid + energyHigh) / 2;

  // ⚡ calcular velocidad del mouse
  let dx = mouseX - prevMouseX;
  let dy = mouseY - prevMouseY;
  mouseSpeed = sqrt(dx * dx + dy * dy);
  prevMouseX = mouseX;
  prevMouseY = mouseY;

  // 🪐 estrellas centradas en el mouse
  if (showStars) {
    push();
    for (let s of stars) {
      s.update(voiceEnergy, energyBass, mouseSpeed);
      s.display();
    }
    pop();
  }

  // 💫 ondas líquidas reactivas
  push();
  translate(-width / 2, 0, 0);
  for (let i = 0; i < 6; i++) {
    let alpha = map(i, 0, 6, 60, 5);
    let amp = map(energyBass + energyMid * 0.6, 0, 255, 10, 180);
    let freq = map(energyHigh, 0, 255, 0.004, 0.015);

    stroke(150 + i * 10, 180 + i * 5, 255, alpha);
    strokeWeight(map(i, 0, 6, 0.5, 2));

    beginShape();
    for (let x = 0; x < width; x += 10) {
      let y =
        sin(x * freq + frameCount * 0.02 + i) *
        amp *
        noise(i, frameCount * 0.004);
      vertex(x, y);
    }
    endShape();
  }
  pop();

  // 🫧 burbujas
  if (showBubbles) {
    push();
    translate(-width / 2, -height / 2, 0);
    for (let b of bubbles) {
      b.update(energyBass);
      b.display();
    }
    pop();
  }
}

// 🌌 Clase para las estrellas del viaje
class Star {
  constructor() {
    this.reset();
  }

  reset() {
    this.x = random(-width / 2, width / 2);
    this.y = random(-height / 2, height / 2);
    this.z = random(width);
    this.alpha = random(150, 255);
  }

  update(voiceEnergy, bassEnergy, mouseSpeed) {
    // velocidad sensible al movimiento del mouse y la música
    let speedFactor =
      map(mouseSpeed, 0, 100, 0.5, 6) + map(voiceEnergy, 0, 255, 0.2, 4);
    this.z -= speedFactor;

    // cuando se aleja, reinicia cerca del mouse
    if (this.z < 1) {
      this.z = width;
      this.x = mouseX - width / 2 + random(-100, 100);
      this.y = mouseY - height / 2 + random(-100, 100);
    }
  }

  display() {
    let sx = map(this.x / this.z, 0, 1, 0, width);
    let sy = map(this.y / this.z, 0, 1, 0, height);
    let r = map(this.z, 0, width, 6, 0);

    noStroke();
    fill(180, 200, 255, this.alpha);
    ellipse(sx - width / 2, sy - height / 2, r, r);
  }
}

// 🫧 Clase para burbujas
class Bubble {
  constructor() {
    this.x = random(width);
    this.y = random(height);
    this.size = random(10, 40);
    this.alpha = random(50, 150);
    this.speed = random(0.2, 1);
  }

  update(energyBass) {
    this.y -= this.speed + map(energyBass, 0, 255, 0, 1);
    if (this.y < -this.size) {
      this.y = height + this.size;
      this.x = random(width);
    }
  }

  display() {
    noStroke();
    fill(180, 200, 255, this.alpha);
    ellipse(this.x, this.y, this.size);
  }
}

// 🌠 Control de alternancia
function keyPressed() {
  if (key === "1") showStars = !showStars;
  if (key === "2") showBubbles = !showBubbles;
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}

```
   
2. Un enlace a tu sketch en el editor de p5.js.

https://editor.p5js.org/luciana.gp0531/sketches/1tmP-qSpN

4. Capturas de pantalla mostrando tu pieza en acción.

<img width="626" height="662" alt="image" src="https://github.com/user-attachments/assets/5e103282-870b-49bc-aa25-04254cfef7e9" />
<img width="623" height="695" alt="image" src="https://github.com/user-attachments/assets/d50e9a84-1dd7-451e-b548-1dd27f39bb6d" />

Autoevaluación: 5, completé todas las actividades, hice de forma autonoma el proceso de diseño, me costó la materializacion en codigo pero lo logre

