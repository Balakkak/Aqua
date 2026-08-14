# Aqua
# Aqua 💧

โปรเจค IoT สำหรับตรวจจับอัตราการไหลของน้ำและวัดปริมาณน้ำ พร้อมระบบบันทึกข้อมูลลง Google Sheets แบบเรียลไทม์ผ่าน Wi-Fi นอกจากนี้ยังมีเซ็นเซอร์ IR สำหรับตรวจจับวัตถุ หากมีวัตถุกีดขวางหรือบังเซ็นเซอร์นานเกินกว่าเวลาที่กำหนด ระบบจะส่งเสียงเตือน (Buzzer) โดยจังหวะการเตือนจะถี่ขึ้นเรื่อยๆ

## 🌟 ฟีเจอร์หลัก (Features)
*   **Water Flow Measurement:** วัดอัตราการไหลของน้ำ (ลิตร/นาที) ด้วย Flow Sensor ผ่านการนับสัญญาณ Pulse (Interrupt)
*   **Google Sheets Integration:** ส่งข้อมูลอัตราการไหลขึ้น Google Sheets อัตโนมัติผ่าน Google Apps Script (HTTPS GET)
*   **Obstacle Detection Alarm:** ตรวจจับวัตถุด้วย IR Sensor หากเซ็นเซอร์ถูกบังติดต่อกันเกิน 15 วินาที Buzzer จะดังเตือนเป็นจังหวะที่เร็วขึ้นเรื่อยๆ จนกระทั่งดังค้าง
*   **Auto-Reset:** ระบบนับเวลาแจ้งเตือนจะรีเซ็ตอัตโนมัติเมื่อไม่มีวัตถุบัง IR Sensor

## 🛠️ อุปกรณ์ที่ใช้ (Hardware Requirements)
*   บอร์ดไมโครคอนโทรลเลอร์ ESP32 (หรือบอร์ดที่รองรับ `WiFi.h` และ `WiFiClientSecure.h`)
*   Water Flow Sensor (อ้างอิงจากสมการ `7.5Q` มักจะเป็นรุ่น **YF-S201**)
*   IR Infrared Obstacle Sensor (เซ็นเซอร์ตรวจจับวัตถุ)
*   Active/Passive Buzzer (ลำโพงเสียงเตือน)
*   สายไฟ Jumper

## 📌 การต่อวงจร (Pin Configuration)

| อุปกรณ์ (Device) | ขาบนบอร์ด (Pin) | ประเภท (Type) |
| :--- | :---: | :--- |
| **Flow Sensor** (สัญญาณ Data) | `GPIO 13` | `INPUT_PULLUP` (Interrupt RISING) |
| **IR Sensor** (สัญญาณ Data) | `GPIO 12` | `INPUT` |
| **Buzzer** (สัญญาณ Data) | `GPIO 15` | `OUTPUT` |

> **Note:** อย่าลืมต่อขา VCC และ GND ของเซ็นเซอร์ทุกตัวเข้ากับแหล่งจ่ายไฟและกราวด์ของบอร์ด

## 💻 ซอฟต์แวร์และไลบรารี (Software & Libraries)
*   **Arduino IDE** (สำหรับการคอมไพล์และอัปโหลดโค้ด)
*   **ESP32 Board Package** (ต้องติดตั้งใน Boards Manager)
*   ไลบรารีพื้นฐานที่มากับบอร์ด: `WiFi.h`, `WiFiClientSecure.h`

## 🚀 การติดตั้งและการใช้งาน (Setup & Usage)

1.  **ตั้งค่าเครือข่าย Wi-Fi:**
    เปิดไฟล์โค้ดในโฟลเดอร์ `Arduino IDE code` และแก้ไขชื่อ Wi-Fi และรหัสผ่านของคุณ
    ```cpp
    char ssid[] = "YOUR_WIFI_SSID";
    char pass[] = "YOUR_WIFI_PASSWORD";
    ```
2.  **ตั้งค่า Google Sheets & Apps Script:**
    *   สร้าง Google Sheets และไปที่ `Extensions` > `Apps Script`
    *   เขียนฟังก์ชัน `doGet(e)` เพื่อรับค่า
    *   กด Deploy เป็น **Web App** และตั้งค่าการเข้าถึงเป็น *Anyone*
    *   นำ Script ID ที่ได้มาใส่ในตัวแปร `GAS_ID`
    ```cpp
    String GAS_ID = "YOUR_GOOGLE_APPS_SCRIPT_ID";
    ```
3.  **อัปโหลดโค้ด:**
    เลือกบอร์ดและพอร์ตใน Arduino IDE ให้ถูกต้อง จากนั้นกด Upload
4.  **ตรวจสอบการทำงาน:**
    เปิด Serial Monitor ที่ Baud Rate `9600` เพื่อดูสถานะการเชื่อมต่อ Wi-Fi, อัตราการไหลของน้ำ, และสถานะการตรวจจับวัตถุ

## ⚠️ คำเตือนด้านความปลอดภัย (Security Note)
ก่อนที่จะคอมมิต (Commit) หรือพุช (Push) โค้ดลงบน GitHub **กรุณาตรวจสอบให้แน่ใจว่าได้ลบหรือเซนเซอร์รหัสผ่าน Wi-Fi และ Google Apps Script ID ออกจากโค้ดแล้ว** เพื่อป้องกันข้อมูลส่วนตัวรั่วไหล
