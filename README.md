
# ESP321-DHT32-LCD
Uso de ESP32 con Sensor DHT22 y Pantalla LCD

## Objetivo
El objetivo de esta práctica es aprender a utilizar la ESP32 en conjunto con el sensor de temperatura y humedad DHT22, así como integrar una pantalla LCD para mostrar las lecturas de temperatura y humedad de manera visual. Este ejercicio busca familiarizar al estudiante con la programación de sensores y la interfaz de usuario básica mediante una pantalla LCD.

##Material y equipo 
- Placa ESP32: Una microcontroladora de bajo costo con capacidades Wi-Fi y Bluetooth.
- Sensor DHT22: Sensor digital para medir temperatura y humedad.
- Pantalla LCD 20x4 con interfaz I2C: Pantalla de cristal líquido con un controlador I2C para mostrar información al usuario.
- Plataforma Wokwi: Herramienta de simulación de circuitos electrónicos que permite emular el comportamiento del código sin necesidad de hardware físico.
## Diagrama de conexión
Sensor DHT22: El pin de señal del sensor DHT22 está conectado al GPIO 15 de la ESP32.
Pantalla LCD: La pantalla LCD está conectada a la ESP32 usando el protocolo I2C, con la dirección I2C configurada a 0x27 y utilizando dos pines (SDA y SCL) para la comunicación.

![image](https://github.com/ZurielO/ESP321-DHT32-LCD/blob/main/imagen_2024-12-15_160728646.png)

## Descripción del código
El código está diseñado para leer los datos de temperatura y humedad desde el sensor DHT22 y mostrarlos en la pantalla LCD. Además, los datos también se envían al monitor serie.

#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4
const int DHT_PIN = 15;

DHTesp dhtSensor;

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {

  Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  lcd.init();
  lcd.backlight();

}

void loop() {

  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
//Serial.println("---");
  
  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  lcd.print("Wokwi Online IoT");

  delay(1000);
}


## Explicación del código:
- Librerías: Se incluyen las librerías DHTesp para la comunicación con el sensor DHT22 y LiquidCrystal_I2C para el manejo de la pantalla LCD.
- Configuración inicial: En la función setup(), se inicializa la comunicación serie, se configura el sensor DHT22 en el pin correspondiente y se prepara la pantalla LCD.
- Lectura de datos: En el ciclo loop(), el código obtiene los datos de temperatura y humedad del sensor utilizando getTempAndHumidity().
- Visualización de datos: Los datos de temperatura y humedad se muestran en el monitor serie y en la pantalla LCD. Se utiliza la función lcd.print() para mostrar la información en la pantalla LCD.
- Delay: Se establece un retardo de 1 segundo con delay(1000) entre las lecturas para evitar actualizaciones demasiado rápidas.
## Resultados 
Al ejecutar el código en Wokwi o en una configuración física, se espera observar los siguientes resultados:
En el monitor serie: Se imprimirán los valores de temperatura y humedad cada segundo.
Ejemplo de salida en el monitor serie:
![Texto alternativo](https://github.com/ZurielO/ESP321-DHT32-LCD/blob/main/imagen_2024-12-15_155637361.png).

En la pantalla LCD: Los valores de temperatura y humedad se actualizarán en la pantalla LCD cada segundo. Ejemplo de lo que se verá en la LCD:

![Texto alternativo](https://github.com/ZurielO/ESP321-DHT32-LCD/blob/main/imagen_2024-12-15_160000538.png).


![Texto alternativo](https://github.com/ZurielO/ESP321-DHT32-LCD/blob/main/imagen_2024-12-15_160928564.png).

## Conclusión
Esta práctica demuestra cómo integrar un sensor DHT22 y una pantalla LCD con la placa ESP32 para mostrar y visualizar datos de temperatura y humedad en tiempo real. A través de la simulación en Wokwi, se puede verificar la correcta lectura de los datos y su presentación tanto en el monitor serie como en la pantalla LCD. 
