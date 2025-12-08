#  Control de LED y Actuadores con ESP32

##  1) Resumen

- **Nombre del proyecto:** Control de LED y Actuadores con ESP32  
- **Autor:** Eduardo Salamanca  
- **Curso / Asignatura:**  Introducción a ala mecatrónica
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

##  3) Alcance y Exclusiones

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
https://youtube.com/shorts/uz_2YryQJPw?si=gdgGDYm6nZkS1PoI

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

https://youtube.com/shorts/d5sxEI43tqk?si=GdzI6c9nHzy7hzp4
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

https://youtube.com/shorts/a2bWCtpGgGk?si=q9Ydl6QjXMjFGHZ1

https://youtube.com/shorts/nBANxXbmY7s?si=MIo1eOWCzVp5HbND

https://youtu.be/kR5TwPrPkgA?si=2ifdoAEf_vBL3Dr5
---
#  Proyecto 2 – Coche Omnidireccional Controlado por Bluetooth (ESP32 + Xbox Controller)

>  Proyecto académico de control de movimiento y actuadores, integrando el ESP32 con comunicación Bluetooth y llantas omnidireccionales.

---

## 1️ Resumen

- **Nombre del proyecto:** Coche de Futbol Bluetooth con ESP32  
- **Equipo / Autor(es):** Eduardo Salamanca y equipo  
- **Curso / Asignatura:** Electrónica y Programación de Microcontroladores  
- **Fecha:** 20/10/2025  
- **Descripción breve:**  
  Este proyecto implementa un coche omnidireccional controlado mediante Bluetooth, usando un control de Xbox y un ESP32.  
  El objetivo final fue participar en partidos tipo fútbol con los coches controlados remotamente.

---

## 2️ Objetivos

- **General:**  
  Diseñar e implementar un sistema móvil controlado por Bluetooth que permita controlar la dirección, velocidad y rotación mediante un control inalámbrico.

- **Específicos:**  
  - Implementar el control de movimiento con llantas omnidireccionales.  
  - Integrar puentes H para controlar los motores.  
  - Programar el ESP32 para recibir señales Bluetooth del control.  
  - Diseñar la carcasa del coche en MDF mediante corte láser.  
  - Incorporar el control de servomotores y LED del proyecto anterior.  

---

## 3️ Alcance y Exclusiones

- **Incluye:**  
  - Control inalámbrico con Bluetooth.  
  - Control direccional completo (adelante, atrás, izquierda, derecha, giro).  
  - Control de velocidad variable.  
  - Estructura mecánica cortada en MDF.  
  - Integración de LEDs y servomotores del proyecto anterior.  

- **No incluye:**  
  - Control por Wi-Fi o conexión a Internet.  
  - Sensores de distancia o evitación de obstáculos (versión futura).  

---

## 4️ Requisitos

### 🧠 Software
- **IDE:** Arduino IDE (versión más reciente)  
- **Plataforma:** ESP32 Dev Module  
- **Librerías necesarias:**  
  - `BluetoothSerial.h`  
  - `Servo.h`  

### ⚙️ Hardware
- ESP32  
- 4 llantas omnidireccionales  
- 2 puentes H L298N  
- Servomotor SG90 (para dirección o accesorio)  
- Jumpers y cables Dupont  
- Fuente de alimentación 12 V  
- Carcasa de MDF cortada con láser  
- Control Xbox (Bluetooth)  
- Protoboard  

---

## 5️ Código Principal (ESP32)

```cpp
#include "BluetoothSerial.h"
#include <Servo.h>

BluetoothSerial SerialBT;

#define ENA 25
#define IN1 26
#define IN2 27
#define IN3 14
#define IN4 12
#define ENB 13

Servo servo;

char comando;

void setup() {
  Serial.begin(115200);
  SerialBT.begin("Coche_BT_ESP32");
  servo.attach(15);
  
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  
  Serial.println("Esperando conexión Bluetooth...");
}

void loop() {
  if (SerialBT.available()) {
    comando = SerialBT.read();
    Serial.print("Comando recibido: ");
    Serial.println(comando);

    switch (comando) {
      case 'F':  // Adelante
        digitalWrite(IN1, HIGH);
        digitalWrite(IN2, LOW);
        digitalWrite(IN3, HIGH);
        digitalWrite(IN4, LOW);
        analogWrite(ENA, 255);
        analogWrite(ENB, 255);
        break;

      case 'B':  // Atrás
        digitalWrite(IN1, LOW);
        digitalWrite(IN2, HIGH);
        digitalWrite(IN3, LOW);
        digitalWrite(IN4, HIGH);
        analogWrite(ENA, 255);
        analogWrite(ENB, 255);
        break;

      case 'L':  // Izquierda
        digitalWrite(IN1, LOW);
        digitalWrite(IN2, HIGH);
        digitalWrite(IN3, HIGH);
        digitalWrite(IN4, LOW);
        break;

      case 'R':  // Derecha
        digitalWrite(IN1, HIGH);
        digitalWrite(IN2, LOW);
        digitalWrite(IN3, LOW);
        digitalWrite(IN4, HIGH);
        break;

      case 'S':  // Stop
        digitalWrite(IN1, LOW);
        digitalWrite(IN2, LOW);
        digitalWrite(IN3, LOW);
        digitalWrite(IN4, LOW);
        break;

      case 'V':  // Servo: variación de ángulo
        for (int i = 0; i <= 180; i += 10) {
          servo.write(i);
          delay(50);
        }
        for (int i = 180; i >= 0; i -= 10) {
          servo.write(i);
          delay(50);
        }
        break;
    }
  }
}
