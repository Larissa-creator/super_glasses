---
title: VEnd's code
type: docs
---

# Final version's code

<pre>
<code>
#include &lt;ESP32Servo.h&gt;
#include &quot;WiFi.h&quot;
#include &quot;ESPAsyncWebServer.h&quot;
#include &lt;Wire.h&gt;
#include &lt;SPI.h&gt;

// Replace with your network credentials
const char* ssid = &quot;wlanName&quot;; 
const char* password = &quot;wlanPassWord&quot;;

// Create AsyncWebServer object on port 80
AsyncWebServer server(80);

// stores the content of the index.html file we want the webserver to send
extern const char index_html[] PROGMEM;

const char* PARAM_INPUT_1=&quot;state&quot;;

// pin position of the light which tells if there is a connection
int led = LED_BUILTIN;

// create servo object to control a servo
Servo leftServo; 
Servo rightServo;

// variable to store the servo position
int leftServoPos = 90; 
int rightServoPos = 0;

// pin position
int leftServoPin = 32; 
int rightServoPin = 33;

// time in ms for the servo to reach the position
int delaytime = 15;

int switchState = 0;

int baseRot = 90;

// clean
int maxRot = 60;

// emotion delay
int emotionDelayTime = 2000;

// shock
int shockRot = 45;

// raisedEyebrow
int browRot = 45;

// anger
int angerRot = 20;

// wiggle
// angle of the big rotation
int wiggleRot = 30;
// angle of the small rotation
int wiggleDif = 20;
// pause between the big and small rotation
int wiggleEmotDelay = 500;
// pause between the small rotation
int wiggleDelay = 250;
// time in ms for the servo to faster reach the position
int wiggleUpdate = 5;

void resetState() {
  leftServo.write(baseRot); 
  rightServo.write(baseRot);
}

void rotateBothAngle(int base, int add, int increment, bool goUp, int delayTime) {

  // in steps of increment degree
  if(goUp) {
    // move the wippers up
    leftServoPos = base - add - increment;
    rightServoPos = base + add + increment;
  } else {
    // move the wippers down
    leftServoPos = base + add + increment;
    rightServoPos = base - add - increment;
  }

  // tell servo to go to position in variable 'pos'
  leftServo.write(leftServoPos); 
  rightServo.write(rightServoPos);

  // waits delaytime for the servo to reach the position
  delay(delayTime);
}

void clean() {

  // goes from baseRot maxRot degrees down
  for (int i = 0; i &lt;= maxRot; i += 1) {
    rotateBothAngle(baseRot, 0, i, false, delaytime);
  }

  // goes up again
  for (int i = maxRot; i &gt;= 0; i -= 1) {
    rotateBothAngle(baseRot, 0, i, true, delaytime);
  }
}

void shock() {

  // goes from baseRot shockRot degrees up
  for (int i = 0; i &lt;= shockRot; i += 1) {
    rotateBothAngle(baseRot, 0, i, true, delaytime);
  }

  delay(emotionDelayTime);

  // goes down again
  for (int i = shockRot; i &gt;= 0; i -= 1) {
    rotateBothAngle(baseRot, 0, i, false, delaytime);
  }
}

void raisedEyebrow() {

  // move the left wipper up
  for (int i = 0; i &lt;= browRot; i += 1) {
    // in steps of 1 degree
    leftServoPos = 90 - i;

    // tell servo to go to position in variable 'pos'
    leftServo.write(leftServoPos); 

    // waits delaytime for the servo to reach the position
    delay(delaytime);  //
  }

  delay(emotionDelayTime);

  // move it down again
  for (int i = browRot; i &gt;= 0; i -= 1) {
    // in steps of 1 degree
    leftServoPos = 90 - i;

    // tell servo to go to position in variable 'pos'
    leftServo.write(leftServoPos); 

    delay(delaytime);
  }
}

void anger() {

  // goes from baseRot angerRot degrees down
  for (int i = 0; i &lt;= angerRot; i += 1) {
    rotateBothAngle(baseRot, 0, i, false, delaytime);
  }

  delay(emotionDelayTime);

  // goes up again
  for (int i = angerRot; i &gt;= 0; i -= 1) {
    rotateBothAngle(baseRot, 0, i, true, delaytime);
  }
}

