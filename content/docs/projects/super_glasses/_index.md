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

A detailed description of the concept and sketches of the planned implementation.

If a section grows too large or handles a very specific part of the project it can be put into [subpages]({{< ref "subpage_1#how-to-format" >}}).

## Related work

For the Project Super Glasses a microcontroller is needed for the logic. At the beginning the Arduino Uno was used. Later it was replaced by an ESP32 to trigger the actions like clean via a website. These two Microcontrollers are both popular. Consequently there are a lot of projects presented on the internet on several websites and blogs which make use of one of these Microcontrollers. One reference about how to build an asynchronus web server with the ESP32 is published on the website [Random Nerd Tutorials](https://randomnerdtutorials.com/esp32-async-web-server-espasyncwebserver-library/). The tutorial shows how to control three LEDs which are placed on a breadboard and connected to the ESP32. 

Moreover the two microcontrollers are also present in the literature. For example the Rheinwerk publishing house published the books [Arduino](https://www.rheinwerk-verlag.de/arduino-das-umfassende-handbuch/) and [ESP32](https://www.rheinwerk-verlag.de/mikrocontroller-esp32-das-umfassende-handbuch/) which give a good introduction to the world of Microcontrollers.

After the decision to build windscreen wipers for glasses, the research on the internet showed that other people also had the same idea. In the following some related projects are presented which are published on the video platform YouTube.

{{< figure src="./assets/1relatedWork.png" caption="*picture of a similar project*">}}
The picture above shows a project which is related to the idea to wash away raindrops. The construction is slightly different. It uses just one motor that can rotate 360°. Similarly to the presented project it is attached to the side of the glasses. For the wipers it uses plastic pieces which are each attached to the top of a spectacle lens. They are connected to the motor via wires. [Source](https://youtube.com/shorts/yv6GhCoSSO8?si=K6DPu0hzVph28PmN) 

{{< figure src="./assets/2relatedWork.png" caption="*picture of a similar project*">}}
The next related project Wiper Glassez from Benjamin King shown in the picture above also tries to solve the problem of fogged glasses. The creator also refers to it as a Chindogu. Additionally the construction of the glasses gets really close to the presented one. It uses two servo motors which are attached to the temples of the glasses next to the spectacle lens. The windscreen wipers are directly on top of the servo motors. So that they follow the movement of the motors. For the logic it uses an Arduino which is attached with the rest of the equipment like the breadboard to a cap. As an extension, it uses a moisture sensor to trigger the movement.
[Source](https://www.youtube.com/watch?v=jDX6aNAMXfQ) 



## Implementation 

A detailed description of your prototyping process.

### Iteration №1

This did not work.

### Iteration №2

This did also not work.

### Iteration №3

This worked!

## Conclusion

A reflection on your prototyping process and the project outcome. What happens to the prototype after the project?