# unidad3-sfi


# **Trayecto de Actividades**

Este repositorio contiene un conjunto de ejercicios para profundizar en los protocolos binarios y la comunicación serial utilizando Arduino y C#. A continuación se detalla cada ejercicio, sus objetivos, y las preguntas clave que guían el aprendizaje.

## **Ejercicio 1: Introducción a los Protocolos Binarios - Caso de Estudio**

Este ejercicio introduce la estructura y funcionamiento de un protocolo binario a través del análisis de un sensor comercial. Puedes consultar la hoja de datos del sensor en [este enlace](http://www.chafon.com/productdetails.aspx?pid=382).

### **Preguntas:**
- **¿Cómo se ve un protocolo binario?**
  Un protocolo binario es un conjunto de reglas que definen la estructura de los mensajes transmitidos en un formato binario, es decir, en secuencias de bits (0s y 1s). Cada parte del mensaje está definida por un campo con un significado específico, como dirección, datos, o códigos de verificación.

- **¿Puedes describir las partes de un mensaje?**
  En el protocolo del sensor, un mensaje binario típico está compuesto por:
  - **Encabezado (Header)**: Indica el inicio del mensaje y puede incluir la dirección del dispositivo receptor.
  - **Datos (Payload)**: La información que se desea transmitir, como los valores de los sensores.
  - **Checksum**: Un valor calculado a partir de los datos, que permite verificar la integridad del mensaje.

- **¿Para qué sirve cada parte del mensaje?**
  - **Encabezado**: Sirve para identificar el tipo de mensaje y a quién está dirigido.
  - **Datos**: Contiene la información relevante, como los valores de los sensores.
  - **Checksum**: Permite verificar que el mensaje no ha sido alterado o corrompido durante la transmisión.

## **Ejercicio 2: Experimento**

En este ejercicio, se exploran los métodos esenciales para la comunicación serial en Arduino:

- `Serial.available()`: Indica cuántos bytes hay disponibles para ser leídos.
- `Serial.read()`: Lee el siguiente byte disponible en el buffer serial.
- `Serial.readBytes(buffer, length)`: Lee una cantidad específica de bytes y los almacena en un buffer.
- `Serial.write()`: Envía datos desde el Arduino hacia otro dispositivo a través de la conexión serial.

El método `Serial.readBytesUntil()` no se utiliza en protocolos binarios, ya que estos no suelen tener un carácter de fin de mensaje, como ocurre en los protocolos ASCII (e.g. `\n`).

## **Ejercicio 3: ¿Qué es el *Endian*?**

Cuando trabajamos con números que ocupan más de un byte, como los números en punto flotante según el estándar [IEEE754](https://www.h-schmidt.net/FloatConverter/IEEE754.html), es necesario decidir el orden de transmisión de sus bytes. Existen dos órdenes posibles:

- **Little Endian**: Se transmite primero el byte de menor peso (menos significativo).
- **Big Endian**: Se transmite primero el byte de mayor peso (más significativo).

## **Ejercicio 4: Transmitir Números en Punto Flotante**

Aquí se muestra cómo transmitir un número en punto flotante en formato binario usando Arduino.

### **Opción 1:**
En este ejemplo, el número `3589.3645` se transmite tal como está en el formato *Little Endian*.
```arduino
void setup() {
    Serial.begin(115200);
}

void loop() {
    static float num = 3589.3645;

    if(Serial.available()){
        if(Serial.read() == 's'){
            Serial.write((uint8_t *) &num, 4);
        }
    }
}