void wiggle() {
  int wiggleRemain = wiggleRot - wiggleDif;

  // goes from baseRot wiggleRot degrees up
  for (int i = 0; i &lt;= wiggleRot; i += 1) {
    rotateBothAngle(baseRot, 0, i, true, delaytime);
  }

  delay(wiggleEmotDelay);

  // goes wiggleDif down
  for (int i = wiggleDif; i &gt;= 0; i -= 1) {
    rotateBothAngle(baseRot, wiggleRemain, i, false, wiggleUpdate);
  }

  delay(wiggleDelay);

  // goes wiggleDif up
  for (int i = 0; i &lt;= wiggleDif; i += 1) {
    rotateBothAngle(baseRot, wiggleRemain, i, true, wiggleUpdate);
  }

  delay(wiggleDelay);

  // goes wiggleDif down
  for (int i = wiggleDif; i &gt;= 0; i -= 1) {
    rotateBothAngle(baseRot, wiggleRemain, i, false, wiggleUpdate);
  }

  delay(wiggleDelay);

  // goes wiggleDif up
  for (int i = 0; i &lt;= wiggleDif; i += 1) {
    rotateBothAngle(baseRot, wiggleRemain, i, true, wiggleUpdate);
  }

  delay(wiggleEmotDelay);

  // goes down to defaultstate
  for (int i = wiggleRot; i &gt;= 0; i -= 1) {
    rotateBothAngle(baseRot, 0, i, down, delaytime);
  }
}

void setup() {
  // Serial port for debugging purposes
  Serial.begin(4800);
  // set LED to be an output pin
  pinMode(led, OUTPUT);

  leftServo.attach(leftServoPin);
  rightServo.attach(rightServoPin);

  // Initialize Random
  unsigned long seed = millis();
  randomSeed(seed);

  // Connect to Wi-Fi
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println(&quot;Connecting to WiFi..&quot;);
  }

  // Print ESP32 Local IP Address
  Serial.println(WiFi.localIP());

  // Route for root / web page
  server.on(&quot;/&quot;, HTTP_GET, [](AsyncWebServerRequest *request) {
    request-&gt;send_P(200, &quot;text/html&quot;, index_html);
    if (WiFi.status() == WL_CONNECTED) {
      digitalWrite(led, HIGH);   // turn the LED on (HIGH is the voltage level)
    } else {
      digitalWrite(led, LOW);
    }
  });

  server.on(&quot;/update&quot;, HTTP_GET, [] (AsyncWebServerRequest *request) {
    if (WiFi.status() == WL_CONNECTED) {
      digitalWrite(led, HIGH);   // turn the LED on (HIGH is the voltage level)
    } else {
      digitalWrite(led, LOW);
    }
    String inputMessage1;
    // GET input1 value on &lt;ESP_IP&gt;/update?state=&lt;inputMessage1&gt;
    if (request-&gt;hasParam(PARAM_INPUT_1)) {
      inputMessage1 = request-&gt;getParam(PARAM_INPUT_1)-&gt;value();

      if(inputMessage1.toInt() == 6) {
        inputMessage1 = random(1, 6);
      }

      switch(inputMessage1.toInt()) {
        case 0:
          resetState();
          break;
        case 1:
          clean();
          break;
        case 2:
          shock();
          break;
        case 3:
          raisedEyebrow();
          break;
        case 4:
          anger();
          break;
        case 5:
          wiggle();
          break;
      }
    }
    else {
      inputMessage1 = &quot;No message sent&quot;;
    }

    Serial.print(&quot;\nState: &quot;);
    Serial.print(inputMessage1);
    request-&gt;send(200, &quot;text/plain&quot;, &quot;OK&quot;);
  });

  // Start server
  server.begin();
}

void loop() {
    if (WiFi.status() == WL_CONNECTED) {
      // turn the LED on (HIGH is the voltage level)
      digitalWrite(led, HIGH);   // turn the LED on (HIGH is the voltage level)
    } else {
      // turn the LED off (LOW is the voltage level)
      digitalWrite(led, LOW);
    }  
}

