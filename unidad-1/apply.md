# Unidad 1

## 🛠 Fase: Apply

### Actividad 8: 
- Un texto donde expliques el concepto de obra generativa.
  Es una pieza artistica creada con algoritmos o lineamientos que permiten que se genere la obra de forma dinamica y muchas veces usa elementos aleatroios o procedimentales.
  Mi obra es una escena donde "flores" suaves y orgánicas crecen al mover el mouse.
  Su posición horizontal se basa en ruido Perlin para fluidez.
  Su tamaño se decide con distribución normal.
  Su color se basa en random(), pero con leves variaciones coherentes.
  Es interactiva, cada clic agrega nuevas flores.
- Copia el código en tu bitácora.
```js
let flores = [];

function setup() {
  createCanvas(600, 400);
  background(255);
  noStroke();
}

function draw() {
  background(240, 240, 255, 20); // rastro suave

  if (mouseIsPressed) {
    let x = mouseX;
    let y = mouseY;
    let size = constrain(randomGaussian(30, 10), 5, 80); // tamaño con distribución normal
    let petalos = floor(random(4, 9)); // número de pétalos
    let colorBase = color(random(200, 255), random(100, 180), random(180, 255), 150); // color suave

    flores.push({ x, y, size, petalos, color: colorBase });
  }

 
  for (let flor of flores) {
    dibujarFlor(flor);
  }

  // Limitar la cantidad total de flores
  if (flores.length > 500) {
    flores.splice(0, 10);
  }
}

function dibujarFlor(flor) {
  push();
  translate(flor.x, flor.y);
  fill(flor.color);
  for (let i = 0; i < flor.petalos; i++) {
    let angle = TWO_PI * i / flor.petalos;
    let px = cos(angle) * flor.size;
    let py = sin(angle) * flor.size;
    ellipse(px, py, flor.size / 2, flor.size / 2);
  }
  pop();
}
```
- Coloca en enlace a tu sketch en p5.js en tu bitácora: https://editor.p5js.org/luciana.gp0531/sketches/fdxFA3Zcs
- Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.
  <img width="554" height="371" alt="image" src="https://github.com/user-attachments/assets/1a75b682-f5d2-48da-b4e5-04f9841d9c74" />
