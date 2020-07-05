# IOT_HIRSIZ_ALARM_S-STEM-
IOT İLE HIRSIZ ALARM SİSTEMİ (raspery pi)
IOT İLE HIRSIZ ALARM SİSTEMİ

170202122 Fatih KARAMAN

Bilgisayar Mühendisliği Bölümü Kocaeli Üniversitesi


 
Giriş
Projemizde Iot uygulaması olarak hırsız alarm sistemi tasarladık. Sistemde evin belli noktalarına yerleştirlmiş hareket sensörleri üzerinden algınan hareketler hırsızın eve girmiş olması durumu  saptamaktadır. Hareket sensöründen hareket algılandığında ev içinde yüksek sesli alarm ve kırmızı ışık ikaz aydınlatmalarıyla hırsıza çaydırıcı bir tepki sergilenmektedir. Tabiki böyle bir durum gerçekleştiğinde yani hırsız eve girdiğinde hareket sensörleri bu durumu algılayarak bu sesli ve aydınlatmalı ikazın yanı sıra acil bir şekilde ev sahibinin  telefonunan bu durumu MQTT ile bildirim göndermektedir. Böylece ev sahibi evine hırsızın girdiğinden haberdar olup en kısa sürede duruma müdahele edebilecek veya hızla ilgilelere haber verebilecektir.

Amaç
Bu derste, bir ev davetsiz misafir alarm sistemi yapmak için Raspberry Pi, PIR hareket sensörü ve zil kullanacağız. PIR hareket sensörü tarafından bir davetsiz misafir tespit edildiğinde, sesli uyarı bip sesi çıkarır ve Raspberry Pi, internet üzerinden uzak MQTT istemci cihazına alarm mesajı gönderir.

Temel Bilgiler
Proje gelişiminde;
Python  programlama dili  kullanılmıştır.

Donanım İhtiyaçları
•	1 x Raspberry Pi 2 veya 3
•	1 x PIR Hareket sensörü ( Motion Sensör )
•	1 x Sesli Uyarı ( Buzzer )
•	Breadboards ve jumper telleri.

Donanım Kurulumu
•	Pi GPIO 17'ye PIR sensörü sinyal pimi (sarı pim) bağlayın
 
•	Buzzer sinyal pimi PI GPIO 17'e bağlanır
 


•	PIR sensörü kırmızı pimi 5V bağlayın
•	GND'ye PIR sensörü ve zil siyah pimi bağlayın

 
 

Yazılım
•	Hareket Sensorü İle Hareket AlgılamaSesli Alarm Verme
•	Işıklı Alarm Verme
•	MQTT İle Bildirim gönderimi
•	sudo apt-get install mosquitto-clients
 
•	sudo mosquitto_sub -h "test.mosquitto.org" -t  "osoyoo/#" -v

 
PIR sensörü hareket algıladığında, istemci terminal penceresinde “osoyoo / alert: Evde Hareket algılandı” mesajı görürsünüz. Bu pi'nin broker.mqtt-dashboard.com brokerine bir mqtt mesajı yayınladığı ve abonelik yoluyla böyle bir mesaj aldığınız anlamına gelir .
			
•	MQTT İle Mobilden Bildirim Alımı


  
  


 

Sistemin Çalışmasında Yazılan Python Kodu

import time
import RPi.GPIO as io
io.setmode(io.BCM)
import paho.mqtt.client as mqtt # Import the MQTT library
from gpiozero import Buzzer
pir_pin = 18
buzzer=Buzzer(17)
buzzer.off()
io.setup(pir_pin, io.IN) # activate input

def messageFunction (client, userdata, message):
 topic = str(message.topic)
 message = str(message.payload.decode("utf-8"))
 print(topic + message)	

ourClient = mqtt.Client("makerio_mqtt") # Create a MQTT client object

ourClient.connect("test.mosquitto.org", 1883) # Connect to the test MQTT broker

ourClient.subscribe("osoyoo/#") # Subscribe to the topic AC_unit

ourClient.on_message = messageFunction # Attach the messageFunction to subscription

ourClient.loop_start() # Start the MQTT client

