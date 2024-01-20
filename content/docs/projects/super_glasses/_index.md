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

<p>(References to related concepts, projects, books, websites, stories, systems, fruits, etc. and their relation to the project at hand.)</p>


<figure>
    <img src="./assets/1relatedWork.png" alt="Similar project"/>
  <figcaption><em>A picture of a similar project</em></figcaption>
</figure>
The picture above shows a similar project which uses plastic pieces as windscreen wipers to move rain drops to the side of the glasses. 
For the movement it uses a small motor which is attached to the side of the glasses and connected to the plastic wipers via wires.
<a href="https://www.youtube.com/shorts/yv6GhCoSSO8" target="_blank">Source</a>

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

This worked!

### Time to solder

### Building a case

### 3D print the wippers

## Conclusion

A reflection on your prototyping process and the project outcome. What happens to the prototype after the project?