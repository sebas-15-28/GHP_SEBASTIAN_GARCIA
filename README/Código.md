#include <Servo.h>

Servo servo;

// ----- Configuración de pines -----
const int trigPlato = 8;
const int echoPlato = 7;
const int trigNivel = 6;
const int echoNivel = 5;
const int ledVacio = 13;

// ----- Display 7 segmentos (ánodo común) -----
const int a = A0;
const int b = A1;
const int c = A2;
const int d = A3;
const int e = A4;
const int f = A5;
const int g = 10;

int porciones = 0;
bool cerealVacio = false;

// ----- Tabla de encendido (LOW = encendido, HIGH = apagado) -----
bool numeros[10][7] = {
  {0,0,0,0,0,0,1}, // 0
  {1,0,0,1,1,1,1}, // 1
  {0,0,1,0,0,1,0}, // 2
  {0,0,0,0,1,1,0}, // 3
  {1,0,0,1,1,0,0}, // 4
  {0,1,0,0,1,0,0}, // 5
  {0,1,0,0,0,0,0}, // 6
  {0,0,0,1,1,1,1}, // 7
  {0,0,0,0,0,0,0}, // 8
  {0,0,0,1,1,0,0}  // 9
};

void setup() {
  Serial.begin(9600);
  servo.attach(9);

  pinMode(trigPlato, OUTPUT);
  pinMode(echoPlato, INPUT);
  pinMode(trigNivel, OUTPUT);
  pinMode(echoNivel, INPUT);
  pinMode(ledVacio, OUTPUT);

  int pines[] = {a,b,c,d,e,f,g};
  for (int i = 0; i < 7; i++) {
    pinMode(pines[i], OUTPUT);
    digitalWrite(pines[i], HIGH); // segmentos apagados (ánodo común)
  }

  servo.write(0);
}

long medirDistancia(int trig, int echo) {
  digitalWrite(trig, LOW);
  delayMicroseconds(2);
  digitalWrite(trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long duracion = pulseIn(echo, HIGH);
  return duracion * 0.034 / 2; // cm
}

void loop() {
  long distPlato = medirDistancia(trigPlato, echoPlato);
  long distNivel = medirDistancia(trigNivel, echoNivel);

  // Si detecta plato y hay cereal
  if (distPlato < 10 && !cerealVacio) {
    servo.write(90);
    delay(2000);
    servo.write(0);
    porciones++;
    if (porciones > 9) porciones = 0;
    mostrarNumero(porciones);
  }

  // Si el nivel de cereal es bajo
  if (distNivel > 15) {
    cerealVacio = true;
    digitalWrite(ledVacio, HIGH);
  }

  // Si se rellena el cereal
  if (distNivel <= 15 && cerealVacio) {
    cerealVacio = false;
    digitalWrite(ledVacio, LOW);
  }

  delay(500);
}

// ----- Mostrar número en display -----
void mostrarNumero(int n) {
  int pines[] = {a,b,c,d,e,f,g};
  for (int i = 0; i < 7; i++) {
    digitalWrite(pines[i], numeros[n][i]);
  }
}


este es el código para el funcionamiento del proyecto, se espera ajustar los horarios de las comidas cuando pongamos en practica el código y montemos el proyecto para probarlo en mejores condiciones.

## funciones
El código tiene dos sensores, uno sirve para detectar el plato para dar la porción del cereal y el otro sensor detecta el nivel del cereal y cuando se acaba activa un led el cual indica el nivel bajo del cereal. También hay in display de 7 segmentos que muestra la cantidad de porciones (de 0 a 9).

## simulación
- circuito: https://www.tinkercad.com/things/4GXwHZDkvFE-dispensador-de-cereal?sharecode=JMRZ1f2Z-Og5uwinxwL4u6sTYCUCAqICaz3o69RdyNk

- modelo 3D: https://www.tinkercad.com/things/0IwALI6RvHm-terrific-uusam-rottis?sharecode=rdODFi4ct_GYfJd9VDQepKcM8vrAv0c641CD_v_9veI
