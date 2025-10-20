# 💡 Control de LED y Actuadores con ESP32

##  1) Resumen

- **Nombre del proyecto:** Control de LED y Actuadores con ESP32  
- **Autor:** Eduardo Salamanca  
- **Curso / Asignatura:** Electrónica / Sistemas Embebidos / IoT  
- **Fecha:** 20/10/2025  
- **Descripción breve:**  
  Este proyecto combina dos prácticas de control básico con el microcontrolador ESP32:  
  1. Encendido de un LED mediante Bluetooth.  
  2. Manejo de actuadores (motor DC y servomotor) con variación de giro, velocidad y ángulo.  
  Ambas actividades introducen conceptos esenciales de control digital, PWM y comunicación inalámbrica.

---

## 2) Objetivos

- **General:**  
  Implementar el control de periféricos y actuadores utilizando el ESP32, aplicando principios de electrónica digital, PWM y comunicación Bluetooth.

- **Específicos:**
  - Configurar el entorno de desarrollo y los pines de salida del ESP32.  
  - Controlar un LED desde una aplicación móvil por Bluetooth.  
  - Manejar un motor DC con control de giro y velocidad usando PWM.  
  - Controlar un servomotor en distintos ángulos mediante señales PWM.  
  - Comprender la relación entre hardware, programación y respuesta física del sistema.

---

## ⚙️ 3) Alcance y Exclusiones

- **Incluye:**
  - Control de LED por Bluetooth.  
  - Control de giro y velocidad de motor DC.  
  - Control de posición de servomotor (0° a 180°).  
  - Programación completa en Arduino IDE.

- **No incluye:**
  - Control automático por sensores.  
  - Control simultáneo de múltiples motores.  
  - Comunicación Wi-Fi o interfaces gráficas avanzadas.

---

##  4) Requisitos

### Software
- **Sistema operativo:** Windows / macOS / Linux  
- **IDE:** Arduino IDE 2.x  
- **Bibliotecas necesarias:**  
  - `BluetoothSerial.h` (control BT)  
  - `Servo.h` (control del servomotor)  

### Hardware
- ESP32 DevKit  
- LED + resistencia de 220 Ω  
- Motor DC     
- Servomotor   
- Fuente de alimentación externa  
- Protoboard y cables Dupont  

### Conocimientos previos
- Señales PWM y control de velocidad.  
- Manejo de pines digitales de salida.  
- Programación estructurada en Arduino.  
- Conexión de componentes electrónicos.

---

## 5) Instalación y Configuración

### A) Control de LED por Bluetooth

```cpp
#include "BluetoothSerial.h"
BluetoothSerial SerialBT;

int ledPin = 2;

void setup() {
  Serial.begin(115200);
  SerialBT.begin("ESP32_LED");
  pinMode(ledPin, OUTPUT);
}

void loop() {
  if (SerialBT.available()) {
    char data = SerialBT.read();
    if (data == '1') digitalWrite(ledPin, HIGH);
    if (data == '0') digitalWrite(ledPin, LOW);
  }
}
```

> **Comandos desde la app Bluetooth:**
> - `'1'` → Enciende el LED  
> - `'0'` → Apaga el LED  

---

### B) Control de motor DC (giro y velocidad)

```cpp
int ENA = 5;   // PWM (velocidad)
int IN1 = 18;  // Dirección
int IN2 = 19;  // Dirección

void setup() {
  pinMode(ENA, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
}

void loop() {
  // Giro en un sentido
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  analogWrite(ENA, 150); // Velocidad media
  delay(2000);

  // Aumentar velocidad
  analogWrite(ENA, 255);
  delay(2000);

  // Cambiar sentido
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  analogWrite(ENA, 200);
  delay(2000);

  // Detener
  analogWrite(ENA, 0);
  delay(2000);
}
```

---

### C) Control de servomotor

```cpp
#include <Servo.h>
Servo servoMotor;

void setup() {
  servoMotor.attach(13); // Pin PWM
}

void loop() {
  // Mover el servo entre varios ángulos
  servoMotor.write(0);
  delay(1000);
  servoMotor.write(90);
  delay(1000);
  servoMotor.write(180);
  delay(1000);
}
```

---

##  6) Resultados esperados

- El LED responde correctamente a los comandos Bluetooth.  
- El motor DC cambia de dirección y velocidad de manera controlada.  
- El servomotor se posiciona con precisión en los ángulos definidos.  
- Se comprende el uso del PWM en el control de velocidad y posición.

---

##  7) Conclusiones

- Se comprobó la versatilidad del ESP32 para controlar diferentes actuadores.  
- El uso de PWM permitió regular tanto la velocidad de motores como el ángulo de servos.  
- Se integraron conceptos de programación, electrónica y comunicación inalámbrica.  
- Este proyecto sienta la base para aplicaciones IoT más complejas con sensores y automatización.

---

##  8) Referencias

- [Documentación oficial del ESP32](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/)  
- [Referencia de la librería Servo en Arduino](https://www.arduino.cc/reference/en/libraries/servo/)  
- [Bluetooth Serial en ESP32 - Ejemplos](https://randomnerdtutorials.com/esp32-bluetooth-classic-arduino-ide/)  

---

