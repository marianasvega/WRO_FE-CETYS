# CETYS FUTURE ENGINEERS 2024 🤖

This is the official repository for the CETYS Team participating in the Future Engineers category at the 2024 Mexican National. Here we will upload all of the processes and results regarding our autonomous vehicle, as well as a full report on the code used and its electrical components.

**Team Members:** Mariana Sofia Vega Contreras, Claudio Iván López Valle & Mariana Flores Martínez.

![CETYS (1920 x 500 px)](https://github.com/user-attachments/assets/a858a081-411f-4ef9-bae7-2771a890f4c4)


# Content
* `Models` Files for models used by 3D printers to produce the vehicle elements.
* `Others` Files which can be used to understand how to prepare the vehicle for the competition. 
* `Schemes` Diagrams of the electromechanical components used in the vehicle and how they connect.
* `Src` Contains code of control software for all components which were programmed to participate in the competition.
* `Team Photos` Contains 2 photos of the team.
* `Vehicle Photos` Contains 6 photos of the vehicle.
* `Video` Contains the video.md file with the link to a video where a driving demonstration exists.

# Introduction 👷‍♀👨‍💻👩‍🔧
Arturo, our autonomous vehicle, can perform thanks to a variety of different mechanisms that when assembled correctly, can help a robot accomplish a challenge such as the Future Engineers Category in the WRO Competition. To make clear how our robot operates, we will continue to explain how each of these mechanisms works, including which specific electronic components were used and the logic behind its code.

![ARTURO](https://github.com/user-attachments/assets/d106c4f6-eba3-49f1-b9f9-482aa9c6ef04)


# Electronic components

|              Component              |    Quantity   |
| ----------------------------------- |     :---:     |
| **Arduino Uno R3**                  |       1       |
| **Bread Board Mini**                |       1       |
| **Servo HS-322HD**                  |       1       |
| **Ultrasonic Sensor HC-SR04**       |       3       |      
| **Faulhaber-type DC Motor**         |       1       |
| **H-bridge Motor Driver L298N**     |       1       |
| **Pushbutton**                      |       1       |
| **9V Battery**                      |       1       |
| **7.4V Battery**                    |       1       |
| **330Ω Resistor**                   |       1       |
| **Capacitor**                       |       1       |
| **Pixy 2 Camera**                   |       1       |
| **Switch Button Interruptor KCD1**  |       2       |

# Mechanism explanation


### Robot´s Vision (Pixy 2 Camera) 📷

In order for our vehicle to detect the obstacles on the rink (red and green traffic lights and the parking walls), we decided to use the Pixy2 Camera. Pixy2 can be programmed to detect specific signatures by recognizing it´s shape and/or color. After you complete the settings on the camera´s app, you are all set to start programming what you want your vehicle to do when detecting each signature. For example, we used the following code (the syntaxis of the commands correspond to the Pixy2 Arduino library):

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

> [!NOTE]
> In this code, when the camera detects our signature number 1 (red) it activates our module "followBlock_red" which makes the robot swerve right, according to the manual rules. When it detects signature number 2 (green), it swerves left.
  
### Front Tire Axle (HS-322HD Servo) 🛞
To give the vehicle more direction at every turn, we installed a free steer system in the front tire axle controlled by an HS-322HD Servo. The servo´s blades connect to an axle completely designed by us that leaves a space in between so that the tires can run freely. With the speed the motor provides in the rear axle, the front tires don´t need any extra power to reel forward. 

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

> [!NOTE]
>The "distance" variable represents the distance from the sensor to the wall. It’s computed by multiplying the duration of the echo by the speed of sound (0.034 cm/µs) and dividing by 2 to account for the round trip of the sound wave. This distance helps in determining when to navigate around obstacles.

### Rear Tire Axle (Faulhaber MOT-165 and LEGO Diferential) ⚙️

### How it all comes together (explaining the code) 💻

# Acknowledgments

We would like to take a space to thank our sponsors, without whom this project could not have come to be. Your help not only made a significant contribution to the progress of this autonomous vehicle but aided in the innovation of technology and design that may very well help other future engineers. We are very grateful for your kindness.

Sincerely, Mariana Flores, Claudio López, and Mariana Vega.

![CETYS (1920 x 500 px) (2)](https://github.com/user-attachments/assets/27a9672c-2f5b-43ed-83a7-8f4b370a78d4)


