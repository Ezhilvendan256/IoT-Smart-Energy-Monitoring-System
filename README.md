/*
  IoT-Based Smart Energy Monitoring System
  Academic / Portfolio Project Design

  Board: ESP32
  Note:
  This is demonstration code for a project design.
  Sensor calibration must be performed on real hardware.
*/

#include <WiFi.h>

// Wi-Fi credentials
const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";

// Example analog input pins
const int voltagePin = 34;
const int currentPin = 35;

// Example calibration values
// Replace these after testing/calibration.
float voltageCalibration = 0.10;
float currentCalibration = 0.01;

float voltage = 0.0;
float current = 0.0;
float power = 0.0;
float energy = 0.0;

// Example assumed power factor.
// For a real system, PF should be measured appropriately.
float powerFactor = 0.90;

unsigned long previousTime = 0;

void setup() {

  Serial.begin(115200);

  pinMode(voltagePin, INPUT);
  pinMode(currentPin, INPUT);

  Serial.println("Smart Energy Monitoring System");
  Serial.println("Connecting to Wi-Fi...");

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println();
  Serial.println("Wi-Fi Connected");

  previousTime = millis();
}

void loop() {

  // Read sensors
  int voltageADC = analogRead(voltagePin);
  int currentADC = analogRead(currentPin);

  // Demonstration conversions
  voltage = voltageADC * voltageCalibration;
  current = currentADC * currentCalibration;

  // Calculate real power
  power = voltage * current * powerFactor;

  // Calculate elapsed time
  unsigned long currentTime = millis();

  float timeHours =
      (currentTime - previousTime) / 3600000.0;

  // Energy in Wh
  energy += power * timeHours;

  previousTime = currentTime;

  // Display values
  Serial.println("-----------------------");

  Serial.print("Voltage: ");
  Serial.print(voltage);
  Serial.println(" V");

  Serial.print("Current: ");
  Serial.print(current);
  Serial.println(" A");

  Serial.print("Power: ");
  Serial.print(power);
  Serial.println(" W");

  Serial.print("Energy: ");
  Serial.print(energy);
  Serial.println(" Wh");

  Serial.print("Power Factor: ");
  Serial.println(powerFactor);

  /*
     IoT upload function can be added here.

     Example:
     uploadData(voltage, current, power, energy);
  */

  delay(2000);
}
