# PROYECTO_FINAL_DISPENSADOR
Este es un proyecto de la materia de metodologia de desarrollo de sofware, este proyecto va a tratar sobre la automatizacion de un dispensador de cereales. EL dispensador funcionara con un boton o desde una aplicacion, el dispensador muestra si el dispensador esta lleno, a nivel medio o cuando esta vacio.
#Objetivo general
Diseñar un sistema automatizado sobre un dispensador de ceral atravez de un boton o aplicion, tambien mostrara la cantidad de cereal en el dispensador. Asi haciendolo mas facil para niños.
#Historias de usuario
# H-1
Nombre: Detección de recipiente 
Descripción: Como desarrolladores, queremos que el dispensador detecte si hay un 
recipiente colocado, para evitar que los cereales caigan directamente al suelo. 
Valor de negocio: 10 
Esfuerzo: 12 
Prioridad: Must
# H-2
Nombre: Estimación de peso en tiempo real 
Descripción: Como desarrolladores, queremos que el sistema muestre el peso de los 
cereales dispensados en tiempo real, para que el usuario controle la cantidad servida. 
Valor de negocio: 9 
Esfuerzo: 15 
Prioridad: Must 
# H-3
Nombre: Dosificación automática 
Descripción: Como desarrolladores, queremos que el dispensador pueda detenerse 
automáticamente al alcanzar un peso configurado, para evitar servir de más. 
Valor de negocio: 10 
Esfuerzo: 20 
Prioridad: Must 
# H-4
Nombre: Visualización en pantalla 
Descripción: Como desarrolladores, queremos que el usuario pueda ver en una pantalla el 
peso actual, la cantidad dispensada y las opciones de configuración, para facilitar el uso. 
Valor de negocio: 8 
Esfuerzo: 10 
Prioridad: Should
# H-5
Nombre: Notificación de bajo nivel de cereal 
Descripción: Como desarrolladores, queremos que el sistema emita una alerta cuando el 
contenedor de cereales esté por vaciarse, para que el usuario lo rellene a tiempo. 
Valor de negocio: 9 
Esfuerzo: 8 
Prioridad: Must 
# H-6
Nombre: Registro histórico de consumo 
Descripción: Como desarrolladores, queremos que el sistema almacene el historial de 
pesos dispensados, para analizar patrones de consumo de cereales. 
Valor de negocio: 6 
Esfuerzo: 7 
Prioridad: Could
# H-7
Nombre: Configuración de porciones 
Descripción: Como desarrolladores, queremos que el usuario pueda configurar tamaños de 
porción (ejemplo: pequeña, mediana, grande), para adaptar el uso a diferentes necesidades. 
Valor de negocio: 8 
Esfuerzo: 12 
Prioridad: Should 
# H-8
Nombre: Prueba de sensores de peso 
Descripción: Como desarrolladores, queremos ejecutar pruebas de calibración y 
funcionamiento de la celda de carga, para garantizar mediciones correctas del peso. 
Valor de negocio: 10 
Esfuerzo: 9 
Prioridad: Must 
# H-9
Nombre: Reporte de fallas en sensores 
Descripción: Como desarrolladores, queremos recibir una alerta si el sensor de peso 
presenta fallas o lecturas erróneas, para evitar errores en la dosificación. 
Valor de negocio: 8 
Esfuerzo: 11 
Prioridad: Must 
# H-10
Nombre: Conectividad y control remoto 
Descripción: Como desarrolladores, queremos que el dispensador se pueda controlar desde 
una app móvil, para servir cereales y revisar consumos a distancia. 
Valor de negocio: 7 
Esfuerzo: 20 
Prioridad: Could
# Diagrama de clases
![diagrama de clases](imagenes/diagrama_clases.png)
# Diagramas de flujo
![Diagrama de flujo]imagen
# Dispensador
El dispensador es llenado manualmente y este muestra cuanta cantidad de cereal hay si esta lleno, medio o vacio. Este funciona con un boton o aplicacion.
# Entradas
C:cereal(1 si el dispensador esta en la midad o mas de cereal, 0 si esta por debajo de la midad) , B:boton(1 si se activa dispensa la porcion de cereal, 0 si no se activa el boton)
# Salidas
M:motor(1 se mueve y da la porcion de cereal, 0 no da la porcion de cereal) , L:led(1 indica si el dispensador esta por de bajo de la midad de cereal, 0 si esta por encima de la mitad)
# Casos de uso
# 1. Identificar actores 
-Niño/Cliente
 *Interactúa directamente con el dispensador 
 *Acciones: Oprime el botón para recibir su porción
-Padre/ Terapeuta
 *Interactúa de forma indirecta
 *Acciones: supervisa cuantas porcione ha recibido el niño/cliente y se encarga de abastecer el dispensador 
-Sistema de control del dispensador 
 *Encargado de gestionar el motor, el contador de porciones y el sensor de cereal 
 *Acciones: Válida si hay cereal, activa el motor, actualiza el contador de las porciones, genera mensajes de error 
# 2. Casos de uso
Dispensar porción de cereal
-Actor principal: Niño (usuario)
 *Objetivo: Obtener una porción de cereal de manera sencilla y segura.

