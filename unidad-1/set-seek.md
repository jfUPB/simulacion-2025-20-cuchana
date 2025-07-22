# Unidad 1

## 🔎 Fase: Set + Seek

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



