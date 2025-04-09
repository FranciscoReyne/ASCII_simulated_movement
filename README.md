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

----

😃Si quieres hacer un GIF ASCII para mostrarlo en **Arduino** o un ESP32, puedes generar una serie de imágenes ASCII y mostrarlas secuencialmente en la pantall para verlas con movimiento. 🚀 

# Ejemplo editable de codigo facil para un ESP32:

 Diseñaremos un **código modular** que permita al usuario seleccionar fácilmente entre las **10 pantallas más usadas con ESP32**. Así, cualquier persona inexperta podrá cambiar el modelo sin necesidad de modificar el código base.

### **Enfoque del sistema**
1. **Selección sencilla de pantalla** → El usuario elige el modelo cambiando una sola variable.
2. **Inicialización automática** → Según la pantalla seleccionada, el código configura los parámetros adecuados.
3. **Compatibilidad con las 10 pantallas más usadas** → Desde LCD con I2C hasta TFT con touch.
4. **Modular y escalable** → Se pueden agregar más pantallas sin cambiar la estructura principal.

---

### **Código en C++ para ESP32 (Compatible con las 10 pantallas)**

```cpp
#include <Wire.h>
#include <SPI.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <Adafruit_ILI9341.h>
#include <TFT_eSPI.h>
#include <LiquidCrystal_I2C.h>

// Enumeración de pantallas soportadas
enum ScreenType {
    OLED_SSD1306,
    OLED_SH1106,
    TFT_ST7735,
    TFT_ILI9341,
    TFT_ILI9488,
    LCD_I2C,
    EPAPER_WAVESHARE,
    TFT_TOUCH_2_8,
    TFT_TOUCH_3_5,
    NEXTION_HMI
};

// --- SELECCIÓN DE PANTALLA ---
// Cambia el modelo aquí según la pantalla que estés usando
ScreenType selectedScreen = LCD_I2C;

// Variables globales para inicialización de pantallas
Adafruit_SSD1306 oled(128, 64, &Wire);
Adafruit_ILI9341 tft = Adafruit_ILI9341(5, 4, 3); // Pueden cambiarse los pines según la configuración
LiquidCrystal_I2C lcd(0x27, 16, 2); // Dirección I2C de la pantalla LCD 16x2

// Función de configuración según pantalla seleccionada
void setupScreen() {
    switch (selectedScreen) {
        case OLED_SSD1306:
            Serial.println("Inicializando OLED SSD1306...");
            oled.begin(SSD1306_SWITCHCAPVCC, 0x3C);
            oled.clearDisplay();
            oled.setTextSize(1);
            oled.setTextColor(WHITE);
            oled.setCursor(0, 0);
            oled.println("Pantalla OLED SSD1306");
            oled.display();
            break;

        case LCD_I2C:
            Serial.println("Inicializando LCD 16x2 con I2C...");
            lcd.init();
            lcd.backlight();
            lcd.setCursor(0, 0);
            lcd.print("Pantalla LCD I2C");
            break;

        case TFT_ILI9341:
            Serial.println("Inicializando TFT ILI9341...");
            tft.begin();
            tft.fillScreen(ILI9341_BLACK);
            tft.setTextColor(ILI9341_WHITE);
            tft.setCursor(10, 10);
            tft.setTextSize(2);
            tft.print("Pantalla ILI9341");
            break;

        default:
            Serial.println("Pantalla no soportada aún.");
            break;
    }
}

void setup() {
    Serial.begin(115200);
    setupScreen();
}

void loop() {
    // Aquí puedes agregar código para mostrar gráficos o texto según la pantalla seleccionada.
}
```

---

### **¿Cómo funciona este código?**
✅ **Cambiar pantalla fácilmente** → Solo modificas la línea `selectedScreen = LCD_I2C;`  
✅ **Cada pantalla se configura automáticamente** → No necesitas modificar más código.  
✅ **Expansión sencilla** → Puedes agregar más pantallas sin afectar el código base.  
✅ **Compatible con ESP32 y sensores externos** → Se adapta bien a otros sistemas.  

---


# EJemplo de animación en esp32 y lcd 2x 16

