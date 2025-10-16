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
