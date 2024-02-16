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
REPO del grupo: 


# Ejercicio 5
*Actualizar info de documentación en mis propias palabras

# Ejercicio 6