-Flujo básico:
 *El niño coloca el recipiente en la base.
 *El niño presiona el botón.
 *El sistema verifica que haya cereal disponible.
 *El sistema libera la porción configurada.
 *El sistema muestra el peso y actualiza el contador.
 
 -Flujos alternativos:
 *Si no hay recipiente → el sistema bloquea la salida.
 *Si el contenedor está vacío → el sistema notifica al usuario.

Consultar contador de porciones 
-Actor principal: Padre/ Terapeuta 
 *Objetivo:Consultar el historial de consumo y el número de porciones servidas

-Flujo básico
 *El actor accede al sistema (pantalla o app externa) 
 *El sistema muestra el número de porciones consumidas y el registro histórico (fecha, hora, peso)

-Flujos alternativos
 *Si no hay registros → el sistema muestra contador en cero.

Manejar errores del sistema
-Actor principal: Sistema
 *Objetivo: Detectar y notificar fallas para mantener la seguridad y el buen funcionamiento.
-Flujo básico:
 *El sistema monitorea constantemente sensores y motor
 *Si detecta un error (atasco, vacío, sensor defectuoso):
  +Muestra un mensaje en pantalla o indicador visual.
  +Emite alerta sonora (si aplica).
  +Detiene el proceso hasta que se corrija la falla.

# 3. Diagrama de casos de uso
imagen

# 4. Narrativa de casos de uso
-Flujo básico:
 *El niño coloca un recipiente en la base del dispensador 
 *El niño presiona el botón para activar el dispensador
 *El sistema verifica que hay cereal en el contenedor 
 *El sistema libera la porción de cereal configurada
 *El sistema muestra el peso dispensado y el contador de porciones
 *El sistema guarda el registro de consumo con fecha, hora y peso

-Flujo alternativo:
 *Recipiente no detectado: el sistema bloquea la salida y alerta al usuario.
 *Contenedor vacío o bajo nivel: el sistema emite una notificación manual / sonora.
 *Error en sensores: el sistema muestra un error y detiene el dispensador.
 *Atasco en motor: el sistema cancela la acción y da una noticia de la falla.

-Relación con las historias de usuario:
 *H-1: Detección de recipiente
 *H-2 : Estimación de peso en tiempo real
 *H-3 : Dosificación automática
 *H-4 : visualización en la pantalla 
 *H-5 : Notificación de bajo nivel de cereal 
 *H-6 : Registro histórico de consumo 
 *H-7 : Configuración de porciones 
 *H-8 : Prueba de sensores de peso
 *H-9 : Reporte de fallas en sensores
 *H-10 : Control remoto por app( externa ) 

# 5. Requisitos no funcionales
-Seguridad  
 *El sistema debe ser seguro para niños, evitando piezas pequeñas o bordes cortantes que puedan representar un peligro.
 *El dispensador no debe liberar cereal si no detecta un recipiente en la base, evitando desperdicio o accidentes (H-1).
 *En caso de atasco en el motor o fallas en los sensores, el sistema debe detenerse automáticamente y mostrar una alerta al usuario (H-9).

-Usabilidad
 *El botón de activación debe ser grande, visible y fácil de presionar, de manera que pueda ser utilizado por niños sin dificultad.
 *La pantalla o indicador visual debe mostrar claramente el peso dispensado y el número de porciones en tiempo real (H-2, H-4).
 *La interacción con el dispensador debe ser intuitiva, de forma que el usuario pueda utilizarlo sin necesidad de leer instrucciones complejas.

-Accesibilidad
 *El sistema debe contar con señales visuales y, de ser posible, sonoras para notificar estados importantes como: bajo nivel de cereal, recipiente no detectado o error en  los sensores (H-5, H-9).
 *La información mostrada en pantalla debe tener un tamaño y contraste adecuados para ser entendida fácilmente por niños y adultos.

-Fiabilidad
 *Cada pulsación del botón debe corresponder a una única porción de cereal, respetando la configuración establecida por el usuario (H-3, H-7).
 *El registro de consumo debe guardarse de manera confiable con la fecha, hora y peso correspondiente, asegurando la trazabilidad del uso (H-6).

# 6. Hidtorias de usuario
-Dispensador de cereal
 *Cómo niño quiero presionar un botón para recibir una porción de cereal para poder servirme fácilmente sin ayuda 
-Contador de porciones 
 *Cómo padre quiero que el dispensador muestre cuántas porciones se han servido, para poder controlar el consumo del niño
-Detección de recipiente 
*Cómo niño quiero que el sistema detecte si hay un recipiente antes de dispensar, para evitar que los cereales caigan al suelo
-Control de porción automática
 *Cómo niño quiero que el dispensador entregue siempre la misma cantidad de cereal para no recibir de más o menos cereal
-Notificación de contenedor vacío 
 *Cómo padre quiero que el dispensador me avise cuando se esté acabando el cereal para poder llenarlo a tiempo
-Historial de consumo
 *Cómo padre quiero consultar el historial de porciones consumidas para ver la cantidad de veces que como el niño.
