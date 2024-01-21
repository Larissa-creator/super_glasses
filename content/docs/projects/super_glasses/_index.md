---
title: Super glasses
type: docs
---

# Super glasses

## Abstract

The view through glasses can easily get blocked due to rain drops or fogging and 
for cleaning it must be taken off. Since this action can be a hassle, 
Super glasses can clean themselves by clicking on a button. 
Additionally, they can display emotions, as glasses tend to cover the eyebrows and 
big sunglasses even cover the eyes

## Introduction

The idea is to have wippers be attached to the glasses 
so that the glasses will clean themselves.
A button click starts the servo motors 
which perform the cleaning motion with the wippers on it.

<figure id="sketch">
    <img src="./assets/brille_sketch.jpg" alt="A sketch of the idea" style="max-height: 200px"/>
  <figcaption><em>A sketch of the idea</em></figcaption>
</figure>

## Related work

For the Project Super Glasses a microcontroller is needed for the logic. 
At the beginning the Arduino Uno was used. 
Later it was replaced by an ESP32 to trigger the actions like clean via a website. 
These two Microcontrollers are both popular. 
Consequently, there are a lot of projects presented on the internet on several websites and blogs 
which make use of one of these Microcontrollers. 
One reference about how to build an asynchronus web server with the ESP32 is published on the website [Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-async-web-server-espasyncwebserver-library/). 
The tutorial shows how to control three LEDs which are placed on a breadboard and connected to the ESP32. 

Moreover, the two microcontrollers are also present in the literature. 
For example the Rheinwerk publishing house published the books [Arduino](https://www.rheinwerk-verlag.de/arduino-das-umfassende-handbuch/) and [ESP32](https://www.rheinwerk-verlag.de/mikrocontroller-esp32-das-umfassende-handbuch/) 
which give a good introduction to the world of Microcontrollers.

After the decision to build windscreen wipers for glasses, 
the research on the internet showed that other people also had the same idea. 
In the following some related projects are presented which are published on the video platform YouTube.

<figure id="sketch">
    <img src="./assets/1relatedWork.png" alt="Project from Deffinite CoRen" style="max-height: 200px"/>
  <figcaption><em>Project from Deffinite CoRen</em></figcaption>
</figure>

The picture above shows a project which is related to the idea to wash away raindrops. 
The construction is slightly different. It uses just one motor that can rotate 360°. 
Similarly to the presented project it is attached to the side of the glasses. 
For the wipers it uses plastic pieces which are each attached to the top of a spectacle lens. 
They are connected to the motor via wires. [Source](https://youtube.com/shorts/yv6GhCoSSO8?si=K6DPu0hzVph28PmN) 

<figure id="sketch">
    <img src="./assets/2relatedWork.png" alt="Project from Benjamin King" style="max-height: 200px"/>
  <figcaption><em>Project from Benjamin King</em></figcaption>
</figure>

The next related project Wiper Glassez from Benjamin King shown in the picture above also tries to solve the problem of fogged glasses. 
The creator also refers to it as a Chindogu. 
Additionally, the construction of the glasses gets really close to the presented one. 
It uses two servo motors which are attached to the temples of the glasses next to the spectacle lens. 
The windscreen wipers are directly on top of the servo motors. 
So that they follow the movement of the motors. 
For the logic it uses an Arduino which is attached with the rest of the equipment like the breadboard to a cap. 
As an extension, it uses a moisture sensor to trigger the movement.
[Source](https://www.youtube.com/watch?v=jDX6aNAMXfQ) 



## Implementation 

A detailed description of your prototyping process.

### Paper Prototyping Session

This did not work.

### First version with the Arduino

Our first try still had a real push button and was made with an Adruino. 
The wippers consisted of popsicle sticks and sponges as 
the width of the stick prevented the sponge from rotating. 
<figure>
    <img src="./assets/V01_front.jpg" alt="First Version from the front" style="max-height: 500px"/>
  <figcaption><em>First Version from the front</em></figcaption>
</figure>
Using our knowledge from the lessons we built our circuit 
which can be seen on <a href="#v01circuit">the picture <em>First version's circuit</em></a>. 
While the circuit looks simple, <a href="#v01real">the photo <em>The cabeling in real life</em></a> shows that the implementation looks like a mess
as there are many wires in a narrow space. 
To make the cabling reproducible, there is <a href="#v01img">the image <em>The cabeling as a diagram</em></a>. 
<figure id="v01circuit">
    <img src="./assets/V01_Kreis.jpg" alt="First version's circuit" style="max-height: 300px"/>
  <figcaption><em>First version's circuit</em></figcaption>
</figure>
<figure id="v01real">
    <img src="./assets/V01_top.jpg" alt="The cabling in real life" style="max-height: 300px"/>
  <figcaption><em>The cabling in real life</em></figcaption>
</figure>
<figure id="v01img">
    <img src="./assets/V01_Kabeln.jpg" alt="The cabling as a diagram" style="max-height: 300px"/>
  <figcaption><em>The cabling as a diagram</em></figcaption>
</figure>

The complete code of the first version can be found on the subpage [First version's code]({{< ref "v01_code" >}}). 
The code works the following way: The arduino continuously listens whether the button has been pressed or not. 
If it has been, the pin position will be attached to the servo and then the servos move the wippers once down and up.
Afterward the pin position will be detached.

### Second version with the ESP32

After knowing the ESP32 we decided to use it instead of the Arduino 
because the wearer would not need to search for the push button on their head anymore
as it would exist on the smartphone. Since there is enough space on the website 
we also decided to implement additional buttons which show emotions with the wippers. 
The end version has in total seven buttons: 
<ul>
    <li>Reset</li>
    <li>Clean</li>
    <li>Shock</li>
    <li>Raised Eyebrow</li>
    <li>Anger</li>
    <li>Wiggle</li>
    <li>Random</li>
</ul>
The website is styled with CSS so that the buttons are bigger and 
that the states <code>hover</code> and <code>active</code> 
are better visible. <a href="#website_states">The picture <em>The website with different button states</em></a> 
shows on the left screen the buttons in the default state, on the middle one is <em>Clean</em> in the <code>hover</code> state 
and on the right one is <em>Clean</em> in the <code>active</code> state.

<figure id="website_states">
    <img src="./assets/website_states.jpg" alt="The website with different button states" style="max-height: 300px"/>
  <figcaption><em>The website with different button states</em></figcaption>
</figure>

The complete code of the final version can be found on the subpage [Final version's code]({{< ref "vend_code" >}}). 
The code works the following way: When the ESP is connected to the WLAN with the given name and password, 
the built-in LED will be on as long the microcontroller is connected to the WLAN.
Additionally, the ESP32 will print to the Serial Monitor the address where the website will be shown.
Every button on the website has an ID and an <code>EventListener</code> 
which sends the ID to the ESP when the button is pressed. 
As the ID tells which button has been pressed, 
the corresponding action can then be performed.
Afterward, the ESP32 updates the website with the <code>h1</code> 
being the last pressed button. 
The title change exists for debugging so that we know 
if the code was iterated in case the action did not happen.

### Time to solder

### Building a case

### 3D print the wippers

## Conclusion

A reflection on your prototyping process and the project outcome. What happens to the prototype after the project?