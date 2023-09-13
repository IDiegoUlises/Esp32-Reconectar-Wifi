# Esp32 Reconectar Wifi

```c++
#include <WiFi.h>

//Credenciales WiFi
const char* ssid = "Wifi";
const char* password = "123456789";

//Funcion que se ejecutara cuando se conecte a una red WiFi
void WiFiConectado(WiFiEvent_t event, WiFiEventInfo_t info)
{
  Serial.println("Conexion WiFi establecida correctamente");
}

//Funcion que se ejecutara siempre cuando no este conectado a una red WiFi
void WiFiDesconectado(WiFiEvent_t event, WiFiEventInfo_t info)
{
  //Imprime cuando se ha podido establecer una conexion WiFi y porque sucedio
  Serial.println("No se ha podido establecer una conexion WiFi");
  Serial.print("Por el motivo: ");
  Serial.println(info.disconnected.reason);
  Serial.println("Reconectando...");

  //Recordar que no se debe agregar delay
  //o tiempos de retardos a los eventos porque
  //puede que el evento no funcione correctamente

  //Reconectar WiFi
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
