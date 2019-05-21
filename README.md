# PLC-Programming
This repository holds a series of PLC practice problems. Each project consists of at least three files:
* The original RSLogix file (.RSS),
* A PDF of the the full PLC program, and
* A video of the program passing the function test criteria.

## Project 1: Pressure Tank
Starting off easy, project one has two digital inputs (Low and High pressure switches), and two digital outputs (to a pump and a run indicator light).  When tank pressure drops below L, the pump turns on until pressure reaches H.  When the pressure is above L the run indicator light is on.

My first time through I coded the logic quite literally, and ended up with 4 simple lines of control code.  However, the real world is a little messier than my theoretical world, and in reality sensors will flash on and off at edge conditions.  Therefore I went through and added timers so that outputs only change after the input conditions stabilize.

## Project 2: Nut Filling Station
Again using digital IOs, the aim of project two was to control the process of bringing a box to a nut hopper, filling the box with one of two types of nuts, and sending the full box away to the end nut consumers.  

There were four inputs: a proximity switch to detect the presence of the box, two photo eyes to check the label colour in order to dispense the appropriate type of nut, and a level sensor to signal when a box was full.  These were used to control three outputs: the conveyor motor to move the box to and from the filling station, and the two individual hoppers, one for walnuts and one for pecans (which I, personally, prefer).

This time I was liberal with my trigger/ latch/ interrupt rungs, and I got creative with adding extra conditions onto these rungs.  From seeing the instructor's solution I realized that storing the state of the system in an integer was the much easier way to go, and made the program easier to read and easier to debug to boot.  Lesson learned: always consider using state logic.

There were a few other lessons learned as well.  In summary:
* Always consider using state logic
* Use ladder files to make the program readable
  * specifically, consider having a "CYCLE" ladder for setting the different states
* Keep an eye out for actions (eg MOV) that could repeat multiple times - consider using One Shots
* *Always* consider what would happen if a sensor false triggered
  * Might make you use a trigger instead of direct activation to avoid flipping a motor on and off

## Project 3: Barcode Scanner
This project was a little different - no input or output signals!  Instead, we were using the PLC memory banks as an inventory for three types of parts.  Based on a "barcode" string, the program would update the quantity of parts held in the inventory.  

Although my approach was quite different from the instructors, I was happy enough with the result that I didn't update my original solution.  I was a little stuck at first due to the lack of a "not equal" string comparator block.  I solved this by using the "equal" block, comparing the incoming barcode register to a blank string, leading to a TND (temporary end) command, therefore sending the scan back to the top of the program, ie the start of this line.  

The other approach would have been to use a bit instead of the TND, and have all the following runs start with a XIO (examine if open) instruction.  Works perfectly well, but I think my approach will catch incoming barcode information just a hair quicker!

## Project 4: Water Treatment System
Here we have a water treatment system being integrated into an existing system.  The program controls a singla output: a signal to a servo that turns a cam.  Depending on the position of the cam it could be closing one of four switches to indicate that it's in one of three cycle states, or that it's at the home position.  

The final input is a digital cycle call input, from the existing system.  When it activates the cycle begins and the cam cycles through activating the three switches for varying amounts of time.

I was happy with my solution and didn't modify it based on the instructor's solution.  By comparing the actual state of the system with the desired state of the system the program knew if the cam should energize or not.  The logic controlling the desired state was based on the switches, actual and desired position, and a series of timers.  

That being said, there might still be a neater way of orgnaizing this logic, that is, instead of just considerng the state of the water treatment system (fill/ drain/ etc), also consider the state of the program itself: run, idle, stop.

And that reminds me of... PackML.

![image of PackML process](https://upload.wikimedia.org/wikipedia/commons/9/98/PackML_State_Model.png "PackML Process Flow")

There's an idea: Using the PackML approach for cyclic PLC programs would be a good practice to use until I learn of a better alternative.

## Project 5: Pipeline Oil Flow
Time for some math!

This scenario involves a pipe filled with flowing oil.  A digital sensor is measuring the fluid flow, sending out a pulse every time 6.3 gallons of oil pass.  The PLC program needs to calculate the flow speed based on this pulse.

Luckily, math is one of my strong points.  This project was quick to complete: just count the number of seconds between pulses, divide the sensor k factor by the time between pulses, and multipy by 60s to get instantaneous flow in gallons per minute.  Done!
