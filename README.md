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

# Ejercicio 8
```
void setup()
{
  Serial.begin(115200);
}

void loop()
{
  static uint32_t lastTime = 0;
  static uint8_t currentState = 0;

  uint32_t currentTime = millis();

  const uint32_t intervalos[] = {1000, 2000, 3000};

  if (currentTime - lastTime >= intervals[currentState])
  {
    switch (currentState)
    {
    case 1:
      Serial.println("Primer mensaje.");
      break;
    case 2:
      Serial.println("Segundo mensaje.");
      break;
    case 3:
      Serial.println("Tercer mensaje.");
      break;
    }

    
    lastTime = currentTime;
    currentState = (currentState + 1) % 3; 
  }
}
```

Estados del Programa: El programa tiene tres estados (currentState). Los estados son 1, 2 y 3, y cada estado corresponde a un intervalo de tiempo (1 segundo, 2 segundos, 3 segundos).
Eventos: Uno de los eventos es el tiempo. El temporizador se usa para revisar que ya ha pasado el tiempo para relizar cierta tarea y luego cambiar al siguiente estado.
Acciones: Las acciones están determinadas por el estado actual. Se envía al puerto serie (Serial.println) un mensaje cuando se alcanza el tiempo determinado por cada estado.

#Ejercicio 9
```
void task1()
{
    enum class Task1States
    {
        INIT,
        WAIT_FOR_TIMEOUT
    };

    static Task1States task1State = Task1States::INIT;
    static uint32_t lastTime;
    static constexpr uint32_t INTERVALA = 1000;

    switch(task1State)
    {
        case Task1States::INIT:
        {
            Serial1.begin(115200);
            lastTime = millis();
            task1State = Task1States::WAIT_FOR_TIMEOUT;
            break;
        }

        case Task1States::WAIT_FOR_TIMEOUT:
        {
          uint32_t currentTime = millis();
            if( (currentTime - lastTime) >= INTERVALA)
             {
                lastTime = currentTime;
                Serial1.print("mensaje a 1Hz from A interval\n");
                delay(4000);
             }
            break;
        }

        default:
        {
            break;
        }
    }
}

void task2()
{
    enum class Task2States
    {
        INIT,
        WAIT_FOR_TIMEOUT
    };

    static Task2States task2State = Task2States::INIT;
    static uint32_t lastTime;
    static constexpr uint32_t INTERVALB = 2000;

    switch(task2State)
    {
        case Task2States::INIT:
        {
            Serial1.begin(115200);
            lastTime = millis();
            task2State = Task2States::WAIT_FOR_TIMEOUT;
            break;
        }

        case Task2States::WAIT_FOR_TIMEOUT:
        {
           uint32_t currentTime = millis();
            if( (currentTime - lastTime) >= INTERVALB)
            {
                lastTime = currentTime;
                Serial1.print("mensaje a 0.5Hz from B interval\n");
            }
            break;
        }

        default:
        {
            break;
        }
    }
}

void task3()
{
    enum class Task3States
    {
        INIT,
        WAIT_FOR_TIMEOUT
    };

    static Task3States task3State = Task3States::INIT;
    static uint32_t lastTime;
    static constexpr uint32_t INTERVALC = 4000;

    switch(task3State)
    {
        case Task3States::INIT:
        {
            Serial1.begin(115200);
            lastTime = millis();
            task3State = Task3States::WAIT_FOR_TIMEOUT;
            break;
        }

        case Task3States::WAIT_FOR_TIMEOUT:
        {
           uint32_t currentTime = millis();
            if( (currentTime - lastTime) >= INTERVALC)
            {
                lastTime = currentTime;
                Serial1.print("mensaje a 0.25Hz from C interval\n");
            }
            break;
        }

        default:
        {
            break;
        }
    }
}
 
void setup() 
{
  task1();
  task2(); 
  task3();
}

void loop() 
{
  task1();
  task2();
  task3();
}
```

Documented Bugs
Archivo de Arduino IDE no ejecutando de manera correcta. Error lanzado: Syntax
Lo resolvimos de una manera inusual. Al momento de seguir el trayecto de acitividades, nos dimos cuenta que estabamos teniendo problemas para flashear el microcontrolador del Raspberry Pico que teniamos. El IDE mencionaba que teniamos errores en el syntax de la escritura del codigo, especificamente, al momento de compilar, la consola del IDE decia que no era posible compilar debido a que la IDE estaba sobreescribiendo las funciones bases del Arduino como el void setupO y el void loop0, pero esta era imposible ya que habiamos copiado el codigo multiples veces y revisado la syntaxis multiples veces. Al final recurrimos a aislar el archivo en cuestion dentro de su propio folder, y al momento de abrirlo e intentar flashearlo al Pico, funciono de manera adecuada. No entendemos cual podria ser la causa, sin embargo, puede ser posible que al tener multiples archivos ino dentro de un mismo folder puede llevar a problemas el momento de compilarlos, ya que el compiler no tiene en cuenta cual archivo es cual. 