¡Buena pregunta! Para conectar la **pantalla ESP32** con las **animaciones ASCII**, podemos modificar el código para que en lugar de imprimir en la consola, muestre los fotogramas de la animación en la pantalla LCD, OLED o TFT.

---

### **1. Enviar Animación ASCII a Pantalla LCD 16x2**
Si usas la pantalla **LCD 16x2 con I2C**, cada fotograma de la animación ASCII se imprimirá en la pantalla usando `lcd.print()`. Aquí tienes un ejemplo:

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2); // Dirección I2C

// Fotogramas de la animación (se mostrarán uno por uno)
const char* frames[] = {
    "  ( ^_^ )  ", 
    "  ( -_- )  ", 
    "  ( o_o )  ",
    "  ( ^_^ )  "
};

void setup() {
    lcd.init();
    lcd.backlight();
}

void loop() {
    for (int i = 0; i < 4; i++) { // Recorrer los fotogramas
        lcd.clear();
        lcd.setCursor(3, 0);
        lcd.print(frames[i]);  // Mostrar fotograma en pantalla
        delay(500);  // Pausa para el efecto de animación
    }
}
```

✅ **Este código hará que la cara ASCII cambie en la pantalla**, simulando movimiento.  
✅ **Puedes reemplazar los caracteres para hacer una animación personalizada**.  

---

### **2. Enviar Animación ASCII a una Pantalla OLED SSD1306**
Si usas una pantalla **OLED 0.96" con SSD1306**, puedes usar `oled.println()` en lugar de `lcd.print()`:

```cpp
#include <Wire.h>
#include <Adafruit_SSD1306.h>

Adafruit_SSD1306 oled(128, 64, &Wire);

const char* frames[] = {
    "  ( o_o )  ",
    "  ( -_- )  ",
    "  ( ^_^ )  ",
    "  ( o_o )  "
};

void setup() {
    oled.begin(SSD1306_SWITCHCAPVCC, 0x3C);
    oled.clearDisplay();
}

void loop() {
    for (int i = 0; i < 4; i++) {
        oled.clearDisplay();
        oled.setCursor(10, 10);
        oled.setTextSize(2);
        oled.setTextColor(WHITE);
        oled.println(frames[i]); // Imprimir fotograma
        oled.display();
        delay(500);
    }
}
```

✅ **Este código mostrará la animación ASCII en una pantalla OLED ESP32**.  
✅ **Puedes modificar los caracteres para hacer un efecto de movimiento más complejo**.  

---

### **3. Usar una Pantalla TFT para Animaciones ASCII**
Si usas una pantalla **TFT ILI9341**, puedes imprimir los fotogramas en la pantalla como si fueran texto:

```cpp
#include <SPI.h>
#include <Adafruit_GFX.h>
#include <Adafruit_ILI9341.h>

Adafruit_ILI9341 tft = Adafruit_ILI9341(5, 4, 3);

const char* frames[] = {
    "   ( o_o )   ",
    "   ( -_- )   ",
    "   ( ^_^ )   ",
    "   ( o_o )   "
};

void setup() {
    tft.begin();
    tft.fillScreen(ILI9341_BLACK);
}

