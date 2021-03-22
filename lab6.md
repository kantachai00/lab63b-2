# การทดลองที่ 6 เรื่อง การเขียนโปรแกรมสร้างไวไฟแอคเซสพอยต์ (Wifi AP)

## วัตถุประสงค์
1. เพื่อทราบการเขียนโปรแกรมสร้างไวไฟแอคเซสพอยต์

## อุปกรณ์ที่ใช้
1. ไมโครคอนโทรเลอร์ (ESP-01)
2. อุปกรณ์ต่อ USB (USB to serial)
3. CPU
4. ตัวโปรแกรมสร้างไวไฟแอคเซสพอยต์ (06_Wifi-AP-Web-Server)


## ศึกษาข้อมูลเบื้องต้น
1. 06 run wiri AP : https://www.youtube.com/watch?v=T26DVHePlTs
2. src code ของโปรแกรม 06_Wifi-AP-Web-Server : https://github.com/choompol-boonmee/lab63b/blob/master/examples/06_Wifi-AP-Web-Server/src/main.cpp
3. แอคเซสพอยต์ : https://www.mindphp.com/%E0%B8%84%E0%B8%B9%E0%B9%88%E0%B8%A1%E0%B8%B7%E0%B8%AD/73-%E0%B8%84%E0%B8%B7%E0%B8%AD%E0%B8%AD%E0%B8%B0%E0%B9%84%E0%B8%A3/4045-what-is-access-point.html

## วิธีการทำทดลอง
1. นำตัวไมโครคอนโทรเลอร์ต่อกับ USB to serial 
3. เข้าcommand prompt
4. เข้าตัวอย่างโปรแกรมจากโดยใช้พิมพ์คำสั่ง **cd pattani** ในหน้า command prompt
5. เลือกตัวอย่างโปรแกรมคำสั่ง **cd 06_Wifi-AP-Web-Server** 
6. พิมพ์คำสั่ง **vi src/main.cpp** เมื่อกด Enter จะขึ้นโค้ดดังนี้

```javascript
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "TUENG-ESP-WIFI";
const char* password = "choompol";

IPAddress local_ip(192, 168, 1, 1);
IPAddress gateway(192, 168, 1, 1);
IPAddress subnet(255, 255, 255, 0);

ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(115200);

	WiFi.softAP(ssid, password);
	WiFi.softAPConfig(local_ip, gateway, subnet);
	delay(100);

	server.onNotFound([]() {
		server.send(404, "text/plain", "Path Not Found");
	});

	server.on("/", []() {
		cnt++;
		String msg = "Hello cnt: ";
		msg += cnt;
		server.send(200, "text/plain", msg);
	});

	server.begin();
	Serial.println("HTTP server started");
}

void loop(void){
  server.handleClient();
}

```        
จากโค้ดนี้จะเห็นว่าเราต้องการให้ ESP 01 มีไวไฟในตัวเอง โดยจะกำหนดชื่อwifiและpasswoedก่อน ที่จะปล่อยให้เครื่องอื่น connect และมีการสร้างIPAdress,gateway,subnet และ เตรียมเว็บเซิฟเวอร์

7. อัพโหลดโปรแกรมเข้าไมโครคอนโทรเลอร์ด้วยคำสั่ง **pio run -t upload** 
8. กดปุ่มรีเซตที่ตัวไมโครคอนโทรเลอร์เพื่อให้ตัวโปรแกรมอัพโหลดเข้าไปในตัว่ไมโครคอนโทรเลอร์
9. **pio device monitor** เพื่อดูผลลัพท์






![image](https://user-images.githubusercontent.com/80879772/111975374-1cefc280-8b33-11eb-90e7-43ce82ee3cee.png)


10. ลองใช้โทรศัพท์มือถือหรือคอมพิวเตอร์ค้นหาไวไฟที่สร้าง







![image](https://user-images.githubusercontent.com/80879772/111975598-5f190400-8b33-11eb-80c4-72690f3fcacd.png)

## การบันทึกผลการทดลอง
1. เขียนโปรแกรมไมโครคอนโทรเลอร์ด้วยโปรแกรม 06_Wifi-AP-Web-Server
	
	จะเห็นว่าจากคำสั่ง **pio device monitor** เพื่อดูผลลัพธ์ของการรันโปรแกรมในไมโครคอนโทรเลอร์ ได้ขึ้นคำว่า ver started และตรวจสอบผลลัพธ์จากการสร้างไวไฟด้วยการหาชื่อไวไฟด้วยโทรศัพท์มือถือ ซึ่งจากจากการค้นหาด้วยโทรศัพท์ก็ได้พบกับไวไฟที่เราสร้างจากการเขียนโปรแกรม (TUENG-ESP-WIFI)

## อภิปรายผลการทดลอง
จากการทดลองเขียนโปรแกรมสามารถสรุปได้ว่า เราสามารถสร้างไวไฟแอคเซสพอยต์ขึ้นมาเองเพื่อให้ผู้อื่นมาเชื่อมต่อได้ 

## คำถามหลังการทดลอง
ถาม จงบอกประโยชน์ของไวไฟแอคเซสพอยต์

ตอบ ประโยชน์ในการใช้งานไวไฟแอคเซสพอยต์คือ เราสามารถนำอุปกรณ์ที่รับสัญญาณ wireless ไปใช้งานตรงไหนก็ได้ที่ AP อยู่โดยไม่ต้องเดินสายแลนไปเชื่อมต่อกับอุปกรณ์นั้น ๆ ซึ่งจะช่วยในการประหยัดค่าใช้จ่ายในการเดินสาย ใมนความเข้าใจของผู้เขียนรายงานเข้าใจว่าตัวไวไฟแอคเซสพอยต์เหมือนตัวกระจายสัญญาณของ wifi โดยจะกระจายสัญญาณได้ก็ต่อเมื่อ มี IP เดียวกัน

