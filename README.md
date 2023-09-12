# Esp32-Reconectar-Wifi

### Codigo original que se le deben hacer modificaciones
```c++
#include <WiFi.h>

// Replace with your network credentials (STATION)
const char* ssid = "REPLACE_WITH_YOUR_SSID";
const char* password = "REPLACE_WITH_YOUR_PASSWORD";

unsigned long previousMillis = 0;
unsigned long interval = 30000;

void initWiFi() {
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  Serial.print("Connecting to WiFi ..");
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print('.');
    delay(1000);
  }
  Serial.println(WiFi.localIP());
}

void setup() {
  Serial.begin(115200);
  initWiFi();
  Serial.print("RSSI: ");
  Serial.println(WiFi.RSSI());
}

void loop() {
  unsigned long currentMillis = millis();
  // if WiFi is down, try reconnecting every CHECK_WIFI_TIME seconds
  if ((WiFi.status() != WL_CONNECTED) && (currentMillis - previousMillis >=interval)) {
    Serial.print(millis());
    Serial.println("Reconnecting to WiFi...");
    WiFi.disconnect();
    WiFi.reconnect();
    previousMillis = currentMillis;
  }
}

```

### Codigo prometedor que utiliza eventos para reconectar el Wifi

```c++
#include <WiFi.h>

//Credenciales WiFi
const char* ssid = "Wifi";
const char* password = "123456789";

//Funcion que se ejecutara cuando se conecte a una red WiFi
void WiFiConectado(WiFiEvent_t event, WiFiEventInfo_t info)
{
  Serial.println("Correctamente conectado al punto de acceso");
}

//Funcion que se ejecutara siempre cuando no este conectado a una red WiFi
void WiFiDesconectado(WiFiEvent_t event, WiFiEventInfo_t info)
{
  //Imprime cuando se pierda la conexion Wifi y la razon porque sucedio
  Serial.println("Se ha perdido la conexion WiFi");
  Serial.print("Por el motivo:");
  Serial.println(info.disconnected.reason);
  Serial.println("Reconectando...");
  
  //Recordar que no se recomienda agregar delay
  //o tiempos de retardo a los eventos porque
  //pueden que el evento no funcione correctamente

  //Reconectar Wifi
  WiFi.begin(ssid, password);
}
void setup()
{
  //Inicia el puerto serial
  Serial.begin(115200);

  //Funcion que se ejecutara cuando se conecte a una red WiFi
  WiFi.onEvent(WiFiConectado, SYSTEM_EVENT_STA_CONNECTED);

  //Funcion que se ejecutara siempre cuando no este conectado a una red WiFi
  WiFi.onEvent(WiFiDesconectado, SYSTEM_EVENT_STA_DISCONNECTED);

  //Se conecta a la red Wifi
  WiFi.begin(ssid, password);
}

void loop()
{
  //En espera infinitamente
  delay(1000);
}
```
