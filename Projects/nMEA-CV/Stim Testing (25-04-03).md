# Pre Experiment
## Setup
- Attach PCBs to systems
- Fill with 100uL of PBS (min 50uL)
    - 270 mOsm in the 4C fridge in GCC
## Validate
- good impedance
- no leaks
- no bubbles
## Intan Setup
- Attach all 6 systems for a full CV setup (assuming all systems are validated)

# Testing Configuration
## Stim channel configurations
Each as a separate recording using default stim settings
- First electrode in every row (the ones outside the tunnel)
- Second electrode in every row (first channel inside the tunnel)
- First electrode in row A
- Second electrode in row A
- First electrode in row C
- Second electrode in row C
## Stim Parameters
Do 2 rounds
1. First electrode in row C
2. Second electrode in row C

**Post Stim Amp Settle**
"Headstage global amp settle" should be selected
- 1ms (default)
- 2ms
- 3ms


**Charge Recovery**
Keep "On" at default (100uS)
Change "Off"
- 500uS (default)
- 1000uS
## Stim Protocol
Stimulations will be manually delivered
1. Do 10 stimulations spaced approx. 1 second apart
2. Wait a few seconds
3. Do 10 stimulations in quick succession

## Resulting Data 
- 16 RHS recordings of about 15-20 seconds each

# Analysis
Identify stim offset duration relative to stimulation onset by finding the timestamp at which the voltage becomes <10uV different from the average of 1ms prior to stimulation onset.