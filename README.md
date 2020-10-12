# Modulo-RF-entre-arduinos-e-BME280
#include <HID.h>

// Include RadioHead Amplitude Shift Keying Library
#include <RH_ASK.h>
// Include dependant SPI Library 
#include <SPI.h> 

#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_BME280.h>
// Create Amplitude Shift Keying Object
RH_ASK rf_driver;


#define SEALEVELPRESSURE_HPA (1013.25)

Adafruit_BME280 bme;

String msg1, msg2, msg3, msg4;

int count = 0;


void setup()
 
{
   Serial.begin(9600);
    if (!bme.begin(0x76)) {
      Serial.println("Could not find a valid BME280 sensor, check wiring!");
    while (1);
  }
  {
    // Initialize ASK Object
    rf_driver.init();
}
}
 
void loop()
{          

const char *msgfinal = "                               ";


  if(count == 0){
      Serial.println("Enviando informações 1" );
    msg1 = "Temperatura = ";
    msg1 += bme.readTemperature();
    msg1 += " °C ";
    // Temperatura
    const char msg_1[50];
    msg1.toCharArray(msg_1, 50);
    rf_driver.send((uint8_t *)msg_1, strlen(msg_1)); 
    rf_driver.waitPacketSent();
    count++;
    delay(500);
    }
     if(count == 1){
      Serial.println("Enviando informações 2" );
    msg2 = "Pressão = " ;
    msg2 += bme.readPressure() / 100.0F;
    msg2 += " hPa ";
       // Pressão
    const char msg_2[50];
    msg2.toCharArray(msg_2, 50);
    rf_driver.send((uint8_t *)msg_2, strlen(msg_2));
    rf_driver.waitPacketSent();
    count++;
    delay(500);
    }
      if(count == 2){
        Serial.println("Enviando informações 3" );
    msg3 = "Altitude = ";
    msg3 += bme.readAltitude(SEALEVELPRESSURE_HPA);
    msg3 += " m ";
     // Altitude
    const char msg_3[50];
    msg3.toCharArray(msg_3, 50);
    rf_driver.send((uint8_t *)msg_3, strlen(msg_3));
    rf_driver.waitPacketSent();
    count++;
    delay(500);
    }
  
    
   if(count == 3){ 
    Serial.println("Enviando informações 4" );
    msg4 = "Umidade = ";
    msg4 += bme.readHumidity();
    msg4 += " % ";
    // Umidade
    const char msg_4[50];
    msg4.toCharArray(msg_4, 50);
    rf_driver.send((uint8_t *)msg_4, strlen(msg_4));
    rf_driver.waitPacketSent();
    count++;
    delay(2000); 
    }

   if(count == 4){ 
    
    rf_driver.send((uint8_t *)msgfinal, strlen(msgfinal));
    rf_driver.waitPacketSent();
    count++;
     }
   if(count == 5){ 
    
    rf_driver.send((uint8_t *)msgfinal, strlen(msgfinal));
    rf_driver.waitPacketSent();
    count++;
     }

    if(count == 6){ 
   
    rf_driver.send((uint8_t *)msgfinal, strlen(msgfinal));
    rf_driver.waitPacketSent();
    count = 0;
    delay(3000); 
 }
   
}

