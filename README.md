# ASCII_simulated_movement
Código en Python, usando Pillow y time.sleep(), que genera una animación ASCII simulando movimiento. En este caso, representaremos un objeto simple (una estrella) que se mueve de izquierda a derecha:

objx: trasladar este sistema a un Arduino o un ESP32.
---

## Código básico con libreria Time-

### Código en **Python** solo con `time.sleep()` que genera una animación ASCII simulando movimiento. En este caso, representaremos un objeto simple (una estrella) que se mueve de izquierda a derecha:

```python
import time
import os

# Lista de fotogramas de la animación (cambia la posición de la estrella)
frames = [
    "     *     ",
    "      *    ",
    "       *   ",
    "      *    ",
    "     *     ",
    "    *      ",
    "   *       ",
    "    *      ",
]

# Función para mostrar la animación en bucle
def animate_ascii(frames, delay=0.2, loops=10):
    for _ in range(loops):
        for frame in frames:
            os.system("cls" if os.name == "nt" else "clear")  # Limpia la pantalla (Windows/Linux)
            print(frame)  # Muestra el fotograma actual
            time.sleep(delay)  # Pausa breve para el efecto de movimiento

# Ejecutar la animación
animate_ascii(frames)
```

### **Cómo funciona**
1. Creamos una lista de **fotogramas ASCII**, donde la estrella `*` cambia de posición en cada iteración.
2. La función `animate_ascii()` imprime los fotogramas en pantalla con una pausa (`time.sleep()`), generando la sensación de movimiento.
3. Se usa `os.system("cls")` (para Windows) y `os.system("clear")` (para Linux) para limpiar la pantalla antes de mostrar el siguiente fotograma, logrando una animación fluida.

Puedes modificar los caracteres ASCII para representar por ejemplo un rostro sonriendo, un **gato durmiendo**, una **persona bailando** o cualquier otra cosa que quieras animar. 🚀🔤
