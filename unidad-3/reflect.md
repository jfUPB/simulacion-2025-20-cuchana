# Unidad 3


## 🤔 Fase: Reflect

### Actividad 11: 

Parte 1: recuperación de conocimiento (Retrieval Practice)

Escribe la ecuación vectorial de la segunda ley de Newton y explica cada uno de sus componentes.
F= m . a
F= Fuerza neta que actúa sobre el objeto
m= masa del objeto
a= aceleración resultante

¿Por qué es necesario multiplicar la aceleración por cero en cada frame del método update()?

La aceleración funciona como un “acumulador de fuerzas”.
Cada frame aplicamos fuerzas → estas se suman en la aceleración.
Si no la reseteamos a cero, las fuerzas de un frame seguirían sumándose indefinidamente. Esto nos asegura que en cada frame solo actúan las fuerzas aplicadas en ese instante 

Explica la diferencia entre paso por valor y paso por referencia cuando aplicamos fuerzas a un objeto.

- Paso por valor: se envía una copia del vector fuerza

- Paso por referencia: se envía la dirección en memoria del vector, si el objeto modifica la fuerza, también cambia el vector original.

¿Cuál es la diferencia conceptual entre modelar fuerzas (como fricción, gravedad) y simplemente definir algoritmos de aceleración?

- Modelar fuerzas: Imita la fisica real, como lo fue gravedad, friccion y viento, hace las cosas mas naturales

- Algoritmos de aceleracion: cambian directamente la aceleracion sin pensar en su origen fisico, aqui es mas rigido y menos expresivo
  
Parte 2: reflexión sobre tu proceso (Metacognición)

¿Qué fue lo más desafiante en la Actividad 10 (problema de los n-cuerpos)? ¿El concepto creativo, la implementación de las fuerzas o la integración de la interactividad?

N/A

¿Las fuerzas que modelaste produjeron el comportamiento que esperabas? Describe un momento “sorpresa” (esperado o inesperado) durante el desarrollo.

N/A

¿Cómo ha cambiado tu forma de pensar sobre la “física” en el arte generativo después de esta unidad?

Pues realmente me ayudo a tener mas luz con el tema de simular fuerzas para darle naturalidad a las obras, no es como tan regido por reglas preestablecidas

Si tuvieras una semana más, ¿Qué otras fuerzas te gustaría modelar o cómo mejorarías tu simulación del problema de los n-cuerpos?

Si tuviera una semana mas me gustaria poder completar las actividades de la unidad, como la 10 y meterle mas visuales a la actividad 9

### Actividad 12: 
No aplica porque mis compañeros no hicieron la actividad y yo tampoco

### Actividad 13: 

- Continuar: Las actividades de modelado de fuerzas.

- Dejar de hacer: Quizá algunos ejercicios muy introductorios de aceleración directa ya se sienten repetitivos después de un tiempo.

- Progresión conceptual: Sí, fue natural. Primero entender aceleración me dio control básico

- Conexión arte-física: Ahora veo que las leyes físicas no limitan, sino que expanden la creatividad. Me siento más cómoda “jugando” con ellas para generar comportamientos inesperados.

