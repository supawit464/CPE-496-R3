# CPE-496-R3
ระบบฟาร์มที่สามารถตรวจวัดโรคพืชด้วย AI และควบคุมการให้น้ำ/อุณหภูมิด้วยระบบ IoT โดยใช้ sensor ตรวจวัดความชื้นในดิน, อุณหภูมิ, ความชื้นในอากาศ และประมวลผลผ่าน microcontroller พร้อมส่งข้อมูลแจ้งเตือนผ่าน Blynk App และตัดสินใจควบคุมอัตโนมัติ

______________________________________________________________________________________________________________________________________________________

คู่มือการทำงาน
คู่มือการใช้งานโค้ด ESP32 + Blynk + DHT22 + Soil Sensor + Relay
1️⃣ สิ่งที่ต้องมี

บอร์ด ESP32 Devkit V1 ,เซ็นเซอร์ DHT22 (วัดอุณหภูมิ/ความชื้นอากาศ) ,เซ็นเซอร์ Soil Moisture Analog (แบบที่มี 2 ขา OUT = Analog)
,โมดูล Relay 1 Channel (ควบคุมปั๊มน้ำ 220V หรือปั๊ม DC) ,ปั๊มน้ำ 5V/12V/220V (ขึ้นกับรีเลย์และแหล่งจ่ายไฟ)
,สายไฟ Jumper (Male-Female) ,โปรแกรม PlatformIO หรือ Arduino IDE ,แอป Blynk IoT (มือถือ)

2️⃣ การต่อสาย (Wiring)
อุปกรณ์	ขาอุปกรณ์	ต่อกับ ESP32
DHT22	VCC	3.3V ,GND	GND ,DATA	GPIO 27 ,Soil Moisture	VCC	3.3V ,GND	GND ,AOUT	GPIO 34
Relay	VCC	5V,GND	GND IN	GPIO 23

⚠️ ข้อควรระวัง:
Relay Module ต้องใช้ VCC 5V เพื่อให้ขับรีเลย์ติดได้
ถ้าใช้ปั๊ม 220V ต้องระวังไฟฟ้าแรงสูง

3️⃣ การตั้งค่าใน Blynk IoT
สร้าง Template
Template Name: project 496
Hardware: ESP32
Connection: WiFi
Copy Template ID และ Auth Token มาใส่ในโค้ด
สร้าง Datastream
V0 → Soil Moisture (ประเภท: Integer)
V1 → Air Humidity (ประเภท: Double)
V2 → Temperature (ประเภท: Double)
V3 → Pump Switch (ประเภท: Switch)

สร้าง Dashboard ในแอป
Gauge หรือ Label สำหรับ V0, V1, V2
Button (Switch Mode) สำหรับ V3 → ควบคุมปั๊มน้ำ

4️⃣ การอัปโหลดโค้ด
เปิด PlatformIO / Arduino IDE
เลือกบอร์ด ESP32 Dev Module
ใส่ WiFi SSID และ Password ของคุณในตัวแปร ssid และ pass
ใส่ BLYNK_AUTH_TOKEN ของคุณ
กด Upload → รอจนเสร็จ → เปิด Serial Monitor (115200 baud)

5️⃣ การทำงานของโค้ด

ESP32 จะ:

อ่านค่าความชื้นดินจากขา Analog 34 → แปลงเป็น % (0-100)
อ่านค่าความชื้นอากาศ + อุณหภูมิจาก DHT22
ส่งค่าทุก 1 วินาทีไป Blynk
ถ้ากดปุ่มใน Blynk (V3):
ON → เปิดรีเลย์ (ปั๊มน้ำทำงาน)
OFF → ปิดรีเลย์ (ปั๊มหยุด)

6️⃣ การใช้งาน
ต่ออุปกรณ์ตามแผนผัง
อัปโหลดโค้ด
เปิด Serial Monitor → ดูการเชื่อมต่อ WiFi
เปิดแอป Blynk → ดูค่าบน Dashboard
กดปุ่มใน Blynk เพื่อเปิด/ปิดปั๊มน้ำ

7️⃣ การแก้ปัญหาเบื้องต้น
Serial Monitor ขึ้น "Failed to read from DHT22!"
→ ตรวจสอบสาย DATA ของ DHT22, ใช้ Pull-up resistor 10k ระหว่าง DATA และ VCC
Relay ไม่ทำงาน
→ ตรวจสอบว่า VCC ของ Relay เป็น 5V, และ GND เชื่อมกับ GND ของ ESP32
ค่าความชื้นดินผิดปกติ
→ อาจต้องปรับ map() ให้เหมาะกับเซ็นเซอร์ของคุณ (บางรุ่นค่ากลับด้าน)

______________________________________________________________________________________________________________________________________________________
