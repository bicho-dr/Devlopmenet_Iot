#include <WiFi.h>
#include <PubSubClient.h>
#include "DHT.h"

// 1. بيانات شبكة الواي فاي المتصلة بالإنترنت (غيرها إلى شبكتك أو هاتف Hotspot)
const char* ssid = "HOT-PC 0651";
const char* password = "12345678";

// 2. إعدادات منصة ThingsBoard
const char* mqtt_server = "thingsboard.cloud"; // أو عنوان السيرفر الخاص بك إذا كان محلياً
const int mqtt_port = 1883;
const char* token = "9hZ21MbH2dWWrHOIDb1O"; // الصق الرمز الذي نسخته من المنصة هنا

#define DHTPIN 20        // بن حساس DHT11
#define DHTTYPE DHT11    
#define BUZZER_PIN 40    // البازر

const float TEMP_THRESHOLD = 30.0;

DHT dht(DHTPIN, DHTTYPE);
WiFiClient espClient;
PubSubClient client(espClient);

unsigned long lastMsg = 0;
const long interval = 5000; // الإرسال كل 5 ثوانٍ

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("جاري الاتصال بشبكة الواي فاي: ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("تم الاتصال بالواي فاي بنجاح!");
  Serial.print("عنوان الـ IP: ");
  Serial.println(WiFi.localIP());
}

void reconnect() {
  // محاولة الاتصال بسيرفر ThingsBoard عبر MQTT
  while (!client.connected()) {
    Serial.print("جاري الاتصال بـ ThingsBoard MQTT...");
    // Attempt to connect (Device Token is used as MQTT Username)
    if (client.connect("ESP32S3Device", token, "")) {
      Serial.println("[متصل بنجاح]");
    } else {
      Serial.print("فشل الاتصال، الخطأ rc=");
      Serial.print(client.state());
      Serial.println(" إعادة المحاولة خلال 5 ثوانٍ...");
      delay(5000);
    }
  }
}

void setup() {
  Serial.begin(115200);
  pinMode(BUZZER_PIN, OUTPUT);
  digitalWrite(BUZZER_PIN, LOW);

  dht.begin();
  setup_wifi();
  
  client.setServer(mqtt_server, mqtt_port);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  unsigned long now = millis();
  if (now - lastMsg > interval) {
    lastMsg = now;

    float h = dht.readHumidity();
    float t = dht.readTemperature();

    if (isnan(h) || isnan(t)) {
      Serial.println("فشل في قراءة الحساس!");
      return;
    }

    // طباعة القراءات في شاشة المراقبة
    Serial.print("الحرارة: "); Serial.print(t);
    Serial.print(" °C | الرطوبة: "); Serial.print(h); Serial.println(" %");

    // تجهيز البيانات بصيغة JSON لإرسالها لـ ThingsBoard
    // الصيغة: {"temperature": 25.5, "humidity": 60}
    String payload = "{";
    payload += "\"temperature\":" + String(t, 1) + ",";
    payload += "\"humidity\":" + String(h, 1);
    payload += "}";

    // نشر البيانات (Publish) إلى الـ telemetry الخاص بـ ThingsBoard
    client.publish("v1/devices/me/telemetry", payload.c_str());
    Serial.println("تم إرسال البيانات إلى ThingsBoard بنجاح!");

    // تفعيل البازر محلياً إذا تجاوزت الحرارة الحد المسموح
    if (t >= TEMP_THRESHOLD) {
      digitalWrite(BUZZER_PIN, HIGH);
    } else {
      digitalWrite(BUZZER_PIN, LOW);
    }
  }
}