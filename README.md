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
# Ejercicio 15
¿Por qué es necesario declarar rxData static? y si no es static ¿Qué pasa?

Al declarar rxData como static, la variable conserva su valor entre las llamadas sucesivas a la función task1. Si no se declara como static, la variable se reiniciaría a cero cada vez que se ingresa a la función, lo que podría provocar la pérdida de datos entre llamadas. Al ser static, rxData mantiene su valor incluso cuando la función task1 sale de su ámbito y luego vuelve a entrar.

¿dataCounter se define static y se inicializa en 0. Cada vez que se ingrese a la función loop dataCounter se inicializa a 0? ¿Por qué es necesario declararlo static?

Sí, al ser declarado static, dataCounter mantiene su valor entre las llamadas a la función. Si no fuera static, se reiniciaría a cero cada vez que se ingresa a la función loop, lo que podría afectar el seguimiento del estado de la recepción de datos. La persistencia de dataCounter permite rastrear cuántos datos se han recibido hasta el momento.

Observa que el nombre del arreglo corresponde a la dirección del primer elemento del arreglo. ¿Por qué es necesario?

Al utilizar el nombre del arreglo (rxData) sin el operador de subíndice [], se refiere a la dirección del primer elemento del arreglo. Esto es útil al pasar el arreglo como argumento a la función processData, ya que se pasa la dirección base del arreglo.

En la expresión sum = sum + (pData[i] - 0x30); observa que puedes usar el puntero pData para indexar cada elemento del arreglo mediante el operador [].

Sí, pData[i] accede al i-ésimo elemento del arreglo al que apunta el puntero pData. En este caso, pData[i] - 0x30 realiza una conversión de caracteres codificados en ASCII a sus valores numéricos correspondientes. Esta conversión es necesaria porque los caracteres numéricos se envían codificados en ASCII a través del puerto serial.

Finalmente, la constante 0x30 en (pData[i] - 0x30) ¿Por qué es necesaria?

La constante 0x30 se utiliza para convertir los caracteres numéricos codificados en ASCII a sus equivalentes numéricos. En ASCII, los dígitos del 0 al 9 tienen códigos hexadecimales de 0x30 a 0x39. Restar 0x30 permite obtener el valor numérico real de los caracteres recibidos.
Para probar el programa, puedes usar ScriptCommunicator en la pestaña Utf8 o Mixed para enviar una cadena de caracteres numéricos por el puerto serial de la Raspberry Pi Pico. Deberías ver la suma de los valores numéricos impresos en la consola.



# Ejercicio 16

¿Qué pasa cuando hago un Serial.available()?

Serial.available() devuelve el número de bytes disponibles en el buffer de recepción del puerto serial. Indica cuántos bytes están listos para ser leídos.

¿Qué pasa cuando hago un Serial.read()?

Serial.read() lee el primer byte disponible en el buffer de recepción y devuelve su valor como un entero. Si no hay datos disponibles, devuelve -1.

¿Qué pasa cuando hago un Serial.read() y no hay nada en el buffer de recepción?

Si no hay nada en el buffer de recepción, Serial.read() devuelve -1.
Un patrón común al trabajar con el puerto serial es este:

```
if (Serial.available() > 0){
    int dataRx = Serial.read();
}
```
Este patrón verifica si hay datos disponibles en el buffer de recepción antes de intentar leer. Si hay datos, se lee el primer byte disponible.

¿Cuántos datos lee Serial.read()?

Serial.read() lee un byte del buffer de recepción y lo devuelve como un entero.

¿Y si quiero leer más de un dato?

Si deseas leer más de un dato, puedes llamar a Serial.read() varias veces en un bucle, leyendo un byte a la vez. Puedes almacenar los bytes leídos en un arreglo o procesarlos según sea necesario.

 ¿Qué pasa si te envían datos por serial y se te olvida llamar Serial.read()?

Si no llamas a Serial.read(), los datos en el buffer de recepción permanecerán allí y podrían acumularse. Si no lees los datos, eventualmente el buffer podría llenarse y podrías perder datos entrantes.

# Ejercicio 17

Escenario 1:
````
if (Serial.available() >= 3){
    int dataRx1 = Serial.read();
    int dataRx2 = Serial.read();
    int dataRx3 = Serial.read();
}
````
En este escenario, el código verifica si hay al menos 3 bytes disponibles en el buffer de recepción antes de leer. Si hay 3 o más bytes disponibles, se leen tres bytes usando Serial.read() y se almacenan en las variables dataRx1, dataRx2 y dataRx3. Este código es útil cuando se espera específicamente que lleguen tres bytes consecutivos y se desea procesarlos juntos.

Escenario 2:
````
if (Serial.available() >= 2){
    int dataRx1 = Serial.read();
    int dataRx2 = Serial.read();
    int dataRx3 = Serial.read();
}
````
En este escenario, el código verifica si hay al menos 2 bytes disponibles en el buffer de recepción antes de leer. Sin embargo, intenta leer tres bytes. Esto podría llevar a problemas si no hay tres bytes disponibles en el buffer. Aquí están los posibles escenarios:

Caso ideal: Hay al menos 2 bytes disponibles y se leen tres bytes correctamente. dataRx1, dataRx2 y dataRx3 contendrán los valores de los tres bytes leídos.

Caso subóptimo: Hay exactamente 2 bytes disponibles. En este caso, se leerán dos bytes, pero el tercer Serial.read() intentará leer de un buffer vacío, y dataRx3 contendrá el valor predeterminado -1 que indica que no hay datos disponibles.

Caso menos ideal: Hay menos de 2 bytes disponibles. En este caso, se leerán los bytes disponibles y los demás Serial.read() intentarán leer de un buffer vacío, y las variables restantes contendrán el valor predeterminado -1.

Conclusión:
Es crucial tener en cuenta la cantidad real de bytes disponibles en el buffer de recepción antes de intentar leer. La lógica del programa debe adaptarse a la cantidad esperada de datos y manejar los casos en los que no haya suficientes datos disponibles para evitar comportamientos inesperados.


# Unidad 2
Inicio del trayecto de actividades unidad 2.

# Ejercicio 1