const char index_html[] PROGMEM = R&quot;(
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
  &lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1&quot;&gt;
    &lt;title&gt;Super Glasses Control&lt;/title&gt;
    &lt;style&gt;
      * {
        box-sizing: border-box;
        font-size: 16px;
        text-align: center;
        --main-color: green;
        --secondary-color: #002c00;
        --button-text-color: white;
        --one-px: 1/16;
        --transition-time: 0.25s;
      }

      body {
        font-family: Arial, Helvetica, sans-serif;
      }

      .capsule {
        display: flex;
        flex-direction: column;
        align-items: center;
      }

      h1 {
        font-size: 2rem;
      }

      button {
        min-height: 3rem;
        min-width: 3rem;
        padding: calc(13rem * var(--one-px)) calc(20rem * var(--one-px));
        border: none;
        color: var(--button-text-color);
        background-color: var(--main-color);
        border-radius: calc(12rem * var(--one-px));
        font-size: 1rem;
        transition: color var(--transition-time),
        background-color var(--transition-time);
      }

      @media (hover: hover) {
        button:hover {
          background-color: var(--secondary-color);
        }
      }

      button:active {
        color: var(--main-color);
        background-color:  var(--button-text-color);
        border-width: calc(2rem * var(--one-px));
        border-color: var(--main-color);
        border-style: solid;
      }

      .container {
        width: 100%;
      }

      .column {
        display: flex;
        justify-content: space-between;
        align-items: center;
        flex-direction: column;
        row-gap: calc(8rem * var(--one-px));
        column-gap: calc(8rem * var(--one-px));
      }

      .column &gt; button {
        width: 100%;
      }

      @media (min-width: 450px) {
        .container {
          width: calc(450rem * var(--one-px));
        }
      }
    &lt;/style&gt;
  &lt;/head&gt;
  &lt;body&gt;
    &lt;div class=&quot;capsule&quot;&gt;
      &lt;h1 id=&quot;title&quot;&gt;Super Glasses Control&lt;/h1&gt;
      &lt;div class=&quot;container&quot;&gt;
        &lt;div class=&quot;column&quot;&gt;
          &lt;button id=&quot;0&quot; class=&quot;&quot; name=&quot;Reset&quot; type=&quot;submit&quot;&gt;Reset&lt;/button&gt;
          &lt;button id=&quot;1&quot; class=&quot;&quot; name=&quot;Clean&quot; type=&quot;submit&quot;&gt;Clean&lt;/button&gt;
          &lt;button id=&quot;2&quot; class=&quot;&quot; name=&quot;Shock&quot; type=&quot;submit&quot;&gt;Shock&lt;/button&gt;
          &lt;button id=&quot;3&quot; class=&quot;&quot; name=&quot;RaisedEyebrow&quot; type=&quot;submit&quot;&gt;Raised Eyebrow&lt;/button&gt;
          &lt;button id=&quot;4&quot; class=&quot;&quot; name=&quot;Anger&quot; type=&quot;submit&quot;&gt;Anger&lt;/button&gt;
          &lt;button id=&quot;5&quot; class=&quot;&quot; name=&quot;Wiggle&quot; type=&quot;submit&quot;&gt;Wiggle&lt;/button&gt;
          &lt;button id=&quot;6&quot; class=&quot;&quot; name=&quot;Random&quot; type=&quot;submit&quot;&gt;Random&lt;/button&gt;
        &lt;/div&gt;
      &lt;/div&gt;
    &lt;/div&gt;
  &lt;/body&gt;
  &lt;script&gt;
    function sendInfo(button) {
      var title = document.querySelector(&quot;h1&quot;);
      title.innerHTML = button.name;
      var xhttp = new XMLHttpRequest();
      xhttp.open(&quot;GET&quot;, &quot;/update?state=&quot; + button.id, true);
      xhttp.send();
    }

    const buttons = document.querySelectorAll(&quot;button&quot;);

    for (let i = 0; i &lt; buttons.length; i++) {
      buttons[i].addEventListener(&quot;click&quot;, (event) =&gt; {
        sendInfo(buttons[i]);
      });
    }
  &lt;/script&gt;
&lt;/html&gt;)&quot;;

</code>
</pre>