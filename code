#include <Wire.h>

const int MPU_ADDR = 0x68;

int16_t gyroX, gyroY, gyroZ;
float gyroXcal, gyroYcal, gyroZcal;

float yaw = 0.0;

unsigned long previousTime, currentTime;

float elapsedTime;

void setup() {
  Wire.begin();
  Serial.begin(9600);

  Wire.beginTransmission(MPU_ADDR);
  Wire.write(0x6B); 
  Wire.write(0);   
  Wire.endTransmission(true);

  Wire.beginTransmission(MPU_ADDR);
  Wire.write(0x1B);
  Wire.write(0x00);
  Wire.endTransmission(true);

  calibrateGyro();

  previousTime = millis(); 

void loop() {
  readGyro();

  currentTime = millis();
  elapsedTime = (currentTime - previousTime) / 1000.0; 
  previousTime = currentTime;

  yaw += (gyroZ - gyroZcal) * elapsedTime; 

  Serial.print("Yaw: ");
  Serial.println(yaw);

  delay(10);
}

void readGyro() {
  Wire.beginTransmission(MPU_ADDR);
  Wire.write(0x43); 
  Wire.endTransmission(false);
  Wire.requestFrom(MPU_ADDR, 6, true);

  gyroX = (Wire.read() << 8) | Wire.read(); 
  gyroY = (Wire.read() << 8) | Wire.read(); 
  gyroZ = (Wire.read() << 8) | Wire.read(); 
}

void calibrateGyro() {
  int numReadings = 2000;
  long gyroXsum = 0, gyroYsum = 0, gyroZsum = 0;

  for (int i = 0; i < numReadings; i++) {
    readGyro();
    gyroXsum += gyroX;
    gyroYsum += gyroY;
    gyroZsum += gyroZ;
    delay(3); 
  }

  gyroXcal = gyroXsum / numReadings;
  gyroYcal = gyroYsum / numReadings;
  gyroZcal = gyroZsum / numReadings;
}
