** **

# CETYS FUTURE ENGINEERS 2024 🤖

This is the official repository for the CETYS Team that participated in the Future Engineers category at the 2024 Mexican National and is soon to compete in the International Final in Izmir, Türkiye!. Here we will upload all of the process, improvements, and results regarding our autonomous vehicle, as well as a full report on the code used and its electrical components.
  
**Team Members:** Mariana Sofia Vega Contreras, Claudio Iván López Valle & Mariana Flores Martínez.

![CETYS (1920 x 500 px)](https://github.com/user-attachments/assets/a858a081-411f-4ef9-bae7-2771a890f4c4)

<br>

** **


# Content
* `Models` Files for models used by 3D printers to produce the vehicle elements.
* `Others` Links of videos to understand how to prepare the vehicle for the competition. 
* `Schemes` Diagrams of the electromechanical components used in the vehicle and how they connect.
* `Src` Contains source codes for all components which were programmed to participate in the competition.
* `Team Photos` Contains 2 photos of the team.
* `Vehicle Photos` Contains 6 photos of the vehicle.
* `Video` Contains the link to a video where a driving demonstration exists.
<br>

** **

# Introduction 👷‍♀👨‍💻👩‍🔧
<br>

<div style="display: flex; align-items: flex-start;">
  <img align="left" width="190" src="https://github.com/user-attachments/assets/5f3e63f8-bbd5-40c3-ac24-5c255352a6ce" alt="ARTURO" style="margin-right: 20px;"/>
  <p><be>This report outlines the design, development, and performance of Arturo, the autonomous vehicle developed by our team, the CETYS Future Engineers, for the 2024 World Robot Olympiad. In this document, we will provide a comprehensive overview of our previous work in the Mexican national competition, where we successfully showcased our prototype and the iterative improvements we made in preparation for the international championship in Türkiye.
</p>
</div>
<br>
<br>

** **


# Electronic components

|              Component              |    Quantity   |
| ----------------------------------- |     :---:     |
| **ESP-WROOM-32**                    |       1       |
| **Bread Board Mini**                |       1       |
| **Servo HS-322HD**                  |       1       |
| **Ultrasonic Sensor HC-SR04**       |       5       |      
| **Faulhaber-type DC Motor**         |       1       |
| **H-bridge Motor Driver L298N**     |       1       |
| **Pushbutton**                      |       1       |
| **9V Battery**                      |       1       |
| **7.4V Battery**                    |       1       |
| **330Ω Resistor**                   |       1       |
| **Capacitor**                       |       1       |
| **Pixy 2 Camera**                   |       1       |
| **Switch Button Interruptor KCD1**  |       2       |
| **Gyro + Accelerometer MPU-6050**   |       1       |

<br>

** **

# Mechanism explanation


### `Robot´s Vision (Pixy 2 Camera) 📷`

In order for our vehicle to detect the obstacles on the rink (red and green traffic lights and the parking walls), we decided to use the Pixy2 Camera, a versatile vision sensor designed for robotics and automation projects. It offers powerful image recognition capabilities and is user-friendly for integrating visual processing into various applications. After you complete the settings on the camera´s app, you are all set to start programming what you want your vehicle to do when detecting each signature. For example, we used the following code (the syntax of the commands correspond to the Pixy2 Arduino library):
  
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
<br>

> [!NOTE]
> In this code, when the camera detects our signature number 1 (red) it activates our module "followBlock_red" which makes the robot swerve right, according to the manual rules. When it detects signature number 2 (green), it swerves left.
<br>
<br>
  
### `Front Tire Axle (Ackerman Steering and HS-322HD Servo) 🛞`

During the Mexican National Competition, we observed that our vehicle needed a better steering system that could round corners with much more facility and precision. After much research, we learned about a mechanism named the Ackerman Steering Principle. When a vehicle turns, the inner wheel follows a tighter path than the outer wheel. To ensure smooth turning without excessive tire wear or slipping, the inner wheel needs to turn at a sharper angle than the outer wheel. This is precisely what the Ackermann geometry achieves.

<br>
![Screenshot 2024-11-06 183028](https://github.com/user-attachments/assets/8ef272bf-3240-444e-80de-13f5da3d773b)

Figure 9: Design and Development of Four-Wheel Steering for All Terrain Vehicle (Vishnu 2020)
<br>

When a driver turns the steering wheel, the motion is passed through the steering column to the steering gear. This gear changes the rotation into a pushing or pulling movement, which is sent to the tie rods. The tie rods then adjust the steering arms connected to the wheels. This setup ensures that the inside wheel turns at a sharper angle than the outside wheel, helping the vehicle turn more smoothly and efficiently while reducing tire wear and slip.
> [!NOTE]
>For the STL files to print the Ackerman system we used, visit the following link for it is NOT of our property and we give full credits to the owner: [https://github.com/KarenWon9/WRO-FI-Team-Spark](url)


<br>

On the other hand, the HS-322HD is a popular heavy-duty servo known for its durability and performance, and without it, our vehicle would have no direction. It features Hitec's Karbonite gear train, which is significantly stronger than standard nylon gears, making it suitable for various applications. With a good balance of speed and torque, the HS-322HD is often used in RC airplanes, helicopters, and other models. To use the servo in the code, it is necessary to include the "Servo.h" library. After that, all that it takes to change the servo´s angle is the following command. 

```ruby
servo.write(45);
```
<br>
<br>

### `Ultrasonic Sensors (HC-SR04) 📏`
The primary challenge in this category is navigating corners and avoiding collisions with walls. To address this, we employed the HSR04 ultrasonic sensor, which operates by emitting ultrasonic pulses and measuring the time it takes for the echoes to return after bouncing off an object. This time delay is then converted into a distance measurement, allowing the sensor to accurately detect objects and measure distances with precision. The HSR04 sensor typically has a range of 2 centimeters to 4 meters and is favored for its ease of integration with microcontrollers like Arduino due to its simplicity and cost-effectiveness. Its compact design and straightforward interface make it ideal for applications involving obstacle detection, proximity sensing, and distance measurement in various DIY and industrial projects. To process and interpret this signal given in centimeters, we use the following code:

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
<br>

> [!NOTE]
>The "distance" variable represents the distance from the sensor to the wall. It’s computed by multiplying the duration of the echo by the speed of sound (0.034 cm/µs) and dividing by 2 to account for the round trip of the sound wave. This distance helps in determining when to navigate around obstacles.
<br>
<br>

### `Rear Tire Axle (Faulhaber MOT-165 and LEGO Differential) ⚙️`
The most crucial mechanism of any vehicle is its drive system, and we aimed to replicate this as closely as possible. After some trial and error during the building process, we decided to incorporate a LEGO differential kit to simulate real-world driving dynamics (you can find the kit [here](https://www.amazon.com/dp/B0C4LFN1HD/ref=sspa_dk_detail_0?psc=1&pd_rd_i=B0C4LFN1HD&pd_rd_w=IBBZq&content-id=amzn1.sym.386c274b-4bfe-4421-9052-a1a56db557ab&pf_rd_p=386c274b-4bfe-4421-9052-a1a56db557ab&pf_rd_r=YCBYSZ8SNM5K3903VGBB&pd_rd_wg=8Vf9g&pd_rd_r=37648874-6ee4-4abb-b28e-2df1ea089964&s=toys-and-games&sp_csd=d2lkZ2V0TmFtZT1zcF9kZXRhaWxfdGhlbWF0aWM)). In the video provided, you can see our custom differential in action, where we designed and attached additional LEGO components. For a step-by-step guide on building your own, please refer to the "Others" folder in this repository.

<br>

<div align="center">
  <video width="340" src="https://github.com/user-attachments/assets/d7e541df-3d19-4036-9754-b866e7325a33
" controls width="600">
  </video>
</div>

<br>

> [!NOTE]
>If you want to learn how a differential works you can watch [this](https://www.youtube.com/watch?v=K4JhruinbWc) video, basically a differential gear allows wheels on the same axle to rotate at different speeds. It works by using a set of gears to distribute torque between the wheels, enabling them to turn at different rates, which is essential for smooth cornering. If 𝑇 is the total torque supplied by the engine to the differential, and it is divided between the two wheels, then the torque 𝑇<sub>1</sub> on one wheel and 𝑇<sub>2</sub> on the other wheel satisfy 𝑇 = 𝑇<sub>1</sub> + 𝑇<sub>2</sub>.
<br>
The motor used in this mechanism is a Faulhaber MOT-165. We opted for this motor because it usually operates at voltages ranging from 6V to 12V, the speed of the motor varies with voltage and load conditions, but it is designed to achieve high RPM (revolutions per minute), and it provides a modest amount of torque, suitable for small, precise applications. It is important to mention that the motor and its driver, the L298N dual H-Bridge, have their power supply, **a Li-Po battery of 1000 mAh and 7.4V**.
<br>

<br>


** **


## How it all comes together (explaining the code) 💻
<br>

We will now discuss how all of the previously mentioned mechanisms assemble together in our final code (Open and Obstacle Challenge). Before we start, we will show all of the functions used along the code to use as reference for when they appear next.

```ruby
void printDistances(long distanceLeft, long distanceMid, long distanceRight) {
  Serial.print("Dis Left: ");
  Serial.print(distanceLeft);
  Serial.print(" ");
  Serial.print("Dis Mid: ");
  Serial.print(distanceMid);
  Serial.print(" ");
  Serial.print("Dis Right: ");
  Serial.println(distanceRight);
}


int DisCalc(int TP, int EP){                                    // CALCULATE DISTANCES //
  long duration = -1;
  int reading = 0;
  int cont = 0;
  digitalWrite(TP, LOW);

  while ((duration!=0) || (cont <5)){
    delayMicroseconds(2);
    digitalWrite(TP, HIGH);
    delayMicroseconds(10);
    digitalWrite(TP, LOW);
    long duration = pulseIn(EP, HIGH,10000);

    if (duration>0){
      reading = duration * 0.034/2;
    }
    else{
      reading = 120;
    }
 
    return reading;
  }
 
} 

long DisCalchLong(int TP, int EP) {                            // CALCULATE DISTANCES BY GETTING AN AVERAGE //
  const int numReadings = 30;
  long readings[numReadings];  
  long sum = 0;

  for (int i = 0; i < numReadings; i++) {
    digitalWrite(TP, LOW);
    delayMicroseconds(2);
    digitalWrite(TP, HIGH);
    delayMicroseconds(10);
    digitalWrite(TP, LOW);

    long duration = pulseIn(EP, HIGH);
    readings[i] = duration * 0.034 / 2;
    sum += readings[i]; 
    delay(5); 
  }

  long minValue = readings[0];
  long maxValue = readings[0];
  for (int i = 1; i < numReadings; i++) {
    if (readings[i] < minValue) {
      minValue = readings[i];
    }
    if (readings[i] > maxValue) {
      maxValue = readings[i];
    }
  }

  sum -= minValue;
  sum -= maxValue;
  long average = sum / (numReadings - 2);
  return average;

}

void followBlock_green(int blockIndex, const char* color){    // GREEN OBSTACLE DETECTED //
 int x = pixy.ccc.blocks[blockIndex].m_x;

  Serial.print("Following ");
  Serial.print(color);
  Serial.print(" block at (");
  Serial.print(x);
  Serial.println(")");

  if(x < 158){
    LEFT();
  }
  else if(x > 158){
    SLIGHTLY_LEFT_MIN();
    delay(100);
    STRAIGHT();
  }
  delay(10);
}

void followBlock_red(int blockIndex, const char* color){    // RED OBSTACLE DETECTED //
  int x = pixy.ccc.blocks[blockIndex].m_x;

  Serial.print("Following ");
  Serial.print(color);
  Serial.print(" block at (");
  Serial.print(x);
  Serial.println(")");

  if(x > 158){
    RIGHT(); 
  }
 else if(x < 158){
    SLIGHTLY_RIGHT_MIN();
    delay(100);
    STRAIGHT();
  }
  delay(10);
} 


void STRAIGHT(){                                              // VEHICLE MOVES STRAIGHT FORWARD //
  servo.write(90);
  digitalWrite(IN1,LOW);   
  digitalWrite(IN2,HIGH);
  analogWrite(ENA,250);                                       // SET SPEED FOR THE MOTOR //
  delay(10);
} 

void STRAIGHT_SLOW(){                                         // VEHICLE MOVES STRAIGHT BUT SLOWER //
  servo.write(90);
  digitalWrite(IN1,LOW);   
  digitalWrite(IN2,HIGH);
  analogWrite(ENA,60);                                        // SET SPEED FOR THE MOTOR //
  delay(10);
} 

void RIGHT(){                                                 // VEHICLE SWERVES RIGHT //
  servo.write(60);
  digitalWrite(IN1,LOW);   
  digitalWrite(IN2,HIGH);   
  analogWrite(ENA,110);                                       // SET SPEED FOR THE MOTOR //
  delay(10);
} 

void LEFT(){                                                  // VEHICLE SWERVES LEFT //
  servo.write(110);
  digitalWrite(IN1,LOW);   
  digitalWrite(IN2,HIGH);   
  analogWrite(ENA,110);                                       // SET SPEED FOR THE MOTOR //
  delay(10);
}

void SLIGHTLY_LEFT(){                                         // VEHICLE SWERVES SLIGHTLY LESS TO THE LEFT //
  servo.write(105);
  digitalWrite(IN1,LOW);   
  digitalWrite(IN2,HIGH);   
  analogWrite(ENA,120);                                       // SET SPEED FOR THE MOTOR //
  delay(10);
}

void SLIGHTLY_LEFT_MID(){                                     // VEHICLE SWERVES SLIGHTLY LESS TO THE LEFT //
  servo.write(100);
  digitalWrite(IN1,LOW);   
  digitalWrite(IN2,HIGH);   
  analogWrite(ENA,120);                                       // SET SPEED FOR THE MOTOR //
  delay(10);
}

void SLIGHTLY_LEFT_MIN(){                                     // VEHICLE SWERVES SLIGHTLY LESS TO THE LEFT //
  servo.write(95);
  digitalWrite(IN1,LOW);   
  digitalWrite(IN2,HIGH);   
  analogWrite(ENA,120);                                       // SET SPEED FOR THE MOTOR //
  delay(10);
}

void SLIGHTLY_RIGHT(){                                        // VEHICLE SWERVES SLIGHTLY LESS TO THE RIGHT //
  servo.write(75);
  digitalWrite(IN1,LOW);   
  digitalWrite(IN2,HIGH);              
  analogWrite(ENA,120);   
  delay(10);                                                  // SET SPEED FOR THE MOTOR //

} 

void SLIGHTLY_RIGHT_MID(){                                    // VEHICLE SWERVES SLIGHTLY LESS TO THE RIGHT //
  servo.write(80);
  digitalWrite(IN1,LOW);   
  digitalWrite(IN2,HIGH);              
  analogWrite(ENA,120);   
  delay(10);                                                  // SET SPEED FOR THE MOTOR //

} 

void SLIGHTLY_RIGHT_MIN(){                                    // VEHICLE SWERVES SLIGHTLY LESS TO THE RIGHT //
  servo.write(85); 
  digitalWrite(IN1,LOW);   
  digitalWrite(IN2,HIGH);              
  analogWrite(ENA,120);   
  delay(10);                                                  // SET SPEED FOR THE MOTOR //

}

void REVERSE_RIGHT(){                                         // VEHICLE SWERVES SLIGHTLY LESS TO THE LEFT //
  servo.write(110);
  digitalWrite(IN1,HIGH);   
  digitalWrite(IN2,LOW);   
  analogWrite(ENA,120);                                       // SET SPEED FOR THE MOTOR //
  delay(10);
}

void REVERSE_LEFT(){                                          // VEHICLE SWERVES SLIGHTLY LESS TO THE RIGHT //
  servo.write(70);
  digitalWrite(IN1,HIGH);   
  digitalWrite(IN2,LOW);              
  analogWrite(ENA,120);   
  delay(10);                                                  // SET SPEED FOR THE MOTOR //

} 

void STOP(){                                                  // VEHICLE STOPS COMPLETELY//
  servo.write(90);
  digitalWrite(IN1,LOW);   
  digitalWrite(IN2,LOW);  
  analogWrite(ENA,0);
  delay(10);
}

void CORRECT(int d1, int d3){                                // VEHICLE IS TOO CLOSE TO A WALL //
 if (d1 < d3) {
    if (d1 < 15) {
      SLIGHTLY_RIGHT();
    } 

    else if (d1 < 30) { 
      SLIGHTLY_RIGHT_MID();
    } 

    else if (d1 < 35) {  
      SLIGHTLY_RIGHT_MIN();  
    } 

    else if(d1 > 35){
      SLIGHTLY_LEFT();
    }    

    else {
      STRAIGHT();
    }
  } 

  else if (d3 < d1) {
    if (d3 < 15) {
      SLIGHTLY_LEFT();
    } 

    else if (d3 < 30) {
      SLIGHTLY_LEFT_MID();
    } 

    else if (d3 < 35) {
      SLIGHTLY_LEFT_MIN();
    } 
   
    else {
      STRAIGHT();
    }
  }
  
}

```
<br>

  1. Now, let´s begin with the main code. First, we check if the button has been pressed. If it hasn´t, the robot must remain still until instructed otherwise.

```ruby
if (digitalRead(buttonPin) == HIGH && cont == 0) {              // START //
  start = true; 
  cont = 1;
  delay(500); 
}

if (digitalRead(buttonPin) == HIGH && cont == 1) {              // STOP //
  start = false;        
  cont = 0;
  delay(500); 
}

if (start == true) {                                            // BUTTON IS PRESSED AND CODE BEGINS //
```
<br>

2. Then, we must check if there is an obstacle present, and swerve to the right condition (left or right).
```ruby
 for (int i = 0; i < pixy.ccc.numBlocks; i++) {
    int blockWidth = pixy.ccc.blocks[i].m_width;
    int blockHeight = pixy.ccc.blocks[i].m_height;
    int blockSize = blockWidth * blockHeight; 

    if (blockSize > highestPrioritySize) {
      highestPrioritySize = blockSize;
      highestPriorityIndex = i;
    }
  }

  if (highestPriorityIndex != -1) {
    pixy.ccc.blocks[highestPriorityIndex].print();
        
    if (pixy.ccc.blocks[highestPriorityIndex].m_signature == 1) {  
      Serial.println("Prioritized Red object detected");
      followBlock_red(highestPriorityIndex, "Red");
    } 
    else if (pixy.ccc.blocks[highestPriorityIndex].m_signature == 2) { 
      Serial.println("Prioritized Green object detected");
      followBlock_green(highestPriorityIndex, "Green");
    }
  }
```
<br>

 3. Then we use our functions to check the distances from the 3 ultrasonic sensors. By receiving this information the vehicle can know if it must move forward, backwards, or give a left/right turn.

```ruby
 for (int i = 0; i < numSensors; i++) {                        // CALCULATE DISTANCES //
    distances[i] = DisCalc(TrigPins[i], EchoPins[i]);
  }
  long distanceMid = DisCalc(Trig_US2, Echo_US2);  
  printDistances(distances[0], distanceMid, distances[1]);
```
<br>

  4. If our middle distance (the one facing forward) detects a distance less than 90 cm, it may be because a turn is near.

```ruby
 if (distanceMid < 90) {                                       // PREPARE FOR A TURN //
    STRAIGHT_SLOW();
  } 
  else {
    STRAIGHT();
  }
```
<br>

  5. Now we check if there is in fact a turn, either left or right. While doing the turn, the vehicle must constantly keep checking the distances to avoid false readings. If the turn was not completed, the vehicle must return and correct.

```ruby
 if (distanceMid < 100 && distances[0] > 100) {
    while (distances[0] > 50) {                                 // TURN LEFT //

      for (int i = 0; i < numSensors; i++) {
        distances[i] = DisCalc(TrigPins[i], EchoPins[i]);       // UPDATE DISTANCES //
      }
      distanceMid = DisCalc(Trig_US2, Echo_US2);            
      printDistances(distances[0], distanceMid, distances[1]);

      if(distanceMid < 10){ 
        REVERSE_LEFT();
        delay(1000);
        break;
      }

      LEFT();                                                      
    }
  }
     
  else if (distanceMid < 100 && distances[1] > 110) {
    while (distances[1] > 50) {                                 // TURN RIGHT //

      for (int i = 0; i < numSensors; i++) {
        distances[i] = DisCalc(TrigPins[i], EchoPins[i]);     // UPDATE DISTANCES //
      }
      distanceMid = DisCalc(Trig_US2, Echo_US2);  
      printDistances(distances[0], distanceMid, distances[1]);  

      if(distanceMid < 10){ 
        REVERSE_RIGHT();
        delay(1000);
        break;
      }

      RIGHT();  
    }
  }

```
<br>

  6. If the middle distance detected something less than 90 but there wasn´t a turn near, the vehicle must continue straight forward, or in the case that it´s too close to any wall, it must correct.

```ruby
 else if(distances[1] < 35 || distances[0] < 35){              // VEHICLE IS TOO CLOSE TO A WALL //
    CORRECT(distances[0], distances[1]);
  }
      
  else{
    STRAIGHT();
  }
```

<br>

** **

# Acknowledgments

We would like to take a space to thank our sponsors, without whom this project could not have come to be. Your help not only made a significant contribution to the progress of this autonomous vehicle but aided in the innovation of technology and design that may very well help other future engineers. We are very grateful for your kindness.

Sincerely, Mariana Flores, Claudio López, and Mariana Vega.

![CETYS (1920 x 500 px) (2)](https://github.com/user-attachments/assets/27a9672c-2f5b-43ed-83a7-8f4b370a78d4)


