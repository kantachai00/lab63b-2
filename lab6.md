# การทดลองที่ 6 เรื่อง การเขียนโปรแกรมสร้างไวไฟแอคเซสพอยต์ (Wifi AP)

## วัตถุประสงค์
1. เพื่อทราบการเขียนโปรแกรมสร้างไวไฟแอคเซสพอยต์
2. เพื่อนำไมโครคอนโทรเลอร์ที่เขียนโปรแกรมแล้วไปประยุกต์ใช้

## อุปกรณ์ที่ใช้
1. ไมโครคอนโทรเลอร์ (ESP-01)
2. อแดปเตอร์ที่ต่อสายพอร์ท 0 (สีขาว) กับ พอร์ท 2 (สีเหลือง)
3. อุปกรณ์ต่อ USB (USB to serial)
4. หลอดไฟ LED
5. CPU
6. ตัวโปรแกรมสร้างไวไฟแอคเซสพอยต์ (06_Wifi-AP-Web-Server)


## ศึกษาข้อมูลเบื้องต้น
1. 04 run example 4 : https://www.youtube.com/watch?v=nFqoZT26U5k
2. src code ของโปรแกรม 06_Wifi-AP-Web-Server : https://github.com/choompol-boonmee/lab63b/blob/master/examples/06_Wifi-AP-Web-Server/src/main.cpp

## วิธีการทำทดลอง
1. เอาอแดปเตอร์ต่อกับตัว USB to seria (หมายเหตุ ตัวอแดปเตอร์ที่ใช้ในการทดลองนี้มีการต่อกับ LED )
2. นำตัวไมโครคอนโทรเลอร์ต่อกับพอร์ท 
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
จากโค้ดนี้จะเห็นว่าเราต้องการให้ ESP 01 มีไวไฟในตัวเอง โดยจะกำหนดชื่อwifiและpasswoedก่อน ที่จะปล่อยให้เครื่องอื่น connect และมีการสร้าง

6. อัพโหลดโปรแกรมเข้าไมโครคอนโทรเลอร์ด้วยคำสั่ง **pio run -t upload** 
7. กดปุ่มรีเซตที่ตัวไมโครคอนโทรเลอร์เพื่อให้ตัวโปรแกรมอัพโหลดเข้าไปในตัว่ไมโครคอนโทรเลอร์
8. **pio device monitor** เพื่อดูผลลัพท์






![image](https://user-images.githubusercontent.com/80879772/111975374-1cefc280-8b33-11eb-90e7-43ce82ee3cee.png)


9. ลองใช้โทรศัพท์มือถือหรือคอมพิวเตอร์ค้นหาไวไฟที่สร้าง







![image](https://user-images.githubusercontent.com/80879772/111975598-5f190400-8b33-11eb-80c4-72690f3fcacd.png)

## การบันทึกผลการทดลอง
1. เขียนโปรแกรมไมโครคอนโทรเลอร์ด้วยโปรแกรม 06_Wifi-AP-Web-Server
	
	จะเห็นว่าจากคำสั่ง **pio device monitor** ได้ขึ้นคำว่า read 1 ซึ่งหมายความว่า input คือ 1 ทำให้เป็น low ที่พอร์ท 2 จะทำให้ไฟดับนั้นเอง 
	
	เมื่อจิ้มเส้นอินพุทไปที่สีดำซึ่งจะได้ค่า  input คือ 0 ทำให้เป็น high ไฟติด 
	
	เมื่อจิ้มเส้นอินพุทไปที่สีแดงซึ่งจะได้ค่า input คือ 1 ทำให้เป็น low  ไฟดับ
	
	กดปุ่มสีดำ                     input คือ 0 ทำให้เป็น high ไฟติด 
	
	ปล่อยปุ่ม		       input คือ 1 ทำให้เป็น low  ไฟดับ

2. การนำตัวไมโครคอนโทรเลอร์ที่่ได้ลงโปรแกรมแล้วมาประยุุกต์ใช้ต่อกับตัวเซนเซอร์แสง

	จะเห็นว่าเห็นว่า  เมื่อเอานิ้วบังแสง    ไฟดับ
		     เมื่อเอาไม่เอานิ้วบังแสง  ไฟติด
	
## อภิปรายผลการทดลอง
จากการทดลองเขียนโปรแกรมสามารถสรุปจากการโค้ดที่เขียนโปรแกรมได้ว่า สัญญาณอินพุทจากพอร์ท 0 คือตัวควบคุมให้ไฟเปิดหรือปิด ถ้า 1 ไฟดับ ถ้า 0 ไฟติด โดยดูไฟที่พอร์ท 2 

และในส่วนของการมาประยุกต์ใช้ต่อกับตัวเซนเซอร์แสง โดยเมื่อเอานิ้วบังแสง ทำให้แรงต้านสูงทำให้ค่า input 1 ไฟดับ และเมื่อสว่างทำให้แรงต้านทานน้อย input เป็น 0 ไฟติด

## คำถามหลังการทดลอง
ถาม ในชีวิตประจำวันเราได้ใช้เซนเซอร์แสงหรือไม่ พร้อมยกตัวอย่าง

ตอบ ได้ใช้ เช่นไฟข้างทาง 

