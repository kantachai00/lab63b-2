# การทดลองที่ 1 เรื่อง การเขียนโปรแกรมค้นหาไวไฟ

## วัตถุประสงค์
1. เพื่อทราบการใช้คำสั่งต่างๆ ที่จะรันโปรแกรม
2. เพื่อทราบข้อมูลพื้นฐานของไมโครคอนโทรเลอร์ที่ใช้ในการทดลอง
3. เพื่อทราบการเขียนโปรแกรมค้นหาไวไฟ

## อุปกรณ์ที่ใช้
1. ไมโครคอนโทรเลอร์ (ESP-01)
2. อุปกรณ์ต่อ USB 
3. CPU
4. เสาอากาศสำหรับไวไฟ

## ศึกษาข้อมูลเบื้องต้น
1. 02 run example 2 : https://www.youtube.com/watch?v=yBjab0UNuB8
2. src code ของโปรแกรม 01_Serial-Monitor : https://github.com/choompol-boonmee/lab63b/blob/master/examples/01_Serial-Monitor/src/main.cpp

## วิธีการทำทดลอง
1. ต่อไมโครคอนโทรเลอร์เข้ากับซีเรียล
2. เข้าcommand prompt
3. เข้าตัวอย่างโปรแกรมจากโดยใช้พิมพ์คำสั่ง **cd pattani** ในหน้า command prompt
4. เลือกตัวอย่างโปรแกรมคำสั่ง **cd 02_Scan-Wifi**
5. พิมพ์คำสั่ง **vi src/main.cpp** เมื่อกด Enter จะขึ้นดังรูปนี้


    
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




6. พิมพ์คำสั่ง **vi platformio.ini** เมื่อกด Enter จะขึ้นดังรูปนี้
      
      
      
      
      

7. อัพโหลดโปรแกรมเข้าไมโครคอนโทรเลอร์ด้วยคำสั่ง **pio run -t upload** เมื่อกด Enter จะขึ้นดังรูปนี้






     

8. กดปุ่มรีเซตที่ตัวไมโครคอนโทรเลอร์
9. **pio device monitor** เพื่อดูผลลัพท์


      
      


## การบันทึกผลการทดลอง
จากการใช้คำสั่ง vi src/main.cpp ทำให้เราทราบว่าโปรแกรมตัวนี้ใช้เพื่อทดสอบโดยมี 2 ส่วน ดังนี้
1. ส่วน set up 
2. ส่วน loop
      ที่เพิ่มตัวแปร count และแสดงผล และมีการหน่วงเวลา 1000 ms

จากการใช้คำสั่ง vi platformio.ini ทำให้เราได้ทราบข้อมูลดังนี้
1. เป็น paltform ของ espressif8266                   
2. bord ชื่อ esp01_1m
3. ใช้วิธีการเขียนโปรแกรมแบบ arduino
4. port ใช้ของ windows เพราะขึ้นว่า com3

จากการใช้คำสั่ง pio device monitor 
1. จะเห็นว่าตัวแปร count จะเพิ่มขึ้นๆ ทุก 1 วินาที 
2. เมื่อกดปุ่มรีเซตที่ตัวไมโครคอนโทรเลอร์ ก็จะเริ่มนับหนึ่งใหม่
## อภิปรายผลการทดลอง
จากการทดลองการเขียนโปรแกรมเพื่อรันบนไมโครคอนโทรเลอร์จะเห็นว่าเราต้องการเขียนโปรแกรม 01_Serial-Monitor ลงในตัวไมโครคอนโทรเลอร์ของเรา โดยตัวโปรแกรมนี้จะทำการนับและเปลี่ยนตัวแปรทุกๆ 1 วินาที ซึ่งสามารถตรวจสอบได้ด้วยคำสั่ง **pio device monitor**
## คำถามหลังการทดลอง
ถาม เราสามารถ

