# การทดลองที่ 5 เรื่อง การเขียนโปรแกรมเชื่อมต่อไวไฟและเว็บเซอร์เวอร์

## วัตถุประสงค์
1. เพื่อทราบการเขียนโปรแกรมเชื่อมต่อไวไฟและเว็บเซอร์เวอร์

## อุปกรณ์ที่ใช้
1. ไมโครคอนโทรเลอร์ (ESP-01)
2. อุปกรณ์ต่อ USB (USB to serial)
3. CPU
4. ตัวโปรแกรมเชื่อมต่อไวไฟและเว็บเซอร์เวอร์ (05_Wifi-Web-Server)

## ศึกษาข้อมูลเบื้องต้น
1. 05 run wifi  : https://www.youtube.com/watch?v=VX-QNQcO-b4
2. src code ของโปรแกรม 05_Wifi-Web-Server : https://github.com/choompol-boonmee/lab63b/blob/master/examples/05_Wifi-Web-Server/src/main.cpp

## วิธีการทำทดลอง
1. เข้าcommand prompt
2. เข้าตัวอย่างโปรแกรมจากโดยใช้พิมพ์คำสั่ง **cd pattani** ในหน้า command prompt
3. เลือกตัวอย่างโปรแกรมคำสั่ง **cd 05_Wifi-Web-Server** 
4. พิมพ์คำสั่ง **vi src/main.cpp** เมื่อกด Enter จะขึ้นโค้ดดังนี้
```javascript
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "HI_BMFWIFI_2.4G";
const char* password = "0819110933";

ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(115200);

	WiFi.mode(WIFI_STA);
	WiFi.begin(ssid, password);
	while (WiFi.status() != WL_CONNECTED) {
		delay(500);
		Serial.print(".");
	}
	Serial.print("\n\nIP address: ");
	Serial.println(WiFi.localIP());

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
 เนื่องจากโปรแกรมนี้ต้องเชื่อมต่อกับไวไฟดังนั้นต้องใส่ชื่อ wifi และ password ก่อน ตัวโปรแกรมแบ่งเป็น 2 ส่วน ได้แก่ 
 
 1.Setup เป็นการ connect กับไวไฟที่เราใส่ชื่อไว้ตอนแรก และ setup webserver โดยให้แสดงผลเป็น Hello cnt โดย cnt จะบวก 1 ขึ้นเรื่อยๆ 
 
 2.Loop 

5. อัพโหลดโปรแกรมเข้าไมโครคอนโทรเลอร์ด้วยคำสั่ง **pio run -t upload** 
6. ต่อไมโครคอนโทรเลอร์กับ USB to serial
7. กดปุ่มอัพโหลดและกดปุ่มรีเซตที่ตัวไมโครคอนโทรเลอร์เพื่อให้ตัวโปรแกรมอัพโหลดเข้าไปในตัว่ไมโครคอนโทรเลอร์
8. **pio device monitor** เพื่อดูผลลัพท์ โดยจะขึ้น ip address
9. ให้ copy ip address ไปที่บราวเซอร์ในการทดสอบ

## การบันทึกผลการทดลอง


1. เขียนโปรแกรมไมโครคอนโทรเลอร์ด้วยโปรแกรม 05_Wifi-Web-Server
	
	จะเห็นว่าจากคำสั่ง **pio device monitor** เพื่อดูผลลัพธ์ของการรันโปรแกรมในไมโครคอนโทรเลอร์ ได้ขึ้น ip address และเมื่อ copy ip address ไปที่บราวเซอร์ก็จะขึ้น Hello 1  โดย cnt ก็จะเปลี่ยนตัวแปรไปเรื่อยๆ

## อภิปรายผลการทดลอง

จากการทดลองเขียนโปรแกรมสามารถสรุปได้ว่า เราสามารถให้ไมโครคอนโทรเลอร์เชื่อมต่อไวไฟและเว็บเซิร์ฟเวอร์สามารถทำงานได้เหมือนเว็บเซิร์ฟเวอร์ทั่วไป

## คำถามหลังการทดลอง


