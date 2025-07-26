# Unidad 1

## 🔎 Fase: Set + Seek


### Actividad 1:
- Piensa y describe en una sola frase y en tus propias palabras cómo la aleatoriedad influye en el arte generativo.
- Altera distintas variables de la obra durante las ejecuciones o distintos plazos de tiempo, haciendo cada obra distinta a su manera

### Actividad 2

- Cuál es el papel de la aleatoriedad en su obra.
- Según tu perfil profesional, cómo se aplica el concepto de aleatoriedad en el tipo de proyectos que desarrollas. Ilustra tu respuesta con ejemplos concretos.
  
**Momentos en los que se usa random y que hacen**

```js
    noiseSeed(floor(random(10000)));
// En cada ejecución el patrón es único

    this.vel = p5.Vector.random2D().mult(0.5);
// todas las raices son distintas porque empiezan con una nueva velocidad

    this.baseColor = lerpColor(currentPalette.c1, currentPalette.c2, random(1));
// Cada color es distinto ligeramente porque es una mezcla de los colores bases de cada paleta que tenemos
```

**Perfil profesional**

- Según mi perfil como diseñadora de experiencias interactivas, este factor de aleatoriedad hace que cada obra generativa que hago tenga un elemento distintivo, tambien en cualquier experiencia que haga que sea digital, va a tener una interacción distinta, un usuario puede repetir la experiencia mil veces y asi sea por un color u orden de cosas, la experiencia cambiara significativamente.
  Ejemplo: En el caso de las experiencias que hice anteriormente, como la de la pasarela interactiva, los tamaños de las estrellas eran aleatorios, entonces aun asi hiciera mini explosiones, cada una iba a tener una magnitud/tamaño distinto en conjunto.


### Actividad 3: 

Realiza el siguiente experimento y reporta los resultados en tu bitácora:

-Modifica el código del ejemplo Example 0.1: A Traditional Random Walk.
Modifiqué el random para que vaya en un rango de 0 a 5
-Antes de ejecutar el código, escribe en tu bitácora qué esperas que suceda.
  Esperaria que ya no tenga una distribuicion tan aleatoria, ya no puede ir a valores menores a 0, por lo cual esperaría que tenga un crecimiento hacia arriba y no en el mismo rango en y.
-Ejecuta el código y escribe en tu bitácora qué sucedió realmente.
  Efectivamente tuvo una tendencia hacia arriba, casi lineal con variaciones en X
-Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?

  <img width="363" height="147" alt="image" src="https://github.com/user-attachments/assets/8e5f7748-1c8f-4d2f-b6d9-e4f1ec385c02" />
  
  Si ocurrio lo que esperaba, creria yo que fue debido al rango que definí, si fueran numeros menores, o un rango mas pequeño creo que es posible que tenga un movimiento mas limitado, por ejemplo si pusiera un rango de 2,4 solo habria movimiento en 3, lo cual es recto hacia arriba.

### Actividad 4: 

- En tus propias palabras cuál es la diferencia entre una distribución uniforme y una no uniforme de números aleatorios.
- En una distribuicion uniforme todos los números tienen la misma probabilidad de salir, en una distribuición no uniforme la probabilidad es distinta, un numero puede salir más veces que otro
- Modifica el código de la caminata aleatoria para que utilice una distribución no uniforme, favoreciendo el movimiento hacia la derecha.
- Usé un randomGaussian limitandolo a que el maximo sea 0 para que no se vaya hacia abajo

  <img width="280" height="209" alt="image" src="https://github.com/user-attachments/assets/3da87bba-6f49-45ec-af82-e3cd0eba52a7" />

### Actividad 5: 

Una vez has entendido el concepto de distribución normal, vas a pensar en una nueva manera de visualizarlo.

Crea un nuevo sketch en p5.js que represente una distribución normal.

