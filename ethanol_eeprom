#include <EEPROM.h> //EEPROM memory lib
#include "SparkFun_SGP30_Arduino_Library.h" //SGP30
#include <Wire.h> //I2C
#include <SPI.h> //SPI
SGP30 mySensor; //create an object of the SGP30 class
SGP30ERR error;
long t1, t2;
int tv; byte btv; //tVOC values
int memiter=0; //The current memory location
void setup() {
  // Setup Serial
  while (!Serial);
  delay(1000);
  Serial.begin(9600);
  //Setup Connection LED
  pinMode(13,OUTPUT);
  pinMode(12,OUTPUT);
  digitalWrite(13,LOW);
  //Setup sensor
  Wire.begin();
  //Sensor supports I2C speeds up to 400kHz
  Wire.setClock(400000);
  //Initialize sensor
  if (mySensor.begin() == false) {
    Serial.println("No SGP30 Detected. Check connections."); //Check light also
    digitalWrite(13,HIGH);
    while (1);
  }
  //Initializes sensor for air quality readings
  //measureAirQuality should be called in one second increments after a call to initAirQuality
  mySensor.initAirQuality();
  t1 = millis();
}
void loop() {
  //First fifteen readings will be
  //CO2: 400 ppm  TVOC: 0 ppb
  t2 = millis();
  if ( t2 >= t1 + 2000); //only will occur if 2 seconds have passed
  {
    t1 = t2;  
    //measure CO2 and TVOC levels
    error = mySensor.measureAirQuality();
    if (error == SUCCESS) {
      tv=mySensor.TVOC; //Get tVOC data, if sensor still booting...
      if (mySensor.CO2==400){
        delay(10); //Wait before checking again. 
      } else {
        Serial.println(tv); //Print the data to serial monitor (for debugging)
        btv=byte(tv); //Convert tVOC value to byte so that we can save it
        EEPROM.write(memiter,tv); //Write to EEPROM at address memiter
        memiter++; //Go to the next address slot in the EEPROM
        if (tv>=10) {digitalWrite(12,HIGH);} else {digitalWrite(12,LOW);}
        t1=millis();
        delay(1990);
      }
    }
    else if (error == ERR_BAD_CRC) {
      Serial.println("CRC Failed"); //CRC was a failure
      digitalWrite(13,HIGH);
    }
    else if (error == ERR_I2C_TIMEOUT) {
      Serial.println("I2C Timed out"); //I2C Timed out, check connections and LED
      digitalWrite(13,HIGH);
    }
  }
}


