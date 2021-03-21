# การทดลองที่ 2 เรื่อง การเขียนโปรแกรมค้นหาไวไฟ

## วัตถุประสงค์
1. เพื่อทราบการเขียนโปรแกรมค้นหาไวไฟ

## อุปกรณ์ที่ใช้
1. ไมโครคอนโทรเลอร์ (ESP-01)
2. อุปกรณ์ต่อ USB 
3. CPU
4. เสาอากาศสำหรับไวไฟ

## ศึกษาข้อมูลเบื้องต้น
1. 02 run example 2 : https://www.youtube.com/watch?v=yBjab0UNuB8
2. src code ของโปรแกรม 02_Scan-Wifi : https://github.com/choompol-boonmee/lab63b/blob/master/examples/02_Scan-Wifi/src/main.cpp

## วิธีการทำทดลอง
1. ต่อไมโครคอนโทรเลอร์เข้ากับซีเรียล
2. เข้าcommand prompt
3. เข้าตัวอย่างโปรแกรมจากโดยใช้พิมพ์คำสั่ง **cd pattani** ในหน้า command prompt
4. เลือกตัวอย่างโปรแกรมคำสั่ง **cd 02_Scan-Wifi**
5. พิมพ์คำสั่ง **vi src/main.cpp** เมื่อกด Enter จะขึ้นโค้ดดังนี้


    
         #include <Arduino.h>
         #include <ESP8266WiFi.h>

        int cnt = 0;

        void setup()
        {
	      Serial.begin(115200);
	      WiFi.mode(WIFI_STA);
	      WiFi.disconnect();
	      delay(100);
	      Serial.println("\n\n\n");
        }

        void loop()
         {
	      Serial.println("========== เริ่มต้นแสกนหา Wifi ===========");
	      int n = WiFi.scanNetworks();
	    if(n == 0) {
		    Serial.println("NO NETWORK FOUND");
	    } else {
		  for(int i=0; i<n; i++) {
			Serial.print(i + 1);
			Serial.print(": ");
			Serial.print(WiFi.SSID(i));
			Serial.print(" (");
			Serial.print(WiFi.RSSI(i));
			Serial.println(")");
			delay(10);
		}
	   }
	    Serial.println("\n\n");
        }


6. อัพโหลดโปรแกรมเข้าไมโครคอนโทรเลอร์ด้วยคำสั่ง **pio run -t upload** 
7. กดปุ่มอัพโหลดบู๊ท(สีดำ)และกดปุ่มรีเซต(สีแดง)ที่ตัวไมโครคอนโทรเลอร์ เพื่อให้โปรแกรมอัพโหลดได้ โดยเมื่ออัพโหลดเสร็จแล้วจะขึ้นหน้าจอดังนี้







![image](https://user-images.githubusercontent.com/80879772/111913390-807ce000-8aa0-11eb-8585-8212565d4ff0.png)


8. **pio device monitor** เพื่อดูผลลัพท์






![image](https://user-images.githubusercontent.com/80879772/111913459-be7a0400-8aa0-11eb-8b18-52afd68525b4.png)


      
      


## การบันทึกผลการทดลอง
จากการใช้คำสั่ง **pio device monitor**   
จะเห็นว่าจะมีการเริ่มต้นสแกนหา Wifi ซึ่งจะขึ้นเป็นจำนวนและชื่อไวไฟที่ตรวจพบได้
## อภิปรายผลการทดลอง
จากการทดลองการเขียนโปรแกรมค้นหาไวไฟ จะเห็นว่าเมื่อเราอัพโหลดโปรแกรม 02_Scan-Wifi ลงในตัวไมโครคอนโทรเลอร์ และใช้คำสั่ง **pio device monitor** เพื่อดูการทำงานของไมโครคอนโทรเลอร์ ซึ่งหน้าจอจะขึ้นเป็นจำนวนและชื่อไวไฟที่ตรวจพบได้ ตามโปรแกรมที่เราได้อัพโหลดลงไป 
## คำถามหลังการทดลอง
ถาม ทำไมการแสกนไวไฟในแต่ละครั้งในสถานที่เดียวกันในเวลาที่ไลเลี่ยกันถึงขึ้นจำนวนและชื่อไวไฟไม่เท่ากัน
ตอบ เพราะการตรวจจับแสกนหาไวไฟในแต่ละครั้งจะขึ้นกับสัญญาณและความเร็วของไวไฟ

