# 🌡️ ESP32-S3 Environmental Monitoring System (IoT)

مشروع كامل لنظام مراقبة بيئي متكامل قائم على لوحة **ESP32-S3** وحساس **DHT11**. يوفر المشروع حلولاً لمراقبة الحرارة والرطوبة محلياً وعن بُعد عبر منصة السحاب **ThingsBoard** باستخدام بروتوكول **MQTT**، بالإضافة إلى تسجيل البيانات المحلي وتنبيهات الأجهزة.

---

## 🚀 المميزات الرئيسية (Features)

* **مراقبة بيئية حية:** قراءة مستمرة لدرجات الحرارة والرطوبة باستخدام حساس DHT11.
* **التكامل مع منصة ThingsBoard السحابية:** إرسال البيانات دورياً عبر بروتوكول MQTT وعرضها في لوحة قيادة (Dashboard) تفاعلية.
* **نظام تنبيه مادي محلي (Hardware Alert):** تفعيل البازر (Buzzer) تلقائياً عند تجاوز درجة الحرارة للحد المسموح (Threshold).
* **خادم ويب محلي (Local Web Server):** إمكانية الوصول إلى البيانات محلياً عبر وضع Access Point (AP).
* **تسجيل البيانات (Data Logging):** حفظ القراءات السابقة في ملف `CSV` داخل الذاكرة الداخلية للوحة (SPIFFS).

---

## 🛠️ العتاد المستخدم (Hardware Components)

| المكون | المخرج / البن (Pin) | الوصف |
| :--- | :--- | :--- |
| **ESP32-S3** | — | وحدة التحكم الرئيسية برابط Wi-Fi |
| **DHT11 Sensor** | GPIO 20 | حساس قياس درجة الحرارة والرطوبة |
| **Buzzer** | GPIO 40 | جرس التنبيه عند ارتفاع الحرارة |
| **I2C LCD (16x2)** | SDA: GPIO 4 / SCL: GPIO 5 | شاشة عرض محلية (تحت التطوير والتكامل) |

---

## 💻 البرمجيات والمكتبات المطلوبة (Software & Libraries)

تم تطوير هذا المشروع باستخدام **Arduino IDE** ومحرر **VS Code**. يلزم تثبيت المكتبات التالية:

* `WiFi.h` (مدمجة مع حزمة ESP32)
* `DHT sensor library` (بواسطة Adafruit)
* `PubSubClient` (بواسطة Nick O'Leary - للاتصال بـ MQTT)
* `LiquidCrystal_I2C` (للتحكم بشاشة الـ LCD)

---

## 📁 هيكل المشروع في المستودع (Repository Structure)

* `code`: كود المرحلة الأولى (اختبار العتاد والخادم المحلي).
* `code_two`: كود تسجيل البيانات وحفظها بصيغة CSV على الذاكرة الداخلية.
* `code_three` / `sketch_oct25a.ino`: الكود النهائي المربوط بـ ThingsBoard Cloud عبر MQTT.
* `download-csv.csv`: نموذج لملف البيانات المسجلة محلياً.

---

## 🔧 كيفية التشغيل (Setup & Installation)

1. **إعداد العتاد:** قم بتوصيل الحساس والبازر والشاشة بالبنات (Pins) المحددة في الجدول أعلاه.
2. **إعداد منصة ThingsBoard:**
   * قم بإنشاء جهاز جديد على [ThingsBoard Cloud](https://thingsboard.cloud).
   * انسخ رمز الوصول الخاص بالجهاز (**Device Access Token**).
3. **تعديل الكود:**
   * افتح كود المشوع وقم بتعديل بيانات الواي فاي ورمز الـ Token:
     ```cpp
     const char* ssid = "YOUR_WIFI_SSID";
     const char* password = "YOUR_WIFI_PASSWORD";
     const char* token = "YOUR_DEVICE_ACCESS_TOKEN";
     ```
4. **الرفع:** ارفع الكود إلى لوحة ESP32-S3 وافتح الـ Serial Monitor بـ Baud Rate `115200`.

---

## 👤 المطور (Author)

* **bicho-dr** - [GitHub Profile](https://github.com/bicho-dr)