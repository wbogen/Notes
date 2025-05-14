# Development 
## Stim Plot Pop-out Refactor
### Todo
- [x] Stim peaks reset to default values for the protocols that are not loaded in the main window
- [ ] Stim ticks do not always visually update in preview window but the values do update to changes
    - When editing ticks for an ROI, the other plots in the column sometimes don't update immediately but will update after interacting with them
    - Difficult to replicate
- [ ] Plots in preview window must be interacted with for batch tick editing to work properly
    - Interacting with another plot in the column counts as an interaction
- [x] Stim Ticks not updating when changing protocol via dropdown
- [x] When loading ROI files back in, tetanusRegions load on the same ROI?
    - This doesn't seem to actually be a problem
- [x] If the stim file was selected but then failed to load video/image stack in LoadAllFiles(), many variables are updated and are now invalid. For now, kill the application instead of trying to rectify this
- [x] Use the loaded protocol for the preview stack
    - [x] Able to load in but all ROIs except for the first one in the protocol have reduced signal quality. When commenting out the downsampling line, all ROIs have reduced quality
    - Data was being transposed an extra time
- [x] stimTicks are sticking to integer values only for protocols with stims <=  1Hz
    - Was a result of stim settings being generated as integers instead of floats
- [x] Save old versions of ROI files from different runs
- [x] Add a number to the ROI button to show which one is active
- [x] Clear contractions and curve when all ROIs have been removed
- [ ] Send another job to the Monday board that changes the preview outputs to the final outputs
- [ ] Make box to show the exact timestamp that is being rendered
- [ ] Active ROI doesn't highlight on switching protocol
- [ ] 
## NMJ Team Feedback 061422
- [x] How to count NMJs
- [x] Output plots from preview window
    - Used matplotlib
- [x] Add markers for static_stim_peaks
- [x] Static popup to cover main window while loading
- [x] Minimize terminal window in batch file
    - Created error_log.txt in case an unhandled exception is raised
- [x] Popup that blocks the window while doing IO or heavy calculations that block the window from updating
    - The popup is also block with current implementation so text may not be dynamic
- [x] Make each subplot its own image file too
## ROI Counting 062322
- [x] Make the pixel magnitude over the entire frame for nmj 
    - Ended up making a hidden ROI that follows the cursor on counting protocols. Calculating the pixel magnitude over the entire frame consumes way too much memory during calculation
- [x] Add NMJ count to agg file
- [x] NMJ counts not showing up on initial load
- [x] ROIs do not load when loadAllFiles from counting protocol
# Structure Notes
- ROIs should be set active from the ROI's ```setRoiActive()``` function
Improved conda environment construction, export plot of preview window, message dialog window, save unhandled exception traceback to disk, static stim tick indicators, hide terminal for batch files

"Add NMJ count outputs to aggregated output file, hidden ROI that updates with mouse cursor location on counting protocols"


# All Feature Validation
- [x] Boot
- [ ] File menu (Initial and repeat loads)
    - [ ] "Load All Files" button
        - [x] Successful selection
        - [x] Unsuccessful selection
        - [x] cancel
    - [x] "Load Stim Files" button (disabled)
        - [ ] Successful selection
        - [ ] Unsuccessful selection
        - [ ] cancel
    - [x] "Load Video" button (disabled)
        - [ ] Successful selection
        - [ ] Unsuccessful selection
        - [ ] cancel
    - [x] "Load Image Stack" button (disabled)
        - [ ] Successful selection
        - [ ] Unsuccessful selection
        - [ ] cancel
    - [ ] "Load Bboxes" button
        - [ ] Successful selection
        - [ ] Unsuccessful selection
        - [ ] cancel
    - [x] "Export ROIs" button (disabled)
    - [ ] "Agg Output Files" button
    - [x] "Exit" button
