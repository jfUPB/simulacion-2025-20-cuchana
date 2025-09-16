# Unidad 1


## 🛠 Fase: Apply

Nota del profe: faltaba una feature más de la unidad 1. Aquí está el código completo:

``` js
let flores = [];
let tiempo = 0;

function setup() {
  createCanvas(600, 400);
  background(255);
  noStroke();
}

function draw() {
  background(240, 240, 255, 20); // efecto de rastro suave

  tiempo += 0.01;

  // Añadir flor con el mouse presionado
  if (mouseIsPressed) {
    let x = mouseX;
    let y = mouseY;
    let size = constrain(randomGaussian(30, 10), 5, 80); // distribución normal
    let petalos = floor(random(4, 9)); // aleatorio discreto
    let colorBase = color(random(200, 255), random(100, 180), random(180, 255), 150);
    let ruidoRotacion = random(1000); // punto de partida único en el ruido

    flores.push({ x, y, size, petalos, color: colorBase, ruidoRotacion });
  }

  // Dibujar todas las flores
  for (let flor of flores) {
    dibujarFlor(flor);
  }

  // Limitar la cantidad de flores
  if (flores.length > 500) {
    flores.splice(0, 10);
  }
}

function dibujarFlor(flor) {
  push();
  translate(flor.x, flor.y);

  // Rotación basada en Perlin noise
  let rotacion = map(noise(flor.ruidoRotacion + tiempo), 0, 1, -PI / 4, PI / 4);
  rotate(rotacion);

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
