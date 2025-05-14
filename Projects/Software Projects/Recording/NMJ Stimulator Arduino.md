Waits 500ms between accepting commands

# Functions
## listenToSerialPort()
### Serial port receives...
<b>*</b>
- Set all values in input buffer to '0'

**&**
1. Convert first character in buffer to equivalent integer, allowed characters that are not integers ("-", ".") will be negative
2. Make sure at least 500 ms have passed since last command sent

## runStimulusSweep()
1. Do nothing if  `isStimulating == false`
2. iterate `currentProtocolN` from 0 to `nProtocols`
3. 