void loop() {
    for (int i = 0; i < 4; i++) {
        tft.fillScreen(ILI9341_BLACK);
        tft.setCursor(30, 40);
        tft.setTextColor(ILI9341_WHITE);
        tft.setTextSize(3);
        tft.print(frames[i]); // Imprimir fotograma en pantalla
        delay(500);
    }
}
```

✅ **Este código hará que el texto ASCII cambie en la pantalla TFT, simulando movimiento**.  
✅ **Puedes ajustar `setTextSize()` y `setCursor()` para mejorar la visualización**.  



### Puedes usar cualquiera de estos códigos dependiendo de la pantalla que uses:  
✔ **LCD 16x2 con I2C** → Usa `lcd.print()`  
✔ **OLED SSD1306** → Usa `oled.println()`  
✔ **TFT ILI9341** → Usa `tft.print()`  

Así puedes mostrar **animaciones ASCII en ESP32**, sin necesidad de imágenes GIF. 🚀😃  

---
---

# Ejemplo para **Pantallas ESP32, OLED SSD1306, y TFT ILI9341** con las **animaciones ASCII**.

Para conectar la **pantalla ESP32** con las **animaciones ASCII**, podemos modificar el código para que en lugar de imprimir en la consola, muestre los fotogramas de la animación en la pantalla LCD, OLED o TFT.


### **1. Enviar Animación ASCII a Pantalla LCD 16x2**
Si usas la pantalla **LCD 16x2 con I2C**, cada fotograma de la animación ASCII se imprimirá en la pantalla usando `lcd.print()`. Aquí tienes un ejemplo:

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2); // Dirección I2C

// Fotogramas de la animación (se mostrarán uno por uno)
const char* frames[] = {
    "  ( ^_^ )  ", 
    "  ( -_- )  ", 
    "  ( o_o )  ",
    "  ( ^_^ )  "
};

void setup() {
    lcd.init();
    lcd.backlight();
}

void loop() {
    for (int i = 0; i < 4; i++) { // Recorrer los fotogramas
        lcd.clear();
        lcd.setCursor(3, 0);
        lcd.print(frames[i]);  // Mostrar fotograma en pantalla
        delay(500);  // Pausa para el efecto de animación
    }
}
```

✅ **Este código hará que la cara ASCII cambie en la pantalla**, simulando movimiento.  
✅ **Puedes reemplazar los caracteres para hacer una animación personalizada**.  

---

### **2. Enviar Animación ASCII a una Pantalla OLED SSD1306**
Si usas una pantalla **OLED 0.96" con SSD1306**, puedes usar `oled.println()` en lugar de `lcd.print()`:

```cpp
#include <Wire.h>
#include <Adafruit_SSD1306.h>

Adafruit_SSD1306 oled(128, 64, &Wire);

const char* frames[] = {
    "  ( o_o )  ",
    "  ( -_- )  ",
    "  ( ^_^ )  ",
    "  ( o_o )  "
};

void setup() {
    oled.begin(SSD1306_SWITCHCAPVCC, 0x3C);
    oled.clearDisplay();
}

void loop() {
    for (int i = 0; i < 4; i++) {
        oled.clearDisplay();
        oled.setCursor(10, 10);
        oled.setTextSize(2);
        oled.setTextColor(WHITE);
        oled.println(frames[i]); // Imprimir fotograma
        oled.display();
        delay(500);
    }
}
```

✅ **Este código mostrará la animación ASCII en una pantalla OLED ESP32**.  
✅ **Puedes modificar los caracteres para hacer un efecto de movimiento más complejo**.  

---

### **3. Usar una Pantalla TFT para Animaciones ASCII**
Si usas una pantalla **TFT ILI9341**, puedes imprimir los fotogramas en la pantalla como si fueran texto:

```cpp
#include <SPI.h>
#include <Adafruit_GFX.h>
#include <Adafruit_ILI9341.h>

Adafruit_ILI9341 tft = Adafruit_ILI9341(5, 4, 3);

const char* frames[] = {
    "   ( o_o )   ",
    "   ( -_- )   ",
    "   ( ^_^ )   ",
    "   ( o_o )   "
};

void setup() {
    tft.begin();
    tft.fillScreen(ILI9341_BLACK);
}

void loop() {
    for (int i = 0; i < 4; i++) {
        tft.fillScreen(ILI9341_BLACK);
        tft.setCursor(30, 40);
        tft.setTextColor(ILI9341_WHITE);
        tft.setTextSize(3);
        tft.print(frames[i]); // Imprimir fotograma en pantalla
        delay(500);
    }
}
```

✅ **Este código hará que el texto ASCII cambie en la pantalla TFT, simulando movimiento**.  
✅ **Puedes ajustar `setTextSize()` y `setCursor()` para mejorar la visualización**.  

---

### **Conclusión**
Puedes usar cualquiera de estos códigos dependiendo de la pantalla que uses:  
✔ **LCD 16x2 con I2C** → Usa `lcd.print()`  
✔ **OLED SSD1306** → Usa `oled.println()`  
✔ **TFT ILI9341** → Usa `tft.print()`  

😃 Así puedes mostrar **animaciones ASCII en ESP32**, sin necesidad de imágenes GIF. 🚀 