- Copia el código en tu bitácora:

  ```js
  let puntos = [];

  function setup() {
  createCanvas(600, 400);
  background(255);
  }

  function draw() {
 
  for (let i = 0; i < 10; i++) {
    let x = randomGaussian(width / 2, 60); 
    let y = 0; 
    puntos.push({ x: x, y: y });
  }

  noStroke();
  fill(255, 105, 180, 50); 
  for (let i = 0; i < puntos.length; i++) {
    let p = puntos[i];
    ellipse(p.x, p.y, 4, 4);
    p.y += 1.5; 
  }

  puntos = puntos.filter(p => p.y < height);
  }
``

- Coloca en enlace a tu sketch en p5.js en tu bitácora: https://editor.p5js.org/luciana.gp0531/sketches/2yXrNv9QP


- Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora:

  <img width="589" height="398" alt="image" src="https://github.com/user-attachments/assets/15930438-6b4a-4bd2-9f63-8567d7932518" />

Se puede ver la forma de la distribucion gaussiana, lo hice en forma de lluvia para que podamos ver la campana y como hay una concentración en el centro y mas ausencia en las esquinas.


### Actividad 6:
- Crea un nuevo sketch en p5.js donde modifiques uno de los ejemplos anteriores y adiciones de Lévy flight.
  Use de referencia el ejemplo de randomGaussian de la pagina: <img width="1150" height="534" alt="image" src="https://github.com/user-attachments/assets/02b44504-ad49-40bd-9e2c-79a22da4707a" />

- Explica por qué usaste esta técnica y qué resultados esberabas obtener.
  Pues esta tecnia hace que una variable este entre varios valores pero con una probabilidad muy pequeña de que salga es mas grande, por lo cual use el diametro para ver la variacion de una forma mas marcada y facil de entender.
- Copia el código en tu bitácora.
```js
function setup() {
  createCanvas(640, 240);
  background(255);
}

function draw() {
  let x = randomGaussian(320, 60);
  let y = 120;

  let r = random(1);
  let diametro;

  if (r < 0.01) {
    // 1% de probabilidad del circulito grande
    diametro = random(30, 80);
  } else {
    
    diametro = random(4, 12);
  }

  noStroke();
  fill(255, 105, 180, 20); 
  circle(x, y, diametro);
}

```
- Coloca en enlace a tu sketch en p5.js en tu bitácora: https://editor.p5js.org/luciana.gp0531/sketches/U51A2Ul_c
- Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.

<img width="1225" height="464" alt="image" src="https://github.com/user-attachments/assets/9702a8c8-56c9-4ade-9a20-7258706a4d26" />


### Actividad 7:
- Crea un nuevo sketch en p5.js donde los visualices.
  
- Explica el concepto qué resultados esberabas obtener.
  Continue con los circulos porque me parece más facil visualizar los cambios asi, hice un campo y espero poder ver como cambian lentamente de tamaño y de color, es un cambio fluido y no es tan abrupto gracias al perlin noise
- Copia el código en tu bitácora.
 ```js
  let t = 0;

function setup() {
  createCanvas(600, 400);
  noStroke();
}

function draw() {
  background(255);

  let gridSize = 20;
  let scale = 1; // Qué tan espaciado está el ruido
  t += 0.01; // Tiempo para animación suave

  for (let x = 0; x < width; x += gridSize) {
    for (let y = 0; y < height; y += gridSize) {
      // Calcular ruido en 3D (x, y, tiempo)
      let n = noise(x * scale, y * scale, t);

      // Mapear el valor de ruido a tamaño y color
      let size = map(n, 0, 1, 2, gridSize);
      let col = map(n, 0, 1, 100, 255);

      fill(255, col, 200); // tonos rosados claros
      ellipse(x + gridSize / 2, y + gridSize / 2, size, size);
    }
  }
}

  ```
- Coloca en enlace a tu sketch en p5.js en tu bitácora: https://editor.p5js.org/luciana.gp0531/sketches/h8ua7Bk5d
  
- Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.
  <img width="758" height="501" alt="image" src="https://github.com/user-attachments/assets/b4dd20d7-17a8-4301-9bc3-250ba0cb327b" />


