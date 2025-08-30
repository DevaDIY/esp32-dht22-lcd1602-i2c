# esp32-dht22-lcd1602-i2c
# ESP32 DHT22 + LCD1602 I2C Display

โครงงานนี้สาธิตการใช้งาน **ESP32** อ่านค่าอุณหภูมิและความชื้นจาก **DHT22 Sensor** และแสดงผลบนจอ **LCD1602 I2C** เหมาะสำหรับผู้เริ่มต้นสาย Maker / Smart Farm

## 🔧 อุปกรณ์ที่ใช้
- ESP32 DevKit V1
- DHT22 Sensor
- LCD1602 พร้อมโมดูล I2C (Address ปกติ 0x27)
- สาย Jumper + Breadboard

## 🔌 การต่อวงจร
- DHT22:  
  - VCC → 3.3V  
  - GND → GND  
  - DATA → GPIO 4  

- LCD1602 I2C:  
  - VCC → 5V  
  - GND → GND  
  - SDA → GPIO 21  
  - SCL → GPIO 22
 

## อธิบายโค้ดสั้นๆ

dht.readTemperature() → อ่านค่าอุณหภูมิ (°C)

dht.readHumidity() → อ่านค่าความชื้น (%)

lcd.setCursor(col,row) → กำหนดตำแหน่งแสดงผลบนจอ

## อ่านบทความเต็ม

👉 [สอนต่อ DHT22 กับ LCD1602 I2C บน ESP32](https://devadiy.com/esp32-dht22-lcd1602-i2c/)

👉 [รวมโครงงาน ESP32 และ Smart Farm](https://devadiy.com/)

## 💻 โค้ดตัวอย่าง

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <Adafruit_Sensor.h>
#include <DHT.h>
#include <DHT_U.h>

#define DHTPIN 4
#define DHTTYPE DHT22

DHT dht(DHTPIN, DHTTYPE);
LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
  Serial.begin(115200);
  lcd.init();
  lcd.backlight();
  dht.begin();

  lcd.setCursor(0, 0);
  lcd.print("Deva DIY Demo");
  lcd.setCursor(0, 1);
  lcd.print("DHT22 + LCD1602");
  delay(2000);
  lcd.clear();
}

void loop() {
  float h = dht.readHumidity();
  float t = dht.readTemperature();

  if (isnan(h) || isnan(t)) {
    Serial.println("อ่านค่า DHT22 ไม่สำเร็จ!");
    lcd.setCursor(0, 0);
    lcd.print("Sensor Error   ");
    delay(2000);
    return;
  }

  Serial.print("Humidity: ");
  Serial.print(h);
  Serial.print("%  Temp: ");
  Serial.print(t);
  Serial.println("C");

  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.print(t);
  lcd.print(" C   ");

  lcd.setCursor(0, 1);
  lcd.print("Hum : ");
  lcd.print(h);
  lcd.print(" %   ");

  delay(2000);
}
