# PLC-Programming
This repository holds a series of PLC practice problems. Each project includes three files:
* The original RSLogix file (.RSS),
* A PDF of the the full PLC program, and
* A video of the program passing the function test criteria.

## Poject 1: Pressure Tank
Starting off easy, project one has two digital inputs (Low and High pressure switches), and two digital outputs (to a pump and a run indicator light).  When tank pressure drops below L, the pump turns on until pressure reaches H.  When the pressure is above L the run indicator light is on.

My first time through I coded the logic quite literally, and ended up with 4 simple lines of control code.  However, the real world is a little messier than my theoretical world, and in reality sensors will flash on and off at edge conditions.  Therefore I went through and added timers so that outputs only change after the input conditions stabilize.
