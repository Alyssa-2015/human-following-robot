# hand following robot
In this project ill be building a hand following robot that uses sensors to detect movement. ill be using 9V batteries as energy and avoidance modules with an ultrasonic moduel as the sensors. Later this project will be modofied to be controlled by a remote allowing it to turn on and off and do other functions like drive without its sensors.


```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| **Engeneer** | **School** | **Desired Path** | **Grade** |
|:--:|:--:|:--:|:--:|
| Alyssa T | Kipp College Prep High School | Electrical Engineering | Incoming junior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone


<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/t9IjdnJJNp8?si=hCaNVnorZQxnBWdJ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my second Milestone ive connected the L9110 , Arduino UNO obstacle, avoidance modules , and ultrasonic module to each other. I had a hard time figuring out where each wire should go on the bread board since this is my first engineering project. When I mistakenly placed wires and imported code my robot wouldnt work at all. I restarted a few times but eventually got my robot to move and follow my hand. I also found out that one 9V battery wasnt enough and that id need to get a second one to power my robot without a wire.
# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/DxZx1jrZPNk?si=xxixbLDLeqH3rpiW" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

for my first Milestone I finished the base of my robot car. On the front ive placed my bread board , Arduino UNO , obstacle avoidance module and L9110 module. On the back of my car ive placed 2 motors for the side wheels , 1 universal wheel , and a 9V battery with a conveter attached to connect to my Arduino board. From here I can start focusing on wiring for specific things and for this project im wiring to get the car to move by sensing motion.

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code

```const int A_1B = 5;
const int A_1A = 6;
const int B_1B = 9;
const int B_1A = 10;

const int rightIR=7;
const int leftIR=8;

const int trigPin = 3;
const int echoPin = 4;

void setup() {
  

  //motor
  pinMode(A_1B, OUTPUT);
  pinMode(A_1A, OUTPUT);
  pinMode(B_1B, OUTPUT);
  pinMode(B_1A, OUTPUT);

  //IR obstacle
  pinMode(leftIR,INPUT);
  pinMode(rightIR,INPUT);
  
  //ultrasonic
  pinMode(echoPin, INPUT);
  pinMode(trigPin, OUTPUT);
}

void loop() {

  float distance = readSensorData();

  int left = digitalRead(leftIR);  // 0: Obstructed   1: Empty
  int right = digitalRead(rightIR);
  int speed = 150;

  if (distance>5 && distance<10){
    moveForward(speed);
  }else if(!left&&right){
    turnLeft(speed);
  }else if(left&&!right){
    turnRight(speed);
  }else{
    stopMove();
  }
}

float readSensorData() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  float distance = pulseIn(echoPin, HIGH) / 58.00; //Equivalent to (340m/s*1us)/2
  return distance;
}

void moveForward(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, speed);
  analogWrite(B_1B, speed);
  analogWrite(B_1A, 0);
}

void moveBackward(int speed) {
  analogWrite(A_1B, speed);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, speed);
}

void turnRight(int speed) {
  analogWrite(A_1B, speed);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, speed);
  analogWrite(B_1A, 0);
}

void turnLeft(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, speed);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, speed);
}

void stopMove() {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, 0);
}

```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **price** | **link** | **note** |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|  sunfounder R3 Board|  how to connect wiring to code | $20-60 | <a href="[Obstacle Avoidance Module¶](https://www.sunfounder.com/collections/official-arduino-boards)"> Link </a> |
| L9110 Motor Driver Module | $5.99 | <a href="https://www.amazon.com/Ferwooh-Stepper-Controller-2-5-12V-H-Bridge/dp/B0D17PJ2MS/ref=asc_df_B0D17PJ2MS?tag=bingshoppinga-20&linkCode=df0&hvadid=80676876080030&hvnetw=o&hvqmt=e&hvbmt=be&hvdev=c&hvlocint=&hvlocphy=98109&hvtargid=pla-4584276356443276&psc=1&hvocijid=999858013441725223-B0D17PJ2MS-&hvexpln=0"> Link </a> |the motors will make the wheels turn which allows the car to drive|
| TT Motor | $9.69 | <a href="https://www.amazon.com/Motor-Leads-Gearbox-Shaft-200RPM/dp/B0D8H89XDY/ref=asc_df_B0D8H89XDY?tag=bingshoppinga-20&linkCode=df0&hvadid=80814314504878&hvnetw=o&hvqmt=e&hvbmt=be&hvdev=c&hvlocint=&hvlocphy=98109&hvtargid=pla-4584413796081357&psc=1&hvocijid=1991183372713299680-B0D8H89XDY-&hvexpln=0"> Link </a> |
|  Ultrasonic Module | $14.99 | <a href="https://www.sunfounder.com/products/5pcs-hc-sr04-ultrasonic-module-distance-sensor"> Link </a> |
| Obstacle Avoidance Module | $8.99 | <a href="https://www.sunfounder.com/products/obstacle-avoidance-sensor"> Link </a> 

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
