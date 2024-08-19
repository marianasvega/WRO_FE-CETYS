# CETYS FUTURE ENGINEERS 2024 🤖
This is the official repository for the CETYS Team participating in the Future Engineers category at the 2024 Mexican National. Here we will upload all of the processes and results regarding our autonomous vehicle, as well as a full report on the code used and its electrical components.

FOTO

# Content
* `Models` Files for models used by 3D printers to produce the vehicle elements.
* `Others` Files which can be used to understand how to prepare the vehicle for the competition. 
* `Schemes` Diagrams of the electromechanical components used in the vehicle and how they connect.
* `Src` Contains code of control software for all components which were programmed to participate in the competition.
* `Team Photos` Contains 2 photos of the team.
* `Vehicle Photos` Contains 6 photos of the vehicle.
* `Video` Contains the video.md file with the link to a video where a driving demonstration exists.

# Introduction
Arturo, our autonomous vehicle, can perform remarkably thanks to a variety of different mechanisms that when assembled correctly, can help a robot accomplish a challenge such as the Future Engineers Category in the WRO Competition. To make clear how our robot operates, we will continue to explain how each of these mechanisms works, including which specific electronic components were used and the logic behind its code.

# Building process

### Electronic components
|        Name        |    Quantity    |        Component        |
| ------------------ | -------------- | ----------------------- |
| U1                 |       1        | Arduino Uno R3          |
| Content Cell   | Content Cell   | -------------  |
| Content Cell   | Content Cell   | -------------  |

# Mechanism explanation

### Robot´s Vision (Pixy 2 Camera) 📷
  For our vehicle to detect the obstacles on the rink (red and green traffic lights and the parking walls), we decided to use the Pixy2 Camera. Pixy2 can be programmed to detect specific signatures by recognizing it´s shape and/or color. After you complete the settings on the camera´s app, you are all set to start programming what you want your vehicle to do when detecting each signature. For example, we used the following code (the syntaxis of the commands correspond to the Pixy2 Arduino library):

  ```ruby
pixy.ccc.getBlocks();

if (pixy.ccc.blocks[i].m_signature == 1) {            // 1 EQUALS RED BLOCK IN OUR CAMERA CONFIGURATION //
  Serial.println("Red object detected");
  followBlock_red(i, "Red");
}

else if (pixy.ccc.blocks[i].m_signature == 2) {       // 2 EQUALS GREEN BLOCK IN OUT CAMERA CONFIGURATION //
  Serial.println("Green object detected");
  followBlock_green(i, "Green");
}  
  ```
In this code, when the camera detects our signature number 1 (red) it activates our module "followBlock_red" which makes the robot swerve right, according to the manual rules. When it detects signature number 2 (green), it swerves left.
  
### Front Tire Axle (HS-322HD Servo) 🛞
With the purpose of giving the vehicle more direction at every turn, we installed a free steer system in the front tire axle controlled by an HS-322HD Servo. The servo´s blades connect to an axle completely designed by us that leaves a space in between so that the tires can run freely. With the speed the motor provides in the rear axle, the front tires don´t need any extra power to reel forward. 

To use the servo in the code, it is necessary to include the "Servo.h" library. After that, all that it takes to change the servo´s angle is the following command. 

```ruby
servo.write(45);
```

### Ultrasonic Sensors (HC-SR04) 📏
The main challenge in this category is to be able to turn corners and, consequently, avoid hitting walls. For this we used Ultrasonic Sensors; these are devices that measure the distance between itself and an object by emitting ultrasonic sound waves, which then convert the reflected sound into an electrical signal. To obtain said electrical signal and interpret it in centimeters we use the following code:

```ruby
long DisCalc(int TP, int EP) {

  digitalWrite(TP, LOW);                    // CLEAR THE TRIG PIN //
  delayMicroseconds(2); 

  digitalWrite(TP, HIGH);                   // SET THE TRIG PIN HIGH FOR 10 MICROSECONDS //
  delayMicroseconds(10);
  digitalWrite(TP, LOW);

  long duration = pulseIn(EP, HIGH);        // READ THE ECHO PIN AND CALCULATE THE DIRECTION //

  long distance = duration * 0.034 / 2;     // CALCULATE THE DISTANCE IN CENTIMETERS //

  return distance;
}
```
The variable "distance"

### Rear Tire Axle (Faulhaber MOT-165 and LEGO Diferential) ⚙️
### How it all comes together (explaining the code) 💻
