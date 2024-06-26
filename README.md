# Autonomous Basil Plant
By Gabriel Zhang, Nami Lindquist
### 1. Video

Link to the final project demo video: https://youtu.be/UKksILaxGA8

### 2. Images
![IMG_0222](https://github.com/JiaxiaoGabrielZhangEE/AutoPlant.github.io/assets/157425369/ffa7cc6c-ceb5-4a2c-a873-5453f4be1f71)
![IMG_0223](https://github.com/JiaxiaoGabrielZhangEE/AutoPlant.github.io/assets/157425369/f169b22e-737a-4caf-b170-8d3ec87e9a46)
![IMG_0225](https://github.com/JiaxiaoGabrielZhangEE/AutoPlant.github.io/assets/157425369/178cd1e2-83c7-4be8-bf4b-744dda9307da)
![IMG_0226](https://github.com/JiaxiaoGabrielZhangEE/AutoPlant.github.io/assets/157425369/4003c6b6-a0ad-49b6-bb3f-62a708ec9454)

### 3. Results

What were your results? Namely, what was the final solution/design to your problem?

The final solution was a platform for the plant to be put onto. This platform has wheels that move in the direction with the most light concentration and can be manually moved with Blynk buttons. There is also an automatic watering capability and an OLED screen that displays to the user the state of the plant (happy, thirsty, drowning). Finally, we also included a crash button at the front of the car so that if it crashes into something, the car will stop moving. 

#### 3.1 Software Requirements Specification (SRS) Results

Here were our original SRS:

* ADC for moisture shall update every 200ms.
* ATMEGA shall control water pump such that it maintains ADC values between 530-570
* ATMEGA shall be able control the cart in all 6 directions (forward, reverse, left, right, strafe left, strafe right)
* ATMEGA shall control the motor such that it spins in place while not moving
* When bump sensor is pressed, platform shall stop in 0.5s
* ATMEGA shall correctly identify the brightest light direction, and control the motors to follow it using a combination of directional moves
* ATMEGA shall determine if the platform is in uniform lighting and not move
* OLED screen shall immediately update after reading moisture values, i.e. every 200ms.
* ATMEGA computes the route around blocks towards the location with strongest light

We successfully met most of our SRSs. First, we programmed our ATMEGA to poll the moisture sensor every 200ms, and update the OLED per reading. Moreover, our water pump stops when ADC is between 530-570. This is done by watering drop by drop so that the ADC values have time to update. By changing the directions of each motor through H-bridge manipulation, we were also able to get the cart to move in all 6 of the specified directions automatically and manually through Blynk. Our ATEMGA can also detect and not move when the cart is in uniform lighting (ADC values are within 120 of each other) and can track the direction of the brightest light direction. It can follow a flashlight pointed at one corner. Lastly, the bump sensor is set up such that when it is initially pressed down, an edge capture on Timer 4 activates, which then activates Timer 3. When Timer 3 overflows (in approximately 0.47s due to 256 prescalar), the status of the button is checked again to make sure it is actually pressed. Then, the platform stops. Thus, this e-stop occurs less than half a second, as specified.

However, due to time constraints, we decided we could not finish software route-planning using data from ultrasonic sensors. Also, given that the processor needs to control motor movement, it is extremely difficult to fit in another computationally complex and real-time monitoring task without a multicore MCU. Thus, we weren't able to meet our stretch goal of route planning, but we were able to detect crashes and stop the cart. Moreover, we also realized that making the platform constantly spin is too draining on the battery and also blocks other functions as the ATMEGA is single core. Thus, we decided not to implement this function.

#### 3.2 Hardware Requirements Specification (HRS) Results

Here were our original HRS:
* Project shall be based on ATmega328PB and ESP 32 microcontrollers
* ESP 32 shall maintain WIFI connection to BLYNK at all times
* ESP 32 should have its own battery to remain connected to Blynk even while the other components are off
* Project shall have a battery life greater than 30 mins
* Motorcontrollers shall be able to spin motors in both directions using H bridge. 
* Water pump shall not overwater the plant, i.e. dumping the entire water tank into the soil
* A capacitive soil sensor with ADC reading shall be used
* 4 light dependent resistors shall be used to determine bighest lighting direction shall be used
* A push switch shall be used as a bump sensor to detect crashes. 
* OLED screen from Pong lab shall be used to display moisture status in 3 levels: too little, just right, and too much
* Ultrasonic sensor should be used to plan a path to the destination around any objects in the way

In general, we also reached our HRS. Our project is made using 2 ATMEGA 328PB boards with one ESP 32 for IOT. One Atmega is used for LDR monitoring, motor control, and collision detection, while the other one is used for the water pump, moisture sensor, and OLED. Our ESP is used solely for manual control of the cart movement on Blynk. For power management, our cart operates on 8 AA batteries, providing more than 30 mins of battery life, as we have been testing the platform for more than that time without any change in battery needed. Moreover, the ESP also has its own power source so that it can remain connected to Blynk even when the main switch is turned off. This is done because if the ESP powers down, it goes into an error state and needs its code reflashed (something we want to avoid). Our motor controllers were also able to switch the forward/reverse directions of each motor, ensuring our cart could move in any of the 6 specified directions by controlling each wheel's spin direction. Our 4 light-dependent resistors are also mounted to each corner of the cart to provide accurate detection of which direction has the strongest light. As said above in the SRS, this allows the Atmega to control the cart's movement in that direction. Our soil moisture sensor can correctly detect the amount of moisture in the soil and, in turn, activate the pump. We sucessfully controlled the pumping rate such that only one drop of water goes into the soil per pump. This avoids any chance of overwatering. Our OLED screen is also able to show the different water levels using frowny, drowning, or happy faces. 

We were able to use a bump switch to detect if the cart crashed or not, but like said before, due to time constraints, we were unable to reach our stretch goal of using ultrasonic sensors for path planning.


### 4. Conclusion

Reflect on your project. Some questions to consider: What did you learn from it? What went well? What accomplishments are you proud of? What did you learn/gain from this experience? Did you have to change your approach? What could have been done differently? Did you encounter obstacles that you didn’t anticipate? What could be a next step for this project?

The beginning of the project was stressful since our hardware parts took a significant amount of time to come. However, once we acquired our parts, testing the individual components worked very well, and powering everything went pretty smoothly for the most part; we did have some trip-ups with powering the ESP, but we found ways to do that efficiently. Figuring out how to control the motors also went smoothly. This laid a great, solid foundation for our work on this project. 

There are many things that we pivoted on when creating this project. The first is that we originally planned on implementing stretch goals that consisted of path-planning or object collision avoidance using an ultrasonic sensor. However, given the short amount of time that we had to complete this project, we decided against including path planning since that would require more sophisticated software than we had time to implement. We also decided against object collision avoidance using an ultrasonic sensor and instead implementing object collision avoidance using a bump switch since that was a simpler solution to object collision avoidance. Another thing we changed was the feature where the plant would constantly rotate in place; we just decided that this feature would drain too much power and was actually unnecessary for the end user. We also decided to use a moisture sensor that would be in the dirt of the plant rather than on the leaf of a plant since we weren't able to find any sensors that were supposed to be attached to the leaf that used ADC or would arrive on time when purchased online. 

When assembling the project, we were trying to 3D print a platform to which we could secure the MCU and ESP so that it would look nice and sleek, but we decided to use the resources around us in Detkin instead and used cardboard, tape, and hot glue to secure the boards on top of the chassis in a faster, simpler, and more cost-effective way. We used tape and hot glue to water-proof the water pump component.

On top of ensuring the proper functionality of all of the components, we also made an effort to make the project visually appealing and sturdy. To do this, we color-coded the wires the best we could and managed them with tape and zip ties. We also 3D printed an encasing for the plant and used cardboard to create space for the MCUs and ESP to fit nicely and neatly onto the chassis. We also made the OLED displays more visually appealing by using a face, in addition to text, to show the plant's status rather than just letting the user know of the plant's status with plain text. We are proud of executing this additional cherry-on-top visual appeal and hardware organization of the project. 

The next step for this project would be creating different water and sunlight settings that users could choose depending on the plant they are trying to take care of. Basil plants thrive in damp soil and constant sunlight, so this project was calibrated to a basil plant, but different types of plants would need a different kind of care so the functionality of the project would have to be different. 

Please note that there were changes in our SRS and HRS. One of the SRSs was using a timer for recording time between watering events, but we didn't implement this since we decided that it was unnecessary since we would simply water the plant depending on the moisture level of the soil. As noted earlier, we also decided against implementing the path planning SRS for reasons mentioned earlier. The one change that we made in the HRS is not using an ultrasonic sensor for reasons mentioned earlier.
