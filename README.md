** **

# CETYS FUTURE ENGINEERS 2024 🤖

This is the official repository for the CETYS Team that participated in the Future Engineers category at the 2024 Mexican National and will soon compete in the International Final in Izmir, Türkiye! Here we will upload all of the processes, improvements, and results regarding our autonomous vehicle, as well as a full report on the code used and its electrical components.
  
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
  <p><be>This report outlines the design, development, and performance of Arturo, the autonomous vehicle developed by our team, the CETYS Future Engineers, for the 2024 World Robot Olympiad. This document will provide a comprehensive overview of our previous work in the Mexican national competition, where we successfully showcased our prototype and the iterative improvements we made in preparation for the international championship in Türkiye.
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
| **11.1V Battery 4000mAh**           |       1       |
| **7.4V Battery 1000mAh**            |       1       |
| **330Ω Resistor**                   |       1       |
| **HuskyLens Camera**                |       1       |
| **Switch Button Interruptor KCD1**  |       2       |
| **Gyro + Accelerometer MPU-6050**   |       1       |

<br>

** **

# Mechanism explanation


### `Robot´s Vision (HuskyLens Camera) 📷`

<div style="display: flex; align-items: flex-start;">
  <img align="left" width="200" src="https://github.com/user-attachments/assets/72711018-55cd-4ef5-8e49-a40d3d569d39" alt="HuskyLens Camera" style="margin-right: 20px;">
  <p>
    For our vehicle to better detect the obstacles on the rink, we decided to upgrade from the Pixy2 Camera to the HuskyLens Camera. This decision was made after much research and due to a compatibility problem we had while trying to program the Pixy2 with the ESP32. HuskyLens is an AI-powered vision sensor designed for a wide range of applications, including robotics, DIY electronics, and IoT projects such as this. This compact camera module integrates machine learning algorithms to perform tasks like face detection, object tracking, color identification, and gesture recognition, enabling intuitive interaction and intelligent automation. For this component, we used the following code (the syntax of the commands corresponds to the HuskyLens Arduino library):
  </p>
</div>


```ruby
if (huskylens.request()) {                                        // REQUEST DETECTED OBJECTS FROM CAMERA //
    for (int i = 0; i < huskylens.countBlocks(); i++) {
      HUSKYLENSResult result = huskylens.getBlock(i);             // CHECK IF THE DETECTED COLOR IS RED OR GREEN //
      if (result.ID == 1) {                                       // GREEN COLOR (BASED ON OUR PROGRAMMING) //
        Serial.println("Detected Green Object!");
        Serial.println("X Position: " + String(result.xCenter));
      }
      else if (result.ID == 2) {                                  // RED COLOR (BASED ON OUR PROGRAMMING) //
        Serial.println("Detected Red Object!");
        Serial.println("X Position: " + String(result.xCenter));
     }
   }
 }
```

<br>
  
One of the main benefits of HuskyLens is its ability to bring sophisticated AI and machine learning capabilities to users without requiring deep technical expertise. It can also perform real-time tasks without an internet connection, leading to faster response times and reduced latency. Finally, it is compatible with various microcontrollers and development platforms, such as Arduino, ESP32, and Raspberry Pi, and supports multiple communication protocols, including UART and I2C.

<br>
<br>
  
### `Front Tire Axle (Ackerman Steering and HS-322HD Servo) 🛞`

During the Mexican National Competition, we observed that our vehicle needed a better steering system that could round corners with much more facility and precision. After much research, we learned about a mechanism named the Ackerman Steering Principle. When a vehicle turns, the inner wheel follows a tighter path than the outer wheel. To ensure smooth turning without excessive tire wear or slipping, the inner wheel needs to turn at a sharper angle than the outer wheel. This is precisely what the Ackermann geometry achieves.

<br>

<div style="display: flex; align-items: flex-start;">
  <img align="left" width="250" src="https://github.com/user-attachments/assets/e0d1e6af-4d0f-4fda-b4cd-e1481a7a86f7" alt="Four-Wheel Steering Design" style="margin-right: 20px;">
  <p>
    <strong>Figure 1:</strong> Design and Development of Four-Wheel Steering for All Terrain Vehicle (Vishnu 2020):  
    When a driver turns the steering wheel, the motion is passed through the steering column to the steering gear. This gear changes the rotation into a pushing or pulling movement, which is sent to the tie rods. The tie rods then adjust the steering arms connected to the wheels. This setup ensures that the inside wheel turns at a sharper angle than the outside wheel, helping the vehicle turn more efficiently while reducing tire wear and slip.
  </p>
</div>

<br>

