#include <Adafruit_NeoPixel.h>
#define LED_MODULE_PIN    7

#define CLASTER_COUNT     60
 

Adafruit_NeoPixel matrix = Adafruit_NeoPixel(CLASTER_COUNT, LED_MODULE_PIN, NEO_GRB + NEO_KHZ800);
#include <LiquidCrystal.h>
const int buzzer = 9;
const int water_motor = 8;


LiquidCrystal lcd(12, 11, 5, 4, 3, 2);
 
const int AirValue = 600;  
const int WaterValue = 350;  
int soilMoistureValue = 0;
int soilmoisturepercent=0;
void setup() {
  Serial.begin(9600); 
  lcd.begin(16, 2);
  pinMode(13, OUTPUT);
  pinMode(water_motor, OUTPUT);
  pinMode(buzzer, OUTPUT);
  matrix.begin();
  
  
}
void loop(){ 

soilMoistureValue = analogRead(A0);  //put Sensor insert into soil
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
  delay(250);
  lcd.clear();
  tone(buzzer, 1000);
  delay(150);
  noTone(buzzer);
  delay(150);
  pinMode(water_motor, HIGH);
  delay(100);

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
digitalWrite(13, HIGH); 
  delay(9600);           
  digitalWrite(13, LOW);  
  delay(640); 
  colorWipe(matrix.Color(255, 0, 0), 50);
  colorWipe(matrix.Color(0, 0, 255), 50);
  colorWipe(matrix.Color(0, 0, 0), 50);
}
void colorWipe(uint32_t c, uint8_t wait)
{
  for (uint16_t i = 0; i < matrix.numPixels(); i++) {
    
    matrix.setPixelColor(i, c);
    matrix.show();
    
    delay(96);
  }
}
