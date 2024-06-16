# Practica 6
## Part A
### Codi en Línea
```cpp
#include <Arduino.h>
#include <SPI.h> 
#include <SD.h> 
File myFile; 
void setup() 
{ 
  Serial.begin(9600); 
  SPI.begin(18,19,23,5);
  Serial.print("Iniciando SD ..."); 
  if (!SD.begin(5)) { 
    Serial.println("No se pudo inicializar"); 
    return; 
  } 
  Serial.println("inicializacion exitosa"); 
  myFile = SD.open("archivo.txt");//abrimos  el archivo  
  if (myFile) { 
    Serial.println("archivo.txt:"); 
    while (myFile.available()) { 
Serial.write(myFile.read()); 
    } 
    myFile.close(); //cerramos el archivo 
  } else { 
    Serial.println("Error al abrir el archivo"); 
  } 
} 
void loop() 
{ 
}
```
### Explicació del codi
`1.Inclusió de llibreries`
```cpp
#include <Arduino.h>
#include <SPI.h>
#include <SD.h>
```
Aquí s'inclouen les llibreries, la primera es fa servir per fer servir les funcions bàsiques d'Arduino, la segona per fer servir la comunicació SPI, i l'última per poder treballar amb targetes SD.

`2.Variable`
```cpp
File myFile;
```
Es declara una variable tipús 'File' anomenada 'myFile' per poder tractar amb l'arxiu a la SD.

`3.Setup`
```cpp
void setup() {
  Serial.begin(9600);  // Inicia la comunicación serial a 9600 baudios.
  SPI.begin(18, 19, 23, 5);  // Inicializa el bus SPI con los pines GPIO 18 (SCK), 19 (MISO), 23 (MOSI) y 5 (CS).
  Serial.print("Iniciando SD ...");  // Imprime un mensaje indicando que se está iniciando la tarjeta SD.
  if (!SD.begin(5)) {  // Intenta inicializar la tarjeta SD usando el pin 5 para CS (Chip Select).
    Serial.println("No se pudo inicializar");  // Si falla, imprime un mensaje de error.
    return;  // Sale de la función setup.
  }
  Serial.println("inicializacion exitosa");  // Imprime un mensaje indicando que la tarjeta SD se ha inicializado con éxito.
  myFile = SD.open("archivo.txt");  // Intenta abrir un archivo llamado "archivo.txt" en la tarjeta SD.
  if (myFile) {  // Verifica si el archivo se abrió correctamente.
    Serial.println("archivo.txt:");  // Imprime el nombre del archivo en el monitor serial.
    while (myFile.available()) {  // Bucle que continúa mientras haya datos disponibles en el archivo.
      Serial.write(myFile.read());  // Lee un byte del archivo y lo escribe en el monitor serial.
    }
    myFile.close();  // Cierra el archivo.
  } else {
    Serial.println("Error al abrir el archivo");  // Imprime un mensaje de error si el archivo no pudo abrirse.
  }
}
```
En aquest cas la funció 'setup ja està explicada linea per linea al codi, el que fa és que el bus SPI intenta inicialitzar la SD, en cas d'aconseguir-ho abrirà un arxiu anomenat "archivo.txt", llegint-lo i enviant el seu contingut al monitor serial. En cas de fallar es mostrarà un missatge d'error al monitor serial.

`4.Loop`
```cpp
void loop() {
}
```
En aquest cas el loop està buit perquè l'objectiu del porgrama és el de inicialitzar la SD i fer la lectura de l'arxiu, cosa que ja es feta al setup i no es necesaria tornarla a fer

## Part B
### Codi en Línea
```cpp
#include <Arduino.h>
#include <SPI.h> 
#include <MFRC522.h> 
#define RST_PIN 9    //Pin 9 para el reset del RC522 
#define SS_PIN 10   //Pin 10 para el SS (SDA) del RC522 
MFRC522 mfrc522(SS_PIN, RST_PIN); //Creamos el objeto para el RC522 
void setup() { 
Serial.begin(9600); //Iniciamos la comunicación  serial 
SPI.begin();        //Iniciamos el Bus SPI 
mfrc522.PCD_Init(); // Iniciamos  el MFRC522 
Serial.println("Lectura del UID"); 
} 
void loop() { 
// Revisamos si hay nuevas tarjetas  presentes 
if ( mfrc522.PICC_IsNewCardPresent())  
        {   
//Seleccionamos una tarjeta 
            if ( mfrc522.PICC_ReadCardSerial())  
            { 
                  // Enviamos serialemente su UID 
                  Serial.print("Card UID:"); 
                  for (byte i = 0; i < mfrc522.uid.size; i++) { Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" 
                  : " "); 
                          Serial.print(mfrc522.uid.uidByte[i], HEX);    
                  }  
                  Serial.println(); 
                  // Terminamos la lectura de la tarjeta  actual 
                  mfrc522.PICC_HaltA();  
            }}}
```

### Explicació del codi
`1.Inclusió de llibreries`
```cpp
#include <Arduino.h>
#include <SPI.h>
#include <MFRC522.h>
```
