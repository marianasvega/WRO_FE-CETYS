# CETYS FUTURE ENGINEERS 2024 🤖

This is the official repository for the CETYS Team participating in the Future Engineers category at the 2024 Mexican National. Here we will upload all of the processes and results regarding our autonomous vehicle, as well as a full report on the code used and its electrical components.

**Team Members:** Mariana Sofia Vega Contreras, Claudio Iván López Valle & Mariana Flores Martínez.

![CETYS (1920 x 500 px)](https://github.com/user-attachments/assets/a858a081-411f-4ef9-bae7-2771a890f4c4)

<br>

# Content
* `Models` Files for models used by 3D printers to produce the vehicle elements.
* `Others` Files which can be used to understand how to prepare the vehicle for the competition. 
* `Schemes` Diagrams of the electromechanical components used in the vehicle and how they connect.
* `Src` Contains code of control software for all components which were programmed to participate in the competition.
* `Team Photos` Contains 2 photos of the team.
* `Vehicle Photos` Contains 6 photos of the vehicle.
* `Video` Contains the video.md file with the link to a video where a driving demonstration exists.
<br>

# Introduction 👷‍♀👨‍💻👩‍🔧
Arturo, our autonomous vehicle, can perform thanks to a variety of different mechanisms that when assembled correctly, can help a robot accomplish a challenge such as the Future Engineers Category in the WRO Competition. To make clear how our robot operates, we will continue to explain how each of these mechanisms works, including which specific electronic components were used and the logic behind its code.

![ARTURO](https://github.com/user-attachments/assets/d106c4f6-eba3-49f1-b9f9-482aa9c6ef04)

<br>


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
<br>


# Mechanism explanation

### `Robot´s Vision (Pixy 2 Camera) 📷`

(the syntaIn order for our vehicle to detect the obstacles on the rink (red and green traffic lights and the parking walls), we decided to use the The Pixy2 Camera is a versatile vision sensor designed for robotics and automation projects. It offers powerful image recognition capabilities and is user-friendly for integrating visual processing into various applications. After you complete the settings on the camera´s app, you are all set to start programming what you want your vehicle to do when detecting each signature. For example, we used the following code xis of the commands correspond to the Pixy2 Arduino library):

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
  
### `Front Tire Axle (HS-322HD Servo) 🛞`
To enhance the vehicle's direction at every turn, we installed a free-steering system on the front axle, controlled by an HS-322HD Servo. The servo’s linkage connects to a custom-designed axle with a gap that allows the tires to rotate freely. Given the speed provided by the rear axle motor, the front tires do not require additional power to move forward.

The following is an example of what we explained in the previous paragraph.

https://github.com/user-attachments/assets/525eaddd-1dce-4344-86ee-807fe781c140

<br>

> [!NOTE]
>The servomotor shown in the video is not the same as the one we used in the end.

To use the servo in the code, it is necessary to include the "Servo.h" library. After that, all that it takes to change the servo´s angle is the following command. 

```ruby
servo.write(45);
```
<br>
<br>

### `Ultrasonic Sensors (HC-SR04) 📏`
The primary challenge in this category is navigating corners and avoiding collisions with walls. To address this, we employed ultrasonic sensors, which measure the distance to an object by emitting ultrasonic sound waves and converting the reflected waves into an electrical signal. To process and interpret this signal in centimeters, we use the following code:

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


https://github.com/user-attachments/assets/d7e541df-3d19-4036-9754-b866e7325a33




<br>

> [!NOTE]
>If you want to learn how a differential works you can watch [this](https://www.youtube.com/watch?v=K4JhruinbWc) video but a differential gear allows wheels on the same axle to rotate at different speeds. It works by using a set of gears to distribute torque between the wheels, enabling them to turn at different rates, which is essential for smooth cornering. If 𝑇 is the total torque supplied by the engine to the differential, and it is divided between the two wheels, then the torque 𝑇<sub>1</sub> on one wheel and 𝑇<sub>2</sub> on the other wheel satisfy 𝑇 = 𝑇<sub>1</sub> + 𝑇<sub>2</sub>.
<br>
The motor used in this mechanism is a Faulhaber MOT-165. We opted for this motor because it usually operates at voltages ranging from 6V to 12V, the speed of the motor varies with voltage and load conditions, but it is designed to achieve high RPM (revolutions per minute), and it provides a modest amount of torque, suitable for small, precise applications. It is important to mention that the motor and its driver, the L298N dual H-Bridge, have their power supply, **a Li-Po battery of 1000 mAh and 7.4V**.
<br>
<br>


## How it all comes together (explaining the code) 💻
<br>
<br>

# Acknowledgments

We would like to take a space to thank our sponsors, without whom this project could not have come to be. Your help not only made a significant contribution to the progress of this autonomous vehicle but aided in the innovation of technology and design that may very well help other future engineers. We are very grateful for your kindness.

Sincerely, Mariana Flores, Claudio López, and Mariana Vega.

![CETYS (1920 x 500 px) (2)](https://github.com/user-attachments/assets/27a9672c-2f5b-43ed-83a7-8f4b370a78d4)


