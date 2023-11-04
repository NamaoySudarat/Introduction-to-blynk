# 64030313 นางสาวสุดารัตน์ ฆ้องอินตะ

# Video
https://drive.google.com/file/d/19J1qwgd0KfqN8WgDNsg_3SbLmEs3tgZ7/view?usp=sharing

# Dashboard
<img width="1531" alt="image" src="https://github.com/NamaoySudarat/Introduction-to-blynk/assets/115037574/08938833-3830-46c0-ba60-4dc1a4d0bd6c">

# Code
```c
#define BLYNK_TEMPLATE_ID           "TMPL6DEDQtnFM"
#define BLYNK_TEMPLATE_NAME         "sudarat"
#define BLYNK_AUTH_TOKEN            "ek4teGmhzICVbHfZ_yk6Au_5wksIBcPR"
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

char ssid[] = "Namaoy";
char pass[] = "11055011";

BlynkTimer timer;

int ledState = LOW;

void myTimerEvent()
{
  float temp = dht.readTemperature();
  float humi = dht.readHumidity();
  int ldrValue = analogRead(ldrPin);
  int volumeValue = analogRead(volumePin);
  Serial.println(volumeValue);
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
