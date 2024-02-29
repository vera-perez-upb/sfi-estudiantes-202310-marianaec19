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
El programa utiliza una maquina de estado para realizar ciertas tareas. INIT inicializa el puerto del monitor serial de la rasperry, inicializa la variable ```lastTime``` y despues declara el seguiente estado ```WAIT_TIMEOUT``` donde realiza un print al monitor serialr. Despues, el siguiente caso del switch verifica en que estado se encuentra el ```Task 1State```, donde deberia estar en ```WAIT_TIMEOUT```, se va a este caso donde se declara y se inicializa una nueva variable llamada ```currentTime```. Se realiza un if statement, donde se determina si la diferencia de ```currentTime``` y ```lastTime``` son mayores o iguales ql intervalo, si esto se cumple, realiza un print mediante el monitor serial donde se aprecia el tiempo actual y se devuelve al inicio del switch statement, donde se completae el ciclo: Se verifica si se encuentra en ```WAIT_TIMEOUT``` y se empieza a contar, cuando el conteo sobrepase el intervalo, se realiza el print en el tiempo actual, y se repite (ya que es un bucle).

Pudiste ver este mensaje: ```Serial. print ("Task1States: :WAIT _TIMEOUT\n");``` - ¿Por qué crees que ocurre esto?
Indica por medio del monitor serial que el estado siguiente es el ```WAIT TIMEOUT```.

¿Cuántas veces se ejecuta el código en el case Task1States:INIT? Una vez, cuando se inicializa el programa (la primera vez), ya que luego de iniciarse, nunca regresara al estado indicado, puesto que el switch no lo permitirá.

# Ejercicio 7
Análisis del programa de prueba 

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

# Ejercicio 9
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

# Ejercicio 10
Configuración ScriptCommunicator.

# Ejercicio 11

```
void task1()
{
    enum class Task1States    {
        INIT,
        WAIT_DATA
    };
    static Task1States task1State = Task1States::INIT;

    switch (task1State)
    {
    case Task1States::INIT:
    {
        Serial.begin(115200);
        task1State = Task1States::WAIT_DATA;
        break;
    }

    case Task1States::WAIT_DATA:
    {
        // evento 1:        // Ha llegado al menos un dato por el puerto serial?        if (Serial.available() > 0)
        {
            Serial.read();
            Serial.print("Hola computador\n");
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
}

void loop()
{
    task1();
}

```

1. Analiza el programa. ¿Por qué enviaste la letra con el botón send? ¿Qué evento verifica si ha llegado algo por el puerto serial?
2. Analiza los números que se ven debajo de las letras. Nota que luego de la r, abajo, hay un número. ¿Qué es ese número?
3. ¿Qué relación encuentras entre las letras y los números?
4. ¿Qué es el 0a al final del mensaje y para qué crees que sirva?
5. Nota que luego de verificar si hay datos en el puerto serial se DEBE HACER UNA LECTURA del puerto. Esto se hace para retirar del puerto el dato que llegó. Si esto no se hace entonces parecerá que siempre tiene un datos disponible en el serial para leer. ¿Tiene sentido esto? Si no es así habla con el profe.

El evento que verifica si ha llegado algo por el puerto serial es la condición if (Serial.available() > 0) en la sección WAIT_DATA del programa.
El número debajo de la R es 114.
Cada letra tiene un valor numérico asociado según la tabla ASCII.
El '0a' al final del mensaje corresponde al valor hexadecimal 0A en la tabla ASCII, que representa un carácter de nueva línea. Este carácter indica un salto.
Tiene sentido, porque después de verificar si hay datos disponibles en el serial port, se realiza el Serial.read() para quitar el dato que llegó. De lo contrario parecerá que siempre hay datos disponibles para leer en el puerto serial.

# Ejercicio 12

```
void task1()
{
    enum class Task1States    {
        INIT,
        WAIT_DATA
    };
    static Task1States task1State = Task1States::INIT;

    switch (task1State)
    {
    case Task1States::INIT:
    {
        Serial.begin(115200);
        task1State = Task1States::WAIT_DATA;
        break;
    }

    case Task1States::WAIT_DATA:
    {
        // evento 1:        // Ha llegado al menos un dato por el puerto serial?        if (Serial.available() > 0)
        {
            // DEBES leer ese dato, sino se acumula y el buffer de recepción            // del serial se llenará.            Serial.read();
            uint32_t var = 0;
            // Almacena en pvar la dirección de var.            uint32_t *pvar = &var;
            // Envía por el serial el contenido de var usando            // el apuntador pvar.            Serial.print("var content: ");
            Serial.print(*pvar);
            Serial.print('\n');
            // ESCRIBE el valor de var usando pvar            *pvar = 10;
            Serial.print("var content: ");
            Serial.print(*pvar);
            Serial.print('\n');
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
}

void loop()
{
    task1();
}

```
¿Cómo se declara un puntero?

Un puntero se declara indicando el tipo de datos al que apunta, seguido por el asterisco *. Por ejemplo: int *ptr; declara un puntero a un entero.

¿Cómo se define un puntero? 

Un puntero se inicializa asignándole la dirección de una variable del mismo tipo de datos al que apunta. Por ejemplo: int *ptr = &miVariable; inicializa el puntero ptr con la dirección de la variable miVariable.

¿Cómo se obtiene la dirección de una variable?

La dirección de una variable se obtiene utilizando el operador de dirección &. Por ejemplo: int *ptr = &miVariable; asigna la dirección de miVariable al puntero ptr.
¿Cómo se puede leer el contenido de una variable por medio de un puntero?

Se utiliza el operador de indirección * para acceder al valor almacenado en la dirección a la que apunta el puntero. Por ejemplo: int valor = *ptr; asigna a valor el contenido de la variable a la que apunta ptr.

¿Cómo se puede escribir el contenido de una variable por medio de un puntero?

Se utiliza el operador de indirección * junto con la asignación para modificar el valor almacenado en la dirección a la que apunta el puntero. Por ejemplo: *ptr = 42; asigna el valor 42 a la variable a la que apunta ptr.

# Ejercicio 13 
Hipótesis:

El programa parece tener dos funciones adicionales además de task1 llamadas changeVar y printVar. En la función task1, se inicializa Serial y se espera un evento en el puerto serial. Cuando llega un dato, se lee un byte y se inicializa una variable var como 0. Se crea un puntero pvar que apunta a la dirección de var. Luego, se imprime el contenido de var utilizando la función printVar, se cambia el valor de var a 10 utilizando la función changeVar y, finalmente, se imprime nuevamente el contenido de var.

Ejecución y Comparación:

Al ejecutar el programa, se espera que la función printVar imprima el contenido de var antes y después de llamar a changeVar. La función changeVar modifica el valor de var a 10. Por lo tanto, se esperaría ver dos líneas impresas por printVar: una con el valor inicial de var (0) y otra con el valor modificado (10).

Posibles Resultados:

Primera Línea: "var content: 0"
Segunda Línea: "var content: 10"
Al comparar el resultado con la hipótesis, si se obtienen estas dos líneas, el programa está funcionando según lo previsto. Si no es así, se debe analizar la ejecución para comprender por qué difiere de la hipótesis.


# Ejercico 14

```
#include <iostream>

// Función para intercambiar el valor de dos variables
void swapValues(int &a, int &b) {
    int temp = a;
    a = b;
    b = temp;
}

int main() {
    // Variables iniciales
    int variable1 = 5;
    int variable2 = 10;

    // Mostrar valores iniciales
    std::cout << "Antes del intercambio:" << std::endl;
    std::cout << "Variable 1: " << variable1 << std::endl;
    std::cout << "Variable 2: " << variable2 << std::endl;

    // Llamar a la función para intercambiar valores
    swapValues(variable1, variable2);

    // Mostrar valores después del intercambio
    std::cout << "\nDespués del intercambio:" << std::endl;
    std::cout << "Variable 1: " << variable1 << std::endl;
    std::cout << "Variable 2: " << variable2 << std::endl;

    return 0;
}
```
