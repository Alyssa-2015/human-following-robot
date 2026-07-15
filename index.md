# hand following robot
In this project ill be building a hand following robot that uses sensors to detect movement. ill be using 9V batteries as energy and avoidance modules with an ultrasonic moduel as the sensors. Later this project will be modofied to be controlled by a remote allowing it to turn on and off and do other functions like drive without its sensors.

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

```c++
const int A_1B = 5;
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
# Code(modification) 

```c++
#include <IRremote.h>

const int IR_RECEIVE_PIN = 12;  // Define the pin number for the IR Sensor

const int A_1B = 5;
const int A_1A = 6;
const int B_1B = 9;
const int B_1A = 10;

const int echoPin = 4;
const int trigPin = 3;

const int rightIR = 7;
const int leftIR = 8;

const int lineTrackPin = 2;

int speed = 150;
String flag = "NONE";

void setup() {
  Serial.begin(9600);

  //motor
  pinMode(A_1B, OUTPUT);
  pinMode(A_1A, OUTPUT);
  pinMode(B_1B, OUTPUT);
  pinMode(B_1A, OUTPUT);

  //ultrasonic
  pinMode(echoPin, INPUT);
  pinMode(trigPin, OUTPUT);

  //IR obstacle
  pinMode(leftIR, INPUT);
  pinMode(rightIR, INPUT);

  //Line Track Module
  pinMode(lineTrackPin, INPUT);

  //IR remote
  IrReceiver.begin(IR_RECEIVE_PIN, ENABLE_LED_FEEDBACK);  // Start the IR receiver // Start the receiver
  Serial.println("REMOTE CONTROL START");

}

void loop() {

  if (IrReceiver.decode()) {
    //    Serial.println(results.value,HEX);
    String key = decodeKeyValue(IrReceiver.decodedIRData.command);
    if (key != "ERROR") {
      Serial.println(key);

      if (key == "+") {
        speed += 50;
        Serial.println(speed);
      } else if (key == "-") {
        speed -= 50;
        Serial.println(speed);
      } else if (key == "2") {
        moveForward(speed);
        delay(1000);
      } else if (key == "1") {
        moveLeft(speed);
      } else if (key == "3") {
        moveRight(speed);
      } else if (key == "4") {
        turnLeft(speed);
      } else if (key == "6") {
        turnRight(speed);
      } else if (key == "7") {
        backLeft(speed);
      } else if (key == "9") {
        backRight(speed);
      } else if (key == "8") {
        moveBackward(speed);
        delay(1000);
      } else if (key == "CYCLE") {
        flag = "LINE";
      } else if (key == "U/SD") {
        flag = "AUTO";
      } else if (key == "0") {
        flag = "NONE";
        stopMove();
      } else if (key == "FORWARD") {
        flag = "ULTR";
      } else if (key == "BACKWARD") {
        flag = "IROB";
      } else if (key == "EQ") {
        flag = "FOLW";
      }

      if (speed >= 255) {
        speed = 255;
      }
      if (speed <= 0) {
        speed = 0;
      }
      delay(500);
      stopMove();
    }

    IrReceiver.resume();  // Enable receiving of the next value
  }
  if (flag == "AUTO") {
    AutoDrive(speed);
  } else if (flag == "LINE") {
    lineTrack(speed);
  } else if (flag == "ULTR") {
    ultrasonicExample(speed);
  } else if (flag == "IROB") {
    irobstacleExample(speed);
  } else if (flag == "FOLW") {
    following(speed);
  }
}
```
# Bill of Materials
These are the parts I used for my project with the price attched along with some notes

| **Part** | **price** | **link** | **note** |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|   Arduino Board| $20-60 | <a href="https://www.sunfounder.com/collections/official-arduino-boards"> |this is how you can connect your car to code |
| L9110 Motor Driver Module | $5.99 | <a href="https://www.amazon.com/Ferwooh-Stepper-Controller-2-5-12V-H-Bridge/dp/B0D17PJ2MS/ref=asc_df_B0D17PJ2MS?tag=bingshoppinga-20&linkCode=df0&hvadid=80676876080030&hvnetw=o&hvqmt=e&hvbmt=be&hvdev=c&hvlocint=&hvlocphy=98109&hvtargid=pla-4584276356443276&psc=1&hvocijid=999858013441725223-B0D17PJ2MS-&hvexpln=0"> Link </a> |This connects the motors to the arduino and bread board|
| TT Motor | $9.69 | <a href="https://www.amazon.com/Motor-Leads-Gearbox-Shaft-200RPM/dp/B0D8H89XDY/ref=asc_df_B0D8H89XDY?tag=bingshoppinga-20&linkCode=df0&hvadid=80814314504878&hvnetw=o&hvqmt=e&hvbmt=be&hvdev=c&hvlocint=&hvlocphy=98109&hvtargid=pla-4584413796081357&psc=1&hvocijid=1991183372713299680-B0D8H89XDY-&hvexpln=0"> Link </a> |The motors are what make the wheels of the car spin|
|  Ultrasonic Module | $14.99 | <a href="https://www.sunfounder.com/products/5pcs-hc-sr04-ultrasonic-module-distance-sensor"> Link </a> |This is how the robot will sense your hand
| Obstacle Avoidance Module | $8.99 | <a href="https://www.sunfounder.com/products/obstacle-avoidance-sensor"> Link </a>|This will be used to detect your hand and move accordingly 

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
