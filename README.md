# UNIDAD 1

# Ejercicio 3

Ejecute el código, lo salve y lo envie al microcontrolador Rasperry Pi Pico

```
void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  static uint32_t previousTime = 0;
  static bool ledState = true;

  uint32_t currentTime = millis();

  if( (currentTime - previousTime) > 100){
    previousTime = currentTime;
    ledState = !ledState;
    digitalWrite(LED_BUILTIN, ledState);
  }
}
```
Luego Modifique el código (como se muestra abajo) y cambie el 100 por un 500 y la luz empezo a parpadear más lento 

```
void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  static uint32_t previousTime = 0;
  static bool ledState = true;

  uint32_t currentTime = millis();

  if( (currentTime - previousTime) > 500){
    previousTime = currentTime;
    ledState = !ledState;
    digitalWrite(LED_BUILTIN, ledState);
  }
}
```

# Ejercicio 4
REPO del grupo: https://github.com/MateoJimenezFamaArt/RASPBERRY_PICO.git


# Ejercicio 5
*Actualizar info de documentación en mis propias palabras

# Ejercicio 6
```
void task1()
{
    // Definición de estados y variable de estado
    enum class Task1States
    {
        INIT,
        WAIT_TIMEOUT
    };
    static Task1States task1State = Task1States::INIT;

    // Definición de variables static (conservan su valor entre llamadas a task1)
    static uint32_t lastTime = 0;

    // Constantes
    constexpr uint32_t INTERVAL = 1000;

    // MÁQUINA de ESTADOS
    switch (task1State)
    {
    case Task1States::INIT:
    {
        // Acciones:
        Serial.begin(115200);

        // Garantiza los valores iniciales para el siguiente estado.
        lastTime = millis();
        task1State = Task1States::WAIT_TIMEOUT;

        Serial.print("Task1States::WAIT_TIMEOUT\n");

        break;
    }
    case Task1States::WAIT_TIMEOUT:
    {
        uint32_t currentTime = millis();

        // Evento
        if ((currentTime - lastTime) >= INTERVAL)
        {
            // Acciones:
            lastTime = currentTime;
            Serial.print(currentTime);
            Serial.print('\n');
        }
        break;
    }
    default:
    {
        Serial.println("Error");
    }
    }
}

void setup()
{
    task1();
}

void loop()
{
    task1();
}

```
¿Cómo se ejecuta este programa?
El programa utiliza una maquina de estado para realizar ciertas tareas. Se crea una clase que contiene ambos estados INIT y WAIT TIMEOUT, donde INIT inicializa el puerto del Monitor Serial de la board, inicializa la variable lastTime y posteriormente declara el seguiente estado WAIT_TIMEOUT donde realiza un print al serial monitor. Despues, el siguiente caso del switch verifica en que estado se encuentra el Task 1State, donde deberia estar en WAIT_TIMEOUT, se accede a este case donde se declara e inicializa una nueva variable llamada currentTime. Se realiza un if statement, donde se verifica si la diferencia de currentTime y lastTime son mayores o iguales que un intervalo (que equivale a 1s), si esto se cumple, realiza un print mediante el serial monitor donde se ve el tiempo actual y regresa al principio del switch statement, donde se cumple el ciclo: Se verifica si se encuentra en WAIT_TIMEOUT y se empieza a contar, cuando el conteo exceda el intervalo, se printea el tiempo actual, y se repite.

Pudiste ver este mensaje: ```Serial. print ("Task1States: :WAIT _TIMEOUT\n");``` - ¿Por qué crees que ocurre esto?
Se deja saber mediante un print al monitor serial que el estado al que se acaba de pasar es al de WAIT TIMEOUT

¿Cuántas veces se ejecuta el código en el case Task1States:INIT? R/: Una vez, cuando se inicializa el programa por primera vez, ya que una vez iniciado, nunca se regresa a este estado, pues el switch case nunca lo permite.
