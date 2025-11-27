#include <WiFiS3.h>
#include <WiFiUdp.h>
#include <NTPClient.h>
#include <Servo.h>

// ======= CONFIGURACIONES =======
const char* ssid     = "RED_WIFI";      
const char* password = "CONTRASEÑA";    

// Horarios de alimentación (por defecto)
int horarios[][2] = {
  {8,  0},   // 08:00
  {14, 0},   // 14:00
  {20, 0}    // 20:00
};
int totalHorarios = 3;

// Tiempo que la compuerta permanece abierta (en segundos)
const int tiempoAbierto = 10;

// Servo
const int pinServo = 9;
const int posCerrado = 0;
const int posAbierto  = 90;
Servo servoAlimentador;

// Control de hora
WiFiUDP ntpUDP;
NTPClient clienteTiempo(ntpUDP, "pool.ntp.org", -5 * 3600); 

// Para evitar múltiples activaciones en el mismo minuto
int ultimoMinutoActivado = -1;

// Bluetooth
#include <SoftwareSerial.h>
SoftwareSerial BT(10, 11); // RX, TX del Arduino

void setup() {
  Serial.begin(9600);
  BT.begin(9600);

  // Conectar al WiFi
  Serial.print("Conectando a WiFi: ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\n¡WiFi conectado!");

  // Iniciar cliente NTP
  clienteTiempo.begin();

  // Configurar el servo
  servoAlimentador.attach(pinServo);
  servoAlimentador.write(posCerrado);
  Serial.println("Alimentador automático iniciado.");
}

void loop() {
  clienteTiempo.update();
  int hora = clienteTiempo.getHours();
  int minuto = clienteTiempo.getMinutes();

  // Verificar cada horario configurado
  for (int i = 0; i < totalHorarios; i++) {
    if (hora == horarios[i][0] && minuto == horarios[i][1]) {
      if (minuto != ultimoMinutoActivado) {
        liberarComida();
        ultimoMinutoActivado = minuto;
      }
    }
  }

  // Revisar comandos por Bluetooth
  recibirComandoBluetooth();

  delay(1000);
}

// Función para abrir y cerrar la compuerta
void liberarComida() {
  Serial.println("Liberando comida...");
  servoAlimentador.write(posAbierto);
  delay(tiempoAbierto * 1000);
  servoAlimentador.write(posCerrado);
  Serial.println("Compuerta cerrada.");
}

// Función para recibir y procesar comandos Bluetooth
void recibirComandoBluetooth() {
  if (BT.available()) {
    String comando = BT.readStringUntil('\n');
    comando.trim(); // eliminar espacios al inicio y final

    // Verificar formato H1=HH:MM
    if (comando.startsWith("H")) {
      int indice = comando.charAt(1) - '1'; // H1 -> 0, H2 -> 1, H3 -> 2
      int posIgual = comando.indexOf('=');
      int posDosPuntos = comando.indexOf(':');

      if (indice >= 0 && indice < totalHorarios && posIgual > 0 && posDosPuntos > posIgual) {
        int nuevaHora = comando.substring(posIgual + 1, posDosPuntos).toInt();
        int nuevoMinuto = comando.substring(posDosPuntos + 1).toInt();

        horarios[indice][0] = nuevaHora;
        horarios[indice][1] = nuevoMinuto;

        BT.println("Horario H" + String(indice + 1) + " actualizado a " +
                   String(nuevaHora) + ":" + String(nuevoMinuto));
        Serial.println("Horario H" + String(indice + 1) + " actualizado a " +
                       String(nuevaHora) + ":" + String(nuevoMinuto));
      }
    }
  }
}


este es el código para el funcionamiento del proyecto, se espera ajustar los horarios de las comidas cuando pongamos en practica el código y montemos el proyecto para probarlo en mejores condiciones.

## funcionamiento
1. El Arduino se conecta al WIFI para obtener la hora por NTP
2. E modulo Bluetooth espera comandos desde el celular
3. los horarios para cambiar los horarios se pondrán así: 
H1=08:30
H2=14:00
H3=20:00
4. El Arduino actualiza los horarios en la memoria
5. cuando llega la hora, se activa el servo

## conexiones 

HC-06      Arduino UNO R4 WiFi
VCC    →    5V
GND   →    GND
TX      →    Pin 10 (RX SoftwareSerial)
RX      →    Pin 11 (TX SoftwareSerial) → con divisor de voltaje
- 1kΩ entre TX de Arduino y RX HC-06
- 2kΩ entre RX HC-06 y GND