- [ ] Edit menu
    - [x] "Use Stim Settings"
    - [ ] "Reset Stims"
        - [x] Main window
        - [ ] Popup (cannot verify yet because of bug where stim ticks reset for each new open of window for the protocols that are not the one loaded into the main window)
    - [ ] "Clear Stims"
         - [x] Main window
        - [ ] Popup (cannot verify yet because of bug where stim ticks reset for each new open of window for the protocols that are not the one loaded into the main window)
    - [x] "Clear ROIs"
- [ ] Info menu
    - [ ] "Help" (functionality not yet defined)
    - [ ] "About" (functionality not yet defined)
- [ ] Toolbar
    - [x] "New ROI Size"
    - [x] "Loop video"
        - [x] checkbox
        - [x] start and stop spin boxes
    - [x] "Gain"
    - [x] "Min Threshold" (disabled)
    - [x] "Protocol"
    - [x] "Export ROIs" (Disabled)
    - [ ] "Preview Data"
- [ ] Status Message
    - [ ] Updates during file loading
    - [x] Ends in correct state 
- [x] Video looping
    - [x] Loops on load (doesn't get stuck at the end and have to manually play it)
- [x] ROI
    - [x] Creation
    - [x] manipulation
- [ ] Normalization
    - [x] Operation
        - [x] Subtraction
            - [x] Histogram limits updated
        - [x] Division
        - [x] Off
    - [ ] Mean
        - [x] ROI (disabled)
        - [x] Frame (disabled)
        - [x] Time Range
            - [x] Subtraction Time Range updates
        - [x] Every Stim (disabled)
    - [x] Blur (disabled)
- [ ] Stim plot
    - [x] Stim peak editing
    - [x] creation/deletion
    - [x] spike detection
    - [ ] Tetanus
        - [x]  generation
        - [x] deletion
        - [x] editing
        - [ ] updating the rendering of contraction points when tetanus is created or removed
- [ ] Popup window
    - [x] Loads all expected plots
    - [x] Plots in the correct order
    - [ ] Help button
    - [x] Scroll bars
    - [x] Saves outputs on close
    - [ ] Tetanus and stim locations update between opening and closing window
    - [ ] Real time ROI updates?
    - [ ] Stim ticks update across the column simultaneously

# IO Speedup
H
fid_PRTCL7_F100_PW50_NP20_LOC1_video.avi load time: 19.82s
fid_PRTCL1_F33_PW50_NP20_LOC0_video.avi load time: 49.46s
fid_PRTCL2_F50_PW50_NP20_LOC0_video.avi load time: 31.69s
fid_PRTCL3_F100_PW50_NP20_LOC0_video.avi load time: 18.39s
fid_PRTCL4_F200_PW50_NP20_LOC0_video.avi load time: 11.76s
fid_PRTCL5_F33_PW50_NP20_LOC1_video.avi load time: 51.12s
fid_PRTCL6_F50_PW50_NP20_LOC1_video.avi load time: 34.70s
fid_PRTCL8_F200_PW50_NP20_LOC1_video.avi load time: 11.49s

D
fid_PRTCL7_F100_PW50_NP20_LOC1_video.avi load time: 18.60s
fid_PRTCL1_F33_PW50_NP20_LOC0_video.avi load time: 50.41s
fid_PRTCL2_F50_PW50_NP20_LOC0_video.avi load time: 32.38s
fid_PRTCL3_F100_PW50_NP20_LOC0_video.avi load time: 19.23s
fid_PRTCL4_F200_PW50_NP20_LOC0_video.avi load time: 11.47s
fid_PRTCL5_F33_PW50_NP20_LOC1_video.avi load time: 49.15s
fid_PRTCL6_F50_PW50_NP20_LOC1_video.avi load time: 33.26s
fid_PRTCL8_F200_PW50_NP20_LOC1_video.avi load time: 11.49s

C
fid_PRTCL7_F100_PW50_NP20_LOC1_video.avi load time: 18.51s
fid_PRTCL1_F33_PW50_NP20_LOC0_video.avi load time: 47.16s
fid_PRTCL2_F50_PW50_NP20_LOC0_video.avi load time: 33.55s
fid_PRTCL3_F100_PW50_NP20_LOC0_video.avi load time: 18.28s
fid_PRTCL4_F200_PW50_NP20_LOC0_video.avi load time: 11.56s
fid_PRTCL5_F33_PW50_NP20_LOC1_video.avi load time: 46.14s
fid_PRTCL6_F50_PW50_NP20_LOC1_video.avi load time: 33.39s
fid_PRTCL8_F200_PW50_NP20_LOC1_video.avi load time: 12.48s

**Fatigue file**
48.8. processed 1 downPyr
37.1 no processed
49.7 processed 2 downPyr

**Fatigue File split threads**
59.3 processed 1 downPyr

Split threads (5 loading): {'load': [66.6, 56.5, 63.0], 'proc': [67.2, 57.6, 63.7]}
Uniform threads (8 total): {'load': [48.4, 47.4, 48.1, 46.4, 46.7]}
Uniform threads (7 total): {'load': [46.5, 48.5, 46.9]}
Uniform threads (6 total): {'load': [49.1, 50.6, 46.9]}
Uniform threads (4 total): {'load': [49.1, 49.3, 49.2]}
Uniform threads (2 total): {'load': [60.7, 60.3]}

## Threading Approaches for popout
### Dedicate one thread per video
- Thread per video (8 threads): [190, 194, 191], no fatigue: [126.7, 127.8]
- Dedicate all threads for one video at a time w/ ROI cutting (8 threads): []

Order can make a big difference

### Multiple threads per video
- Create chunks per video equal to the number of threads that can be run (8 threads): 
    {
        'starting threads as they are created': [169.9, 168.9, 170.9],
        'starting threads after they are all created': [175.9, 181.4]
    }
    - doing other things is increasing load time
    - starting threads after all created makesšthe main window responsive faster
CPU chugging harder than dedicated thread method
- Create chunks per video with the same number of frames per thread
    {
        'threads': 8,
        'starting threads as they are created': {300: [177.2, 176.6], 400: [167.8, 166.9], 500: [169.4], 750: [174.2]
        'starting threads after they are all created': []
    }
    {
        'threads': 4,
        'starting threads as they are created': {200: [185.6, 186.5, 182.4], 300: [179.3, 181], 400: [182.2], 500: [184.1], 750: []
        'starting threads after they are all created': []
    }
- Each thread handles video chunks w/ ROI cutting (8 threads): []

# Threading GUI bug fixes and feature changes
- [x] Don't select tetanus from main window
- [x] Popout window progress dialog box not close when closing window
- [x] Feedback from export selections button in main window
- [x] Protocol dropdown does not work
- [ ] Add Help and About functionality from top bar
- [x] Better labeling for loading preview images in status bar
- [x] Disable image stack loading
- [ ] Drag to resize in preview window
- [x] Pause playback when opening preview window
- [x] Right click option to remove all red stim markers in stimplot
- [x] Removing contractions is not saved
# Calculation Changes 100422
## Fidelity
- [x] Fix accuracy for ROIs in tetanus
## Tetanus
Fatigue index is only calculated for longest region. Change to sum all regions together? This is how it works in the current analysis

Hotkey for copying NMJRegionItems
- Load counting videos individually
- ROI loading progress dialog
- Bug fix for scatter plot crashes
- Help box updates

# Updates 4/6/23
1. all peaks
2. some peaks
3. tetanus w/ all peaks
4. inactivity w/ all peaks
5. tetanus w/ some peaks
6. inactivity w/ some peaks
7. tetanus + inactivity w/ all peaks
8. tetanus + inactivity w/ some peaks

active stims / total stims

# Deprecated from 2019
- Write To Spreadsheet in 4_roi_integrate_stim.vi

