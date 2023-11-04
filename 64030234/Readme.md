# นางสาวกุลธิดา  เกษร

![image](https://github.com/KUNTIDA234/Introduction-to-blynk/assets/115066215/f895a735-5107-4b17-822f-9263bc74539b)

youtube : https://www.youtube.com/watch?v=dgFiiwYcb2I

# code
```c
#define BLYNK_TEMPLATE_ID           "TMPL6rGCUw-Rx"
#define BLYNK_TEMPLATE_NAME         "blynk234"
#define BLYNK_AUTH_TOKEN            "dNUkczApf6K4NrnuG1PCMNzwj6HOrcGA"
#define BLYNK_PRINT Serial


#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>

#include "DHT.h"

#define DHTPIN 4
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

#define ledPin 23
#define ldrPin 39
#define volumePin 34

char ssid[] = "Ktdd";
char pass[] = "87654321";

BlynkTimer timer;

int ledState = LOW;

void myTimerEvent()
{
  float temp = dht.readTemperature();
  float humi = dht.readHumidity();
  int ldrValue = analogRead(ldrPin);
  int volumeValue = analogRead(volumePin);

  if (isnan(temp) || isnan(humi)) {
    Serial.println("Failed to read from DHT sensor. Please check the wiring.");
  } else {
    Serial.println("Sended to Blynk.");
    Blynk.virtualWrite(V1, temp);
    Blynk.virtualWrite(V2, humi);
    Blynk.virtualWrite(V3, ldrValue);
    Blynk.virtualWrite(V4, volumeValue);
  }
}

BLYNK_WRITE(V5) {
  int value = param.asInt();
  if (value == 1) {
    ledState = HIGH;
  } else {
    ledState = LOW;
  }
  digitalWrite(ledPin, ledState);
}

void setup()
{
  Serial.begin(115200);
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
  timer.setInterval(1000L, myTimerEvent);
  dht.begin();
  pinMode(ledPin, OUTPUT);
}

void loop()
{
  Blynk.run();
  timer.run();
}

```
