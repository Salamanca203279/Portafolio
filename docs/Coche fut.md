# Coche Omnidireccional Controlado por Bluetooth (ESP32 + Xbox Controller)

>  Proyecto académico de control de movimiento y actuadores, integrando el ESP32 con comunicación Bluetooth y llantas omnidireccionales.
[Enlace directo](https://youtube.com/shorts/Kpeg7VFwfDc?si=IWb2UG_srBYI-B7H)

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

###  Software
- **IDE:** Arduino IDE (versión más reciente)  
- **Plataforma:** ESP32 Dev Module  
- **Librerías necesarias:**  
  - `BluetoothSerial.h`  
  - `Servo.h`  

###  Hardware
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
include "BluetoothSerial.h"
include <Servo.h>

BluetoothSerial SerialBT;

define ENA 25
define IN1 26
define IN2 27
define IN3 14
define IN4 12
define ENB 13

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
