Records standard cantilevers using laser deflection to determine the distance traveled. The software is written in [[LabVIEW]].

# Measurements from analysis
- [[Amplitude]]
- [[contractile force]]
- [[Beat Frequency]]

# Operation
## Inputs
- Multiple interactions with numeric inputs, checkboxes, and other button presses used for selecting cantilever locations and some control over laser location
- Gamepad for fine control over laser position.

# Development
## May 2022
Adding additional commands to the gamepad to reduce mouse and keyboard inputs when determining the location of cantilevers.
### Proposed controls
- [x] Use the face buttons to select cantilevers 1, 16, 17, and 32 (the corners of the chip)
    - Mapping: 1=up, 17=right, 32=down, 16=left
    - If a corner cantilever is missing, automatically set the closest checked cantilever as an interpolation point instead
- [x] Use L3 to call the interpolate command for the positions of cantilevers 1-16 and R3 for cantilevers 17-32
- [x] Traverse the distance of 16 cantilevers when pressing L2 and a d-pad direction and the distance between the left and right columns while holding R2.
    - [x] The distance will be hardcoded based on the expected distance. A [[UI Elements#spinbox |spinbox]] can be included to let users edit this value but the expected number will be the default.
    - [x] Only allow L2 to go up-down, and only allow R2 to go left-right
    - [ ] The distance can be modulated based on which is the closest cantilever to the edge checked. For example, if CL3 is the topmost one checked and CL16 is the bottommost channel checked, the laser will travel the expected distance of 13 cantilevers.
- [x] The start and select buttons are mapped to modulate the motor speed, remap one of these to be used as a key combination with a face button to move the laser directly to that cantilever. The other can be remapped to stop the motor.
- [x] Get link to buy two controllers. Probably just use most recent revision of Xbox controllers. Ma allow us to use the analog triggers
- [x] static com ports for initializing devices
- [x] Move the motor with the control stick
    - Need to control both direction and speed
    - Add a dead zone to the center
    - Using only left control stick, right stick is unbound
### Challenges
- There is a command to stop the motor already programmed to the left face button that needs to be remapped

### Testing Updates 051722
- [x] Bind the R2 functionality onto L2 and have the distance specific to each direction
    - It was difficult to remember which button did what
    - R2 is now freed up to do something else, perhaps an analog input
- Control stick input kept causing motors to disconnect and the USB connection had to be physically reset after some instances

### Control Stick attempted implementations
1. Set the motor speed and then set the distance to move
    - Frequently caused motors to disconnect and they either had to b restarted in the motor software or the USB connection needed to be physically reset followed by identifying motors in their software
    - ![[Pasted image 20220518102024.png]]
2. Set the motor as a nested while loop that continues as long as the threshold value is met
    - Still had the same problem with the motor disconnecting and with the motor steps being slow and non-constant. The motor seems to disconnect when a variable speed is passed and does not disconnect when fed a constant number. 

### Things to check out during acellular testing
- [ ] What is the distance between two cantilevers (CL1-2)
    - Give this value to **CL dist** and will be used to determine how far to travel for L2 motor movement within the same row
    - A static multiple or a constant can be used for traversing across rows (CL1-17)