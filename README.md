#include <Servo.h> 

Servo verticalServo; 
int servoVertical = 60; 
int servoVerticalLimitHigh = 175;
int servoVerticalLimitLow = 5;

int ldrTop = A1;    
int ldrBottom = A0; 

void setup() {
  Serial.begin(9600);
  verticalServo.attach(11); 
  verticalServo.write(servoVertical);
  delay(2000);
}

void loop() {
  int valueTop = analogRead(ldrTop);
  int valueBottom = analogRead(ldrBottom);

  int differenceVertical = valueTop - valueBottom;

  int tolerance = 25;   
  int delayTime = 15;   

  if (abs(differenceVertical) > tolerance) {
    if (valueTop > valueBottom) {
      servoVertical++; 
      if (servoVertical > servoVerticalLimitHigh) servoVertical = servoVerticalLimitHigh;
 
    } else {
      servoVertical--;  
     if (servoVertical < servoVerticalLimitLow) servoVertical = servoVerticalLimitLow;
    }
  }
    verticalServo.write(servoVertical);

  Serial.print("Top LDR: "); Serial.print(valueTop);
  Serial.print(" | Bottom LDR: "); Serial.print(valueBottom);
  Serial.print(" || Servo Vertical: "); Serial.println(servoVertical);

  delay(delayTime);
}
