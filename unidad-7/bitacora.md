# Evidencias de la unidad 7

## Actividad 1: 

<img width="1256" height="668" alt="image" src="https://github.com/user-attachments/assets/9a724778-6f85-4a3c-a8ff-410b5f7cacce" />
Esta de comedy y drama, logra esa conexión por medio de la C y la D, comedy es algo que trae risas y felicidad por lo cual simula una sonrisa, y drama, retrata cosas que en ese genero suelen dar tristeza o lastima y por eso la :( 


<img width="1172" height="500" alt="image" src="https://github.com/user-attachments/assets/61b09711-165d-4438-8f66-e54345beb149" />
Esta de vampiro me pareció super charra e inteligente, pues usar la M como colmillos y ponerles sangre le da esa visualización al concepto de vampiros usando la palabra

<img width="921" height="512" alt="image" src="https://github.com/user-attachments/assets/422c1d26-ef60-47d5-9c66-d30aa3c80b76" />
La de baloons logra eso con el efecto visual de inflar y dejar volar las O como si fueran globos, pero se complementa tambien mucho con el sonido de inflador

Mis ideas: 

Futbol: que aparezca Futbo, la b patee la O y llegue la L, la devuelva y se pare al lado formando la palabra

Popcorn: pense en crispetas pero no tiene una o para lograr lo que quiero, que es como poner un maíz pira antes de explotar y luego que explote como una crispeta

inbox: que este las letras y una caja llegue y los aplaste

## Actividad 2: 

Experimento 1: Hice cajitas que caen con gravedad
```js
let Engine = Matter.Engine,
    World = Matter.World,
    Bodies = Matter.Bodies;

let engine, world;
let boxes = [];
let ground;

function setup() {
  createCanvas(600, 400);
  engine = Engine.create();
  world = engine.world;

  ground = Bodies.rectangle(300, height, 600, 20, { isStatic: true });
  World.add(world, ground);
}

function mousePressed() {
  let box = Bodies.rectangle(mouseX, mouseY, 40, 40);
  World.add(world, box);
  boxes.push(box);
}

function draw() {
  background(220);
  Engine.update(engine);

  // Dibujar cajas
  for (let box of boxes) {
    push();
    translate(box.position.x, box.position.y);
    rotate(box.angle);
    rectMode(CENTER);
    rect(0, 0, 40, 40);
    pop();
  }

  // Dibujar suelo
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, 600, 20);
}

```

Experimento 2: bolitas con gravedad que se pueden mover con el mouse
```js
let Engine = Matter.Engine,
    World = Matter.World,
    Bodies = Matter.Bodies,
    Mouse = Matter.Mouse,
    MouseConstraint = Matter.MouseConstraint;

let engine, world, mConstraint;
let circles = [];

function setup() {
  createCanvas(600, 400);
  engine = Engine.create();
  world = engine.world;

  // Círculos que caen
  for (let i = 0; i < 5; i++) {
    let c = Bodies.circle(random(width), random(-100, 0), 20);
    World.add(world, c);
    circles.push(c);
  }

  // Suelo
  let ground = Bodies.rectangle(width / 2, height, width, 20, { isStatic: true });
  World.add(world, ground);

  // Mouse
  let canvasmouse = Mouse.create(canvas.elt);
  mConstraint = MouseConstraint.create(engine, { mouse: canvasmouse });
  World.add(world, mConstraint);
}

function draw() {
  background(200);
  Engine.update(engine);

  for (let c of circles) {
    ellipse(c.position.x, c.position.y, 40);
  }
}

```

Conceptos: 

Engine: es el motor fisico que calcula movimiento, gravedad y las colisiones

World: El espacio donde existen estos objetos fisicos que creamos

Bodies: Los objetos: ciruclos, rectaalgunos, poligonos, estos pueden ser estaticos o tener movimiento

Constrain: es la conexion entre cuerpo, que restringe el movimiento

MouseConstrain: permite que el mouse pueda arrastrar o interactuar con los objetos

## Actividad 3: 

1. Indica claramente la palabra elegida.
   Fútbol
2. Explica tu idea conceptual: ¿Cómo la animación física representa el significado de la palabra?
   Quiero con la O representar el balón y que las letras cumplan como la funcion de los jugadores, representando la dinámica de futbol como deporte
3. Describe brevemente los aspectos técnicos clave de tu implementación: ¿Cómo formaste las letras con Matter.js? ¿Qué propiedades físicas fueron importantes? ¿Usaste restricciones?
   Cada letra se creó como un cuerpo fisico independiente utilizando Bodies.rectangle() para las letras y Bodies.circle() para la pelota. Las letras fueron posicionadas para formar la palabra y poder controlar su movimiento y poder controlar la animación.
   El motor físico (Engine) y el mundo (World) fueron configurados con una gravedad baja para permitir colisiones naturales sin que los cuerpos se caigan demasiado rápido. La “O” fue reemplazada por un balón de fútbol dibujado con p5.js dentro de un cuerpo circular de Matter.js, de modo que respondiera a las colisiones de manera realista.

    La letra “B” recibió una velocidad inicial hacia la derecha mediante Body.setVelocity() para simular el movimiento de patear el balón, mientras que la “L” se ubicó de forma que pudiera colisionar de vuelta con la pelota y completar la palabra.
   
4. Incluye el código completo de tu sketch final.
```js
const Engine = Matter.Engine;
const World = Matter.World;
const Bodies = Matter.Bodies;
const Body = Matter.Body;

let engine, world;
let ball, ground;
let bBody, lBody;
let phase = "idle";
let alignTimer = 0;

function setup() {
  createCanvas(800, 400);
  textAlign(CENTER, CENTER);
  engine = Engine.create();
  world = engine.world;

  ground = Bodies.rectangle(400, 320, 800, 20, { isStatic: true });
  World.add(world, ground);


  bBody = Bodies.rectangle(310, 250, 60, 80, { isStatic: true });
  World.add(world, bBody);

  ball = Bodies.circle(390, 250, 25, {
    restitution: 0.8,
    friction: 0.01,
    density: 0.001,
  });
  World.add(world, ball);

  lBody = Bodies.rectangle(750, 250, 40, 100, {
    restitution: 0.6,
    friction: 0.02,
    density: 0.002,
  });
  World.add(world, lBody);
}

function draw() {
  background(255);
  Engine.update(engine);

  fill(0);
  noStroke();
  textSize(100);

  text("FUT", 170, 250);


  push();
  translate(bBody.position.x, bBody.position.y);
  text("B", 0, 0);
  pop();


  drawBall(ball.position.x, ball.position.y);

  push();
  translate(lBody.position.x, lBody.position.y);
  text("L", 0, 0);
  pop();

  if (phase === "kick") {
    // La B patea
    Body.applyForce(ball, ball.position, { x: 0.03, y: -0.01 });
    phase = "ballMoving";
  }

  // Cuando la pelota se mueve, la L empieza a avanzar
  if (phase === "ballMoving" && ball.position.x > 430) {
    Body.applyForce(lBody, lBody.position, { x: -0.015, y: 0 });
  }

  if (phase === "ballMoving" && lBody.position.x < 520) {
    alignTimer++;
    if (alignTimer > 60) {
      alignFinal();
      phase = "done";
    }
  }
}

function drawBall(x, y) {
  push();
  translate(x, y);
  fill(255);
  stroke(0);
  ellipse(0, 0, 50);
  fill(0);
  polygon(0, 0, 8, 6);
  for (let i = 0; i < 6; i++) {
    let a = TWO_PI / 6 * i;
    let px = cos(a) * 15;
    let py = sin(a) * 15;
    polygon(px, py, 5, 5);
  }
  pop();
}

function polygon(x, y, radius, npoints) {
  let angle = TWO_PI / npoints;
  beginShape();
  for (let a = 0; a < TWO_PI; a += angle) {
    let sx = x + cos(a) * radius;
    let sy = y + sin(a) * radius;
    vertex(sx, sy);
  }
  endShape(CLOSE);
}

function mousePressed() {
  if (phase === "idle") {
    phase = "kick";
  }
}


function alignFinal() {
  Body.setStatic(bBody, true);
  Body.setStatic(ball, true);
  Body.setStatic(lBody, true);

  Body.setPosition(bBody, { x: 310, y: 250 });
  Body.setPosition(ball, { x: 390, y: 250 });
  Body.setPosition(lBody, { x: 470, y: 250 });
}
```
5. Inserta una captura de pantalla estática Y un enlace a un GIF animado (¡Esencial!) que muestre tu tipografía semántica animada en acción.
   <img width="995" height="495" alt="image" src="https://github.com/user-attachments/assets/8df16d05-27ce-4929-ac72-9e68cf9ee60c" />

   <img width="761" height="493" alt="image" src="https://github.com/user-attachments/assets/d6daed4c-c10f-4ba2-a409-8bd14370ed5a" />
   
   ![Vídeo sin título ‐ Hecho con Clipchamp](https://github.com/user-attachments/assets/edcc9c00-4f64-466e-8d2e-d366b8e5ae5b)

## Autoevaluacion: 

5, complete todas las actividades a consiencia
