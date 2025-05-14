# Features

## 1. Choosing what/where to display
### 1.1 Where
- Spike scope opens up in a new window by clicking on the toolbar button
- Simplified version of the spike scope available within toolbar

### 1.2 What
#### 1.2.1 Common
- Spike display
    - See [[#2. Averaging Methods]] for details
- Checkboxes
    - average spike
    - individual spikes
    - spike density
    - Threshold

#### 1.2.2 Pop out
- All channels that are currently loaded in main window
- Each column is a row
    - If no mapping is set, display at most 5 channels per column
- Dropdown to toggle between event types
    - Don't display options for which no events were selected
- Settings
    - Toggle to display
        - average spike
        - individual spikes (coupled with slider)
        - standard deviation thresholds
    - Slider for amount to display up to a hard cap
- Thresholding
    - 

#### 1.2.3 Integrated
Displays in dock, settings for event detection on a channel

##### 1.2.3.1 Spike Scope
- Display scope
- Checkboxes for elements to show, all options present in pop out
    - average spike
    - density
    - threshold (based on current setting)


Settings
- threshold
    - spinbox and toggle for display in spike scope
    - 
- Only displays the scope and a few checkboxes for common toggleable settings
- Only one channel shown at a time
- Channel shown updates with the channel the mouse is hovering over
- Event type displayed is current selectable event type

## 2. Averaging Methods
### 2.1 Visualization
- Can't draw every single spike with transparance for a channel, calculation time grows exponentially
- Draw the density and confidence interval around average spike
- Show a certain percentage of individual spikes up to a hard cap, adjustable by the user
### 2.2 Calculations

## 3. Spike Detection
- Spinbox for current threshold
    - Do calculation for minimum threshold (3std?) and display spikes crossing the defined threshold. Speeds up calculations later
- Spinbox and button to do calculations on one channel
    - Updates based on last channel clicked
- Button for all channels
- Drag and deop threshold line within precalculated range