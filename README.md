#include <LiquidCrystal.h>  
#define echoPin 6
#define trigPin 7
#define buzzerPin 8

int maximumRange = 50;
int minimumRange = 0;
int sure;                                               
int uzaklik;                                            
int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;   
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);              


void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzerPin, OUTPUT);
  pinMode(trigPin, OUTPUT);                             
  pinMode(echoPin,INPUT);                               
  lcd.begin(16, 2);    
  Serial.begin(9600);

}

void loop() {

  int olcum = mesafe(maximumRange, minimumRange);
  melodi(olcum*10);

}

int mesafe(int maxrange, int minrange)
{
  long duration, distance;

  digitalWrite(trigPin,LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = duration / 58.2;
  delay(50);

  if(distance >= maxrange || distance <= minrange)
  return 0;
  return distance;
}

int melodi(int dly)
{
  tone(buzzerPin, 440);
  delay(dly);
  noTone(buzzerPin);
  delay(dly);
digitalWrite(trigPin, LOW);                          
  delayMicroseconds(5);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  sure = pulseIn(echoPin, HIGH, 11600);                 
  uzaklik= sure*0.0345/2;                              
  lcd.clear();                                          //LCD'deki eski 
  lcd.setCursor(0, 0);                                  //LCD'nin 1. satır 1. sütunundan yazmaya başlıyoruz.      
  lcd.print("Uzaklik:");                                
  lcd.setCursor(0, 1);                                  //LCD'nin 2. satır 1. sütunundan yazmaya başlıyoruz.
  lcd.print(uzaklik);                                   //Uzaklık değerini LCD'ye yazdırıyoruz.
  lcd.print("cm");
  Serial.println(uzaklik);
}