while True:
	if io.input(pir_pin):
 	 print("Evde Hareket Algilandi.")
	 buzzer.on()
	 ourClient.publish("osoyoo/", "		Dikkat! Eve hirsiz girdi!!!") # Publish message to MQTT broker	time.sleep(4)
 	buzzer.off()


 
				PROGRAMIN ÇALIŞMASINDA BAZI EKRAN GÖRÜNTÜLERİ
 
 






 
 
        
 



Kaynaklar
[1]	https://www.youtube.com/watch?v=6CFTNx-R6-M
[2]	http://osoyoo.com/2016/09/08/use-raspberry-pi-and-pir-motion-sensor-to-make-iot-home-alarm-system/
[3]	https://www.youtube.com/watch?v=KamZ5x1OykQ
[4]	https://maker.robotistan.com/raspberry-pi-dersleri-10-pir-sensor-ile-hareket-algilama/?utm_source=youtube&utm_medium=aciklama&utm_campaign=rpi9
[5]	https://electronics.stackovernet.com/tr/q/49483
[6]	https://www.youtube.com/watch?v=6CFTNx-R6-M
[7]	https://www.youtube.com/watch?v=Tw0mG4YtsZk&feature=youtu.be
[8]	http://osoyoo.com/2016/09/08/use-raspberry-pi-and-pir-motion-sensor-to-make-iot-home-alarm-system/
[9]	https://www.youtube.com/watch?v=6CFTNx-R6-M
[10]	https://circuitdigest.com/microcontroller-projects/raspberry-pi-iot-intruder-alert-system
[11]	https://www.electronicsweekly.com/blogs/gadget-master/raspberry-pi-gadget-master/how-to-build-your-own-raspberry-pi-home-alarm-system-2013-07/
[12]	https://www.researchgate.net/publication/330715621_IoT_Based_Home_Security_System_Using_Raspberry_Pi-3
[13]	https://hackaday.io/project/26336-diy-cheap-safety-alarm-system-w-raspberry-pi
[14]	https://www.youtube.com/watch?v=k8fnjcnf6uI
[15]	https://www.protectamerica.com/home-security-blog/tech-tips/protection-one-home-security-systems-options_23242
[16]	https://www.instructables.com/id/Home-Security-With-Raspberry-Pi/
[17]	http://www.davidhunt.ie/alarmpi-raspberry-pi-security-alarm-pt-2/
[18]	https://circuitdigest.com/microcontroller-projects/raspberry-pi-iot-intruder-alert-system
[19]	https://www.google.com/search?q=raspberry+pi+linux&sa=X&ved=2ahUKEwiP97-m6dDmAhV0SxUIHQksDQ44ChDVAigCegQICxAD&biw=713&bih=657
[20]	https://ubuntu.com/download/raspberry-pi
[21]	https://www.electromaker.io/blog/article/12-best-linux-operating-systems-for-the-raspberry-pi
[22]	https://www.fossmint.com/operating-systems-for-raspberry-pi/
[23]	https://elinux.org/RPi_raspi-config
[24]	https://desertbot.io/blog/headless-raspberry-pi-4-ssh-wifi-setup/
[25]	https://www.youtube.com/watch?v=TbUQpcZRCGo
[26]	https://hackernoon.com/raspberry-pi-headless-install-462ccabd75d0
[27]	https://www.instructables.com/id/Ultimate-Raspberry-Pi-Configuration-Guide/
[28]	https://www.findchips.com/search/RASPBERRY%20PI%203?gclid=EAIaIQobChMIkMHynOnQ5gIVSrTtCh21xQ-dEAAYASAAEgJVUPD_BwE&gclsrc=aw.ds


Kazanımlar: Geliştirdiğimiz bu proje, IOT uygulamalarında kullanılan donanım, yazılım, tasarım, bulut platformlar, kullanılan protokol vb. konular üzerinde geniş araştımalar yapmamızı sagladı. Bu alan üzerinde ilk projemizi ortaya çıkararak teorik öğrendiklerimizi pratiğe dökebilmiş olduk. 


	 


