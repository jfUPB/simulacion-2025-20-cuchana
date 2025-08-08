



































































































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

