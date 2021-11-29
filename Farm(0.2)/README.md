#include <DHT.h>

#define DHT22PIN 8

#define DHTTYPE DHT22

DHT dht(DHT22PIN, DHTTYPE);

float h,tc,tf;

#include <Adafruit_NeoPixel.h>

#include <LiquidCrystal.h>

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

#define LED_MODULE_PIN    7

// количество кластеров

#define CLASTER_COUNT     60

Adafruit_NeoPixel matrix = Adafruit_NeoPixel(CLASTER_COUNT, LED_MODULE_PIN, NEO_GRB + NEO_KHZ800);

const int buzzer = 9;

const int AirValue = 600;   //you need to replace this value with Value_1

const int WaterValue = 350;  //you need to replace this value with Value_2

int soilMoistureValue = 0;

int soilmoisturepercent=0;

void setup() {

Serial.begin(9600); // open serial port, set the baud rate to 9600 bps

lcd.begin(16, 2);

pinMode(13, OUTPUT);



pinMode(buzzer, OUTPUT);

matrix.begin();



delay(500);

Serial.begin(9600);

dht.begin();

delay(1000);

lcd.print("DHT22 Temperature and Humidity ");

delay(500);

lcd.clear();
    
}
void loop(){ 

soilMoistureValue = analogRead(A0);  

Serial.println(soilMoistureValue);

soilmoisturepercent = map(soilMoistureValue, AirValue, WaterValue, 0, 100);

if(soilmoisturepercent >= 100)

{

Serial.println("100 %");

lcd.setCursor(0, 0);

lcd.print("Soil Moisture");

lcd.setCursor(0, 1);

lcd.print("100 %");

delay(250);

lcd.clear();

}

else if(soilmoisturepercent <=60)

{

Serial.println("0 %");

lcd.setCursor(0, 0);

lcd.print("Soil Moisture");

lcd.setCursor(0, 1);

lcd.print("0 %");

delay(2000);

lcd.clear();

tone(buzzer, 1000);

delay(150);

noTone(buzzer);

delay(150);

}

else if(soilmoisturepercent >0 && soilmoisturepercent < 100)

{

Serial.print(soilmoisturepercent);

Serial.println("%");

lcd.setCursor(0, 0);

lcd.print("Soil Moisture");

lcd.setCursor(0, 1);

lcd.print(soilmoisturepercent);

lcd.print(" %");

delay(640);

lcd.clear();

}  

  h = dht.readHumidity();
    
  lcd.print('\n');

lcd.print("Humidity = ");

lcd.print(h);

lcd.print("%,  ");

delay(600);

lcd.clear();

tf = dht.readTemperature(true);

lcd.print('\n');

lcd.print("Temperature- ");

lcd.print(tf);

lcd.print("°F");

delay(600);

lcd.clear();

tc = dht.readTemperature();

lcd.print('\n');

lcd.print("Temperature- ");

lcd.print(tc);

lcd.print("°C");

delay(600);

lcd.clear();

digitalWrite(13, HIGH); // sets the digital pin 13 on
  
  delay(9600);            // waits for a second
  
  digitalWrite(13, LOW);  // sets the digital pin 13 off
  
  delay(640); 
  
  colorWipe(matrix.Color(255, 0, 0), 50);
  
  colorWipe(matrix.Color(0, 0, 255), 50);
  
  colorWipe(matrix.Color(0, 0, 0), 50);

}

void colorWipe(uint32_t c, uint8_t wait)

{
  for (uint16_t i = 0; i < matrix.numPixels(); i++) {

// заполняем текущий сегмент выбранным цветом

matrix.setPixelColor(i, c);

matrix.show();

// ждём

delay(96);

}
}
