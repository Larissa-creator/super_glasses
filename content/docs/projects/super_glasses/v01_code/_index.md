---
title: V01's code
type: docs
---

# First version's code

<pre>
<code>
#include &lt;Servo.h&gt;

// create servo object to control a servo
Servo leftServo;
Servo rightServo;

// variable to store the servo position
int leftServoPos = 0; 
int rightServoPos = 0;

// pin position
int leftServoPin = 10; 
int rightServoPin = 9;
int buttonPin = 2;

// time in ms for the servo to reach the position
int delaytime = 15; 

// state of the push button
int switchState = 0;

int maxRot = 85;

void setup() {
  // put your setup code here, to run once:
}

void loop() {
  // read the state of the push button
  switchState = digitalRead(2);

  if (switchState == HIGH) {
    // if the button is pressed

    // attach the pin position for the servo to the respective servo
    leftServo.attach(leftServoPin);
    rightServo.attach(rightServoPin);

    // moves the wippers down
    for (int i = 0; i <= maxRot; i += 1) {
      // in steps of 1 degree
      leftServoPos = i;
      rightServoPos = 180 - i;

      // tell servo to go to position in variable 'pos'
      leftServo.write(leftServoPos); 
      rightServo.write(rightServoPos);

      // waits delaytime for the servo to reach the position
      delay(delaytime);  //
    }

    // moves the wippers up
    for (int i = maxRot; i >= 0; i -= 1) {
      // in steps of 1 degree
      leftServoPos = i;
      rightServoPos = 180 - i;

      // tell servo to go to position in variable 'pos'
      leftServo.write(leftServoPos); 
      rightServo.write(rightServoPos);

      // waits delaytime for the servo to reach the position
      delay(delaytime);
    }
    
    // detach the pin position from the servo
    leftServo.detach();
    rightServo.detach();
  }
}
</code>
</pre>