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


---

## Usando Pillow: Ejemplo que convierte una imagen en caracteres ASCII para construir una animación:

Ahora vamos a aprovechar `Pillow` para manipular imágenes y generar arte ASCII dinámico. Este ejemplo convierte una imagen en caracteres ASCII y luego construye con varias de ellas en una animación:

```python
from PIL import Image
import time
import os

# Convertir una imagen en ASCII
def image_to_ascii(image_path, width=40, height=20):
    chars = "@%#*+=-:. "  # Conjunto de caracteres para representar distintos niveles de brillo
    img = Image.open(image_path).convert("L")  # Convertir la imagen a escala de grises
    img = img.resize((width, height))  # Redimensionar imagen

    ascii_art = ""
    for y in range(img.height):
        for x in range(img.width):
            pixel_brightness = img.getpixel((x, y)) // (255 // (len(chars) - 1))
            ascii_art += chars[pixel_brightness]
        ascii_art += "\n"
    return ascii_art

# Lista de imágenes a animar (deberías tener una serie de imágenes modificadas)
frames = ["frame1.png", "frame2.png", "frame3.png", "frame4.png"]  # Imágenes de la animación

# Función para mostrar la animación en bucle
def animate_ascii(frames, delay=0.2, loops=10):
    for _ in range(loops):
        for frame in frames:
            os.system("cls" if os.name == "nt" else "clear")  # Limpia la pantalla
            print(image_to_ascii(frame))  # Convierte y muestra cada imagen en ASCII
            time.sleep(delay)  # Pausa para simular movimiento

# Ejecutar la animación
animate_ascii(frames)
```

### **¿Cómo funciona?**
1. **`Pillow` convierte cada imagen en escala de grises y en caracteres ASCII.**
2. **Cada imagen actúa como un fotograma**, generando movimiento cuando se muestran en secuencia.
3. **Se limpia la pantalla antes de mostrar cada fotograma**, creando la ilusión de animación.

😃Si quieres hacer un GIF ASCII para mostrarlo en **Arduino** o un ESP32, puedes generar una serie de imágenes ASCII y mostrarlas secuencialmente en la pantall para verlas con movimiento. 🚀