> [!NOTE]
>For the STL files to print the Ackerman system we used, visit the following link for it is NOT of our property and we give full credits to the owner: [https://github.com/KarenWon9/WRO-FI-Team-Spark](url)


<br>


<div style="display: flex; align-items: flex-start;">
  <img align="left" width="135" src="https://github.com/user-attachments/assets/8e8a99a0-a5fc-449d-b4ea-e0ab55b098a0" alt="HS-322HD Servo" style="margin-right: 20px;">
  <p>
    On the other hand, the HS-322HD is a popular heavy-duty servo known for its durability and performance, and without it, our vehicle would have no direction. It features Hitec's Karbonite gear train, which is significantly stronger than standard nylon gears, making it suitable for various applications. With a good balance of speed and torque, the HS-322HD is often used in RC airplanes, helicopters, and other models. To use the servo in the code, it is necessary to include the "Servo.h" library. After that, all that it takes to change the servo's angle is the following command.
  </p>
</div>


```ruby
servo.write(45);
```
<br>
<br>


### `Ultrasonic Sensors (HC-SR04) 📏`
The primary challenge in this category is navigating corners and avoiding collisions with walls. To address this, we employed the HSR04 ultrasonic sensor, which operates by emitting ultrasonic pulses and measuring the time it takes for the echoes to return after bouncing off an object. This time delay is then converted into a distance measurement, allowing the sensor to detect objects and measure distances with precision accurately. 


<br>

<div style="display: flex; align-items: flex-start;">
  <img align="left" width="135" src="https://github.com/user-attachments/assets/7646ba01-3088-4c38-a8b6-38dd5b4f73b8" alt="Ultrasonic Sensor" style="margin-right: 12px;">
  <p>
    Our autonomous vehicle uses 3 ultrasonic sensors in the front part. We have a middle sensor that is always facing forward (parallel to any wall when driving straight), the other two are positioned perpendicular to the first, one on the left side and the other to the right. These two “side” ultrasonic sensors are what help us know at any moment if we're driving too close to a wall or if a turn is near. To keep our driving as straight as possible, we added two extra ultrasonics, each one collinear to a side sensor. For a better understanding of how each sensor is positioned, please visit our <strong>Vehicle Images</strong> section.
  </p>
</div>


<br>


The HSR04 sensor typically has a range of 2 centimeters to 4 meters and is favored for its ease of integration with microcontrollers like Arduino due to its simplicity and cost-effectiveness. Its compact design and straightforward interface make it ideal for applications involving obstacle detection, proximity sensing, and distance measurement in various DIY and industrial projects. To process and interpret this signal given in centimeters, we use the following code:

<br>


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

### `Gyro + Accelerometer (MPU-6050) 📐`
In both the open and the obstacle avoidance challenge, the vehicle must automatically stop after completing three full rounds. To accomplish this, we decided to use a combination of an accelerometer and a gyroscope. The accelerometer tracks the vehicle’s movement and angle, helping confirm its motion along the course, while the gyroscope measures precise angular changes, allowing us to identify each turn with accuracy.

<br>


<div style="display: flex; align-items: flex-start;">
  <img align="left" width="95" src="https://github.com/user-attachments/assets/729c3b26-2511-489d-9d59-2959e4cc3db7" alt="MPU-6050 Gyroscope" style="margin-right: 20px;">
  <p>
    Using the gyroscope, each directional change of the vehicle is logged based on its orientation. Specifically, a 90° change corresponds to a left turn, while a -90° change corresponds to a right turn. This setup allows us to monitor every turn the vehicle makes throughout its path. By counting these 90° increments, we can accurately detect each completed round, ensuring reliable tracking over multiple rounds. Once the vehicle has completed three rounds, this data is used to signal an automatic stop.
  </p>
</div>


<br>

To achieve this, the following code uses the MPU6050.h library to read data from the MPU6050 gyroscope and accelerometer. It tracks changes in the yaw angle, incrementing by 90° with each turn. By monitoring these increments, the program counts full rotations and stops the vehicle after it reaches the target round count:

<br>

```ruby
MPU6050 mpu;                 // INITIALIZE MPU 6050 //

float ANG = 0;               // VARIABLE TO STORE YAW ANGLE //
unsigned long lastTime = 0;  // STORE THE LAST TIME TIME readYawAngle WAS CALLED //

void setup() {
  Serial.begin(115200);
  
  Wire.begin(21, 22);        // INITIALIZE I2C - SDA, SCL FOR ESP32 //

  // INITIALIZE MPU6050 ACCELEROMETER/GYRO // 
  if (!mpu.begin(MPU6050_SCALE_2000DPS, MPU6050_RANGE_2G)) {
    Serial.println("Could not find MPU6050");
    while (1);
  }
  Serial.println("MPU6050 initialized");
  
  mpu.calibrateGyro();       // SET ZERO REFERENCE FOR YAW //

  lastTime = millis();       // INITIALIZE lastTime //
}

void loop() {
  ANG = readYawAngle();      // GET THE CURRENT YAW ANGLE //
  Serial.print("Yaw Angle: ");
  Serial.println(ANG);

  // CHECK IF A 90-DEGREE TURN HAS BEEN MADE //
  if (ANG >= 90.0 || ANG <= -90.0) {
    Serial.println("90-degree turn detected!");
    ANG = 0;                 // RESET YAW ANGLE //
}
delay(10);
}

float readYawAngle() {
  Vector rawGyro = mpu.readRawGyro();
  float YR = rawGyro.ZAxis/130.0;       // CORRECT SENSITIVITY FOR 2000DPS RANGE //
  
  unsigned long currentTime = millis(); // CALCULATE TIME DIFFERENCE IN SECONDS //
  float dt = (currentTime - lastTime)/1000.0; // CONVERT TO SECONDS //
  lastTime = currentTime;                     // UPDATE lastTime FOR NEXT CALL //  
  
  float driftRate = .02 / 3.0;   // DRIFT RATE OF 1 DEGREE EVERY 3 SECONDS //
  // INTEGRATE YAW RATE OVER TIME TO GET YAW ANGLE //
  ANG += YR * dt;
  ANG -= driftRate * dt;         // Subtract the estimated drift

  return ANG;
}
```
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

The motor used in this mechanism is a Faulhaber MOT-165. We opted for this motor because it usually operates at voltages ranging from 6V to 12V, the speed of the motor varies with voltage and load conditions, but it is designed to achieve high RPM (revolutions per minute), and it provides a modest amount of torque, suitable for small, precise applications. It is important to mention that the motor and its driver, the L298N dual H-Bridge, have their power supply, a _**Li-Po battery of 1000 mAh and 7.4V**_.
<br>

<br>


** **


## How it all comes together (explaining the code) 💻
<br>

We will now discuss how all of the previously mentioned mechanisms assemble together in our final code (Open and Obstacle Challenge). Before we start, we will show all of the functions used along the code to use as reference for when they appear next.

```ruby
int DisCalc(int TP, int EP) {                                                // ULTRASONIC BASIC CODE //

  long duration = -1;
  int reading = 120; 
  int cont = 0;

  while (cont < 5) { 
    digitalWrite(TP, LOW);
    delayMicroseconds(2);
    digitalWrite(TP, HIGH);
    delayMicroseconds(10);
    digitalWrite(TP, LOW);

    duration = pulseIn(EP, HIGH, 10000); 

    if (duration > 0) {
      reading = duration * 0.034 / 2; 
      break;  
    }

    cont++;  
  }

  return reading;

}


void printDistances(long distanceLeft, long distanceHleft, long distanceMid, long distanceHright, long distanceRight){

  Serial.print("Dis Left: ");
  Serial.print(distanceLeft);
  Serial.print(" Dis MidLeft: ");
  Serial.print(distanceHleft);
  Serial.print(" Dis Mid: ");
  Serial.print(distanceMid);
  Serial.print(" Dis MidRight: ");
  Serial.print(distanceHright);
  Serial.print(" Dis Right: ");
  Serial.println(distanceRight);

}


void printDistancesM(long distanceMid) {

  Serial.print(" Dis Mid: ");
  Serial.println(distanceMid);

}


float readYawAngle(int X) {                                                  // GYROSCOPE BASIC CODE //

  Vector rawGyro = mpu.readRawGyro();
  float YR = rawGyro.ZAxis / 131.0; 
  
  if (X == 0){
    unsigned long currentTime = millis();
    float dt = (currentTime - lastTime) / 1000.0; 
    lastTime = currentTime; 

    ANG += YR * dt;
    ANG -=   .02;  
  }

  else if (X == 1){
    unsigned long currentTime = millis();
    float dt = (currentTime - lastTime) / 1000.0; 
    lastTime = currentTime; 
  
    ANG += YR * dt;
    ANG +=   0.019;  
  }

  else if (X == 2) {
    unsigned long currentTime = millis();
    float dt = (currentTime - lastTime) / 1000.0; 
    lastTime = currentTime; 
  
    ANG += YR * dt;
    ANG -=   .075;  
  }
  
  return ANG;

}


void OBSTACLE(){

  if (huskylens.request()) {                                                  // OBTAIN DATA FROM HUSKYLENS //
    for (int i = 0; i < huskylens.countBlocks(); i++) {
      HUSKYLENSResult result = huskylens.getBlock(i);
      if (result.ID == 1) {                                                   // ID1 = GREEN //
        GreenO(result.xCenter);
      }
      else if (result.ID == 2) {                                              // ID2 = RED //
        RedO(result.xCenter);
      }
    }
  }

}


void GreenO(int X){                                                           // OBJECT DETECTED IS GREEN //

  Serial.println("Detected Green Object!");

  if (X < 90 ){
    while(X < 90){
      SLIGHTLY_LEFT_MAX();
    } 
  }

  else if(X < 158 && X > 90){
    while(X < 158 && X > 90){
      SLIGHTLY_LEFT();
    } 
  }

  else if(X > 158 && X < 240){
    SLIGHTLY_LEFT_MID();
  }

  else if(X > 240){
    SLIGHTLY_LEFT_MIN();
  }

}

void RedO(int X){                                                             // OBJECT DETECTED IS RED //

  Serial.println("Detected Red Object!");

  if (X < 90 ){
    while(X < 90){
      SLIGHTLY_RIGHT_MIN();
    }
  }

  else if(X < 158 && X > 90){
    while(X < 158 && X > 90){
      SLIGHTLY_RIGHT_MID();
    } 
  }

  else if(X > 158 && X < 240){
    while(X > 158 && X < 240){
      SLIGHTLY_RIGHT();
    }
  }

  else if(X > 240){
    while(X > 240){
      SLIGHTLY_RIGHT_MAX();
    }
  }

}


void STRAIGHT(){                                                              // DIFFERENT VELOCITY CONFIGURATIONS //

  servo.write(93);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  ledcWrite(ENA, 100);  
  delay(10);

}

void STRAIGHT_SLOW(){

  servo.write(94);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  ledcWrite(ENA, 70);  
  delay(10);

}

void RIGHT(){

  servo.write(63);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  ledcWrite(ENA, 90);  
  delay(10);

}

void SLIGHTLY_RIGHT(){

  servo.write(79);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  ledcWrite(ENA, 90); 
  delay(10);

}

void SLIGHTLY_RIGHT_MAX(){

  servo.write(74);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  ledcWrite(ENA, 90);  
  delay(10);

}

void SLIGHTLY_RIGHT_MID(){                     

  servo.write(84);
  digitalWrite(IN1,HIGH);   
  digitalWrite(IN2,LOW);            
  ledcWrite(ENA, 100);  
  delay(10);                

} 

void SLIGHTLY_RIGHT_MIN(){                    

  servo.write(86);
  digitalWrite(IN1,HIGH);   
  digitalWrite(IN2,LOW);         
  ledcWrite(ENA, 100);   
  delay(10);                  

} 

void REVERSE_RIGHT(){

  servo.write(119);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  ledcWrite(ENA, 100);  
  delay(10);

}

void LEFT(){

  servo.write(125);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  ledcWrite(ENA, 90); 
  delay(10);

}

void SLIGHTLY_LEFT(){

  servo.write(109);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  ledcWrite(ENA, 90); 
  delay(10);

}

void SLIGHTLY_LEFT_MAX(){

  servo.write(114);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  ledcWrite(ENA, 90);  
  delay(10);

}

void SLIGHTLY_LEFT_MID(){                      

  servo.write(104);
  digitalWrite(IN1,HIGH);   
  digitalWrite(IN2,LOW); 
  ledcWrite(ENA, 100);  
  delay(10);
}

void SLIGHTLY_LEFT_MIN(){                   

  servo.write(102);
  digitalWrite(IN1,HIGH);   
  digitalWrite(IN2,LOW);
  ledcWrite(ENA, 100); 
  delay(10);

}

void REVERSE_LEFT(){

  servo.write(69);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  ledcWrite(ENA, 100);  
  delay(10);

}

void STOP(){

  servo.write(94);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  ledcWrite(ENA, 0);  
  delay(10);

} 

void CORRECT(int d1, int d2, int d3, int d4){                                 // VEHICLE IS TOO CLOSE TO A WALL //

  if (d1 < d4) {
    if (d1 < 23) {
      SLIGHTLY_RIGHT();
    } 
    else if (d2 < 23) {
      SLIGHTLY_RIGHT();
    }      
  }
  
  else if (d4 < d1) {
    if (d4 < 23) {
      SLIGHTLY_LEFT();
    } 
    else if (d3 < 23) {
      SLIGHTLY_LEFT();
    }  

  }

}


void CORRECT1(float ang) { 
 if (ang > 3)
{
  SLIGHTLY_RIGHT_MIN();
}
else if (ang < -3)
{
  SLIGHTLY_LEFT_MIN();
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


![CETYS (1920 x 500 px)](https://github.com/user-attachments/assets/173353a6-418b-4d88-a202-8c9f04a39d2e)


