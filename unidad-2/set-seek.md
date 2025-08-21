



































































































### Actividad 5: 

```js
let t=0;
let direccion=1;

function setup() {
    createCanvas(500, 500);
}

function draw() {
    background(200);

    let v0 = createVector(50, 50);
    let v1 = createVector(300,0);
    let v2 = createVector(0, 300);
    let v3 = p5.Vector.lerp(v1, v2, t);//linear interpolation, sirve para encontrar el punto intermedio entre v1 y v2
   
    let v4 = p5.Vector.sub(v2, v1); //sub= resta

  // Hace que se mueva
    t += 0.01 * direccion;
    if (t > 1 || t < 0) direccion *= -1;

    //Cambia el color respondiendo a la porximidad
    let colorDinamico = lerpColor(color('blue'), color('red'), t);

  // Dibujar vectores principales
    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(v0, v3, colorDinamico);// esta flecha respone al color
    drawArrow(p5.Vector.add(v0, v1), v4, 'green');
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```
https://editor.p5js.org/luciana.gp0531/sketches/D9xmvZr43

¿Cómo funciona lerp() y lerpColor().
Cada uno funciona interpolando, lerp entre 2 valores y lerpColor entre colores

¿Cómo se dibuja una flecha usando drawArrow()?
Se dibuja usando los siguientes parámetros:
- base: vector base desde donde parte la flecha.
- vec: vector dirección y magnitud de la flecha.
- myColor: color de la flecha.

### Actividad 6:

- Cuál es el concepto del marco motion 101 y cómo se interpreta geométricamente.
  Es un marco basico de simulacion de movimiento utilizando vectores. Geometricamente se interpreta como una cadena de efectos, la caeleracion cambia la direccion o magnirud de la velocidad, la velocidad cambia la trayectoria del objeto en el espacio, finalmente el objeto se mueve en el canvas de acuerdo a esos cambios.
  
- ¿Cómo se aplica motion 101 en el ejemplo?
  En el código, el objeto Mover tiene dos vectores principales:
  - position: la ubicación acual del objeto
  - velocity: el cambio que tendrá la posicion en cada frame
    El objeto se mueve de forma fluida simplemente porque su posición se actualiza constantemente usando la velocidad. No hay necesidad de modificar directamente la posición a mano, sino que todo se controla con vectores

### Actividad 7:
¿Qué observaste cuando usas cada una de las aceleraciones propuestas?

1. Aceleración constante
  Observación: El objeto se acelera uniformemente en una dirección, su velocidad crece de forma constante.
  Resultado visual: Se mueve cada vez más rápido en línea recta.

2. Aceleración aleatoria
  Observación: El movimiento se vuelve caótico e impredecible, pero con fluidez.
  Resultado visual: El objeto cambia constantemente de dirección, simulando algo como el viento o un borracho.

3. Aceleración hacia el mouse
  Observación: El objeto parece tener intención, como si “quisiera” alcanzar el mouse.
  Resultado visual: Se dirige al mouse, primero lento y luego más rápido, dependiendo de la distancia.

### Actividad 1:

¿Cómo funciona la suma dos vectores en p5.js?
Se hace con el método .add(), en el caso de este código la línea es position.add(velocity); esto modifica el vector position directamente sumando x y y del vector velocity

¿Por qué esta línea position = position + velocity; no funciona?

El operador + no funciona para sumar vectores, esta linea intentaria operar las referencias como si fueran strings.

### Actividad 2: 

¿Qué tuviste que hacer para hacer la conversión propuesta?
Elegí el ejemplo 0.6, para esto uni las variables this.x y this.y para crear un vector de la posicion, por lo cual se deben llamar con this.position.x, ya no se usan varoles sueltos si no que se encapsula en un vector
Muestra el código que utilizaste para resolver el ejercicio.
```js
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.position = createVector(0, 0); 
    this.tx = 0;
    this.ty = 10000;
  }

  step() {

    let x = map(noise(this.tx), 0, 1, 0, width);
    let y = map(noise(this.ty), 0, 1, 0, height);
    this.position.set(x, y); // 
    this.tx += 0.01;
    this.ty += 0.01;
  }

  show() {
    strokeWeight(2);
    fill(127);
    stroke(0);
    circle(this.position.x, this.position.y, 48); 
  }
}
```

### Actividad 3: 

¿Qué resultado esperas obtener en el programa anterior?
Espero que en la consola se escriban los vectores con [6,9,0] y [20,30,0] y se imprima Only once, por el noLoop()
¿Qué resultado obtuviste?
<img width="357" height="92" alt="image" src="https://github.com/user-attachments/assets/1ad2062e-8f11-407d-a3ca-fb9d084c643d" />
Es el esperado pero con un formato distinto
Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.
Voy a hacer un ejemplo de paso por referencia, lo que se hace es una referencia al mismo objeto en memoria, los cambios que se hacen dentro de la funcion afectan la variable
```js
let obj = {x: 5};
function modificar(o) {
  o.x = 10;
}
modificar(obj);
console.log(obj.x); // 10 (sí cambia)
```
¿Qué tipo de paso se está realizando en el código?
Se está realizando paso por referencia.
El objeto p5.Vector que se pasa a la función playingVector es una instancia, y en js los objetos se pasan por referencia, lo que significa que los cambios dentro de la función afectan el objeto original.
¿Qué aprendiste?
Aprendí varias cosas de js, los objetos como el p5.vector se pasan por referencia, por lo tanto cuando modifico un vector dentro de una funcion, si se afecta el vector original declarado en otra parte del programa. Me ayudó a entender como se manejan estas estructuras de datos y como optimizar el uso de memoria y funciones cuando trabajo en p5.js

### Actividad 4: 
¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?
El método mag devuelve la magnitud (longitud) del vector, la diferencia con magSq() es que devuelve el cuadrado de la magnitud, sin hacer raíz cuadrada, lo cual es más eficiente cuando no se necesita la raiz cuadrada.

¿Para qué sirve el método normalize()?
Convierte el vector en un vector unitario, de longitud 1, manteniendo su dirección, lo cual es útil cuando solo te interesa la direccion y no magnitud.

Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?
Este método indica qué tanto apuntan dos vectores en la misma direccion. Si el resultado es 0, son perpendiculares, si es positivo, apuntan en la misma direccion.

El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?



Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.

¿Para que te puede servir el método dist()?

¿Para qué sirven los métodos normalize() y limit()?

