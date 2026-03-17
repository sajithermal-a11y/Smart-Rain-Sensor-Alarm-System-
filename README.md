# Smart-Rain-Sensor-Alarm-System-
#include <Servo.h>

Servo myServo;

int rainPin = 8;
int ledPin = 9;
int buzzerPin = 11;
int servoPin = 10;

int pos;
bool rainDetected = false;

void setup() {

  pinMode(rainPin, INPUT);
  pinMode(ledPin, OUTPUT);
  pinMode(buzzerPin, OUTPUT);

  myServo.attach(servoPin);
  myServo.write(0);
}

void loop() {

  int rainState = digitalRead(rainPin);

  // Rain detected first time
  if (rainState == LOW && rainDetected == false) {

    for (pos = 0; pos <= 90; pos++) {

      myServo.write(pos);

      digitalWrite(ledPin, HIGH);
      digitalWrite(buzzerPin, HIGH);
      delay(70);

      digitalWrite(ledPin, LOW);
      digitalWrite(buzzerPin, LOW);
      delay(70);
    }

    rainDetected = true;   // remember rain condition
  }

  // When completely dry
  if (rainState == HIGH && rainDetected == true) {

    delay(3000);   // wait 3 seconds to confirm dry

    if (digitalRead(rainPin) == HIGH) {

      for (pos = 90; pos >= 0; pos--) {

        myServo.write(pos);
        delay(70);
      }

      rainDetected = false;
    }
  }

}
