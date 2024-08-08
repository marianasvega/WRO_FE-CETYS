# CETYS FUTURE ENGINEERS 2024 🤖
This is the official repository for the CETYS Team participating in the Future Engineers category at the 2024 Mexican National. Here we will upload all of the process and results regarding our autonomous vehicle, as well as a full report on the code used and its electrical components.

FOTO

# Content
* `Models` Files for models used by 3D printers, laser cutting machines and CNC machines to produce the vehicle elements.
* `Others` Files which can be used to understand how to prepare the vehicle for the competition. 
* `Schemes` Diagrams of the electromechanical components used in the vehicle and how they connect to each other.
* `Src` Contains code of control software for all components which were programmed to participate in the competition.
* `Team Photos` Contains 2 photos of the team.
* `Vehicle Photos` Contains 6 photos of the vehicle.
* `Video` Contains the video.md file with the link to a video where driving demonstration exists.

# Introduction
Arturo, our autonomous vehicle, is able to perfom remarkably thanks to a variety of diferent mechanisms that when, assembled together correctly, can help a robot accomplish a challenge such as the Future Engineers Category in the WRO Competition. So as to make clear how our robot operates, we will continue to explain how each of these mechanisms work, including which specific electronic components were used and the logic behind it´s code.

### Robot´s Vision (Pixy 2 Camera) 📷
  For our vehicle to be able to detect the obstacles present on the rink (red and green traffic lights and the parking walls), we decided to use the Pixy2 Camera. Pixy2 can be programed to detect specific signatures by recognizing it´s shape and/or color. After you complete the settings on the camera´s app, you are all set to start programming what you want your vehicle to do when detecting each signature. For example, we used the following code (the sintaxis of the commands correspond to the Pixy2 Arduino library):

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
### Ultrasonic Sensors (HC-SR04) 
### Rear Tire Axle (DC Motor and LEGO Diferential) ⚙️
## How it all comes together (explaining the code) 
