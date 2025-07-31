# Unidad 2

## 🔎 Fase: Set + Seek

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

