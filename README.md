# PLC-Programming
This repository holds a series of PLC practice problems. Each project includes three files:
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
