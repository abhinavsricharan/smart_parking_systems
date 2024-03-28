# smart_parking_systems
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

#define TRIG_PIN_1 2
#define ECHO_PIN_1 3
#define TRIG_PIN_2 4
#define ECHO_PIN_2 5
#define TRIG_PIN_3 6
#define ECHO_PIN_3 7

#define DISTANCE_THRESHOLD 20 // Distance threshold in centimeters

// Define I2C address for the LCD module
#define I2C_ADDR 0x27

// Initialize LCD object with I2C address
LiquidCrystal_I2C lcd(I2C_ADDR, 16, 2); // 16x2 LCD

void setup() {
  pinMode(TRIG_PIN_1, OUTPUT);
  pinMode(ECHO_PIN_1, INPUT);
  pinMode(TRIG_PIN_2, OUTPUT);
  pinMode(ECHO_PIN_2, INPUT);
  pinMode(TRIG_PIN_3, OUTPUT);
  pinMode(ECHO_PIN_3, INPUT);

  Serial.begin(9600);
  lcd.init();      // Initialize LCD
  lcd.backlight(); // Enable backlight

  // Display welcome message
  lcd.setCursor(0, 0);
  lcd.print("Smart Parking");
  lcd.setCursor(0, 1);
  lcd.print("System");
}

void loop() {
  bool slot1Occupied = isSlotOccupied(TRIG_PIN_1, ECHO_PIN_1);
  bool slot2Occupied = isSlotOccupied(TRIG_PIN_2, ECHO_PIN_2);
  bool slot3Occupied = isSlotOccupied(TRIG_PIN_3, ECHO_PIN_3);

  // Update LCD display
  lcd.clear(); // Clear previous readings
  lcd.setCursor(0, 0);
  lcd.print("Slot 1: ");
  lcd.print(slot1Occupied ? "Occupied" : "Open");

  lcd.setCursor(0, 1);
  lcd.print("Slot 2: ");
  lcd.print(slot2Occupied ? "Occupied" : "Open");

  lcd.setCursor(0, 2);
  lcd.print("Slot 3: ");
  lcd.print(slot3Occupied ? "Occupied" : "Open");

  delay(1000); // Delay for readability
}

bool isSlotOccupied(int trigPin, int echoPin) {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  float duration = pulseIn(echoPin, HIGH);
  float distance = duration * 0.034 / 2;

  return distance < DISTANCE_THRESHOLD;
}


// servo code for entry and exit gate 
#include <Servo.h>



Servo servo;



int trigPin = 11;

int echoPin = 12;



// defines variables

long duration;

int distance;



void setup() 

{

  servo.attach(13);

  servo.write(180);

 delay(2000);

  

// Sets the trigPin as an Output

pinMode(trigPin, OUTPUT);

// Sets the echoPin as an Input 

pinMode(echoPin, INPUT);

}

void loop() 

{

// Clears the trigPin

digitalWrite(trigPin, LOW);

delayMicroseconds(2);

// Sets the trigPin on HIGH state for 10 micro seconds

digitalWrite(trigPin, HIGH);

delayMicroseconds(10);

digitalWrite(trigPin, LOW);

// Reads the echoPin, returns the sound wave travel time in microseconds

duration = pulseIn(echoPin, HIGH);

// Calculating the distance

distance= duration*0.034/2;

// Prints the distance on the Serial Monitor

Serial.print("Distance: ");

Serial.println(distance);

if ( distance <= 25   ) // Change Distance according to Ultrasonic Sensor Placement

 {



servo.write(180);

delay(3000);

 } 

else 

{

// blynk app for displaying the output shown in the lcd this output will be displayed in the blynk app
#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

char auth[] = "YourAuthToken";  // Blynk authentication token
char ssid[] = "YourNetworkName"; // Your Wi-Fi network name
char pass[] = "YourPassword";    // Your Wi-Fi network password

#define BLYNK_TEMPLATE_ID "YourTemplateID"     // Replace with your Blynk template ID
#define BLYNK_TEMPLATE_NAME "YourTemplateName" // Replace with your Blynk template name

#define TRIG_PIN_1 2
#define ECHO_PIN_1 3
#define TRIG_PIN_2 4
#define ECHO_PIN_2 5
#define TRIG_PIN_3 6
#define ECHO_PIN_3 7

#define DISTANCE_THRESHOLD 20 // Distance threshold in centimeters

BlynkTimer timer;

void setup() {
  Serial.begin(9600);
  Blynk.begin(auth, ssid, pass);
  pinMode(TRIG_PIN_1, OUTPUT);
  pinMode(ECHO_PIN_1, INPUT);
  pinMode(TRIG_PIN_2, OUTPUT);
  pinMode(ECHO_PIN_2, INPUT);
  pinMode(TRIG_PIN_3, OUTPUT);
  pinMode(ECHO_PIN_3, INPUT);

  timer.setInterval(1000L, sendSensorData); // Run every second
}

void loop() {
  Blynk.run();
  timer.run();
}

void sendSensorData() {
  float distance_1 = getDistance(TRIG_PIN_1, ECHO_PIN_1);
  float distance_2 = getDistance(TRIG_PIN_2, ECHO_PIN_2);
  float distance_3 = getDistance(TRIG_PIN_3, ECHO_PIN_3);

  // Check parking slot occupancy
  bool slot1Occupied = distance_1 < DISTANCE_THRESHOLD;
  bool slot2Occupied = distance_2 < DISTANCE_THRESHOLD;
  bool slot3Occupied = distance_3 < DISTANCE_THRESHOLD;

  // Send occupancy status to Blynk app
  Blynk.virtualWrite(V1, slot1Occupied ? "Occupied" : "Open");
  Blynk.virtualWrite(V2, slot2Occupied ? "Occupied" : "Open");
  Blynk.virtualWrite(V3, slot3Occupied ? "Occupied" : "Open");
}

float getDistance(int trigPin, int echoPin) {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  float duration = pulseIn(echoPin, HIGH);
  float distance = duration * 0.034 / 2;
  return distance;
}

servo.write(90);



 }



}

