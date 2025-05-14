[[NMJ ROI Analysis Software]]
#  Analysis Notes
## CH1
- [ ] Able to perfectly match ROI location
### .33Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### .5Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 1Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 2Hz
- [ ] Stims correctly align without editing position
- [ ] Analysis results comparable
### 4Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable

## CH2
- [ ] Able to perfectly match ROI location
### .33Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### .5Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 1Hz
- [ ] Stims correctly align without editing position
- [ ] Analysis results comparable
### 2Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 4Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable

## CH3
- [ ] Able to perfectly match ROI location

### .33Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### .5Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 1Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 2Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 4Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable

## CH4
- [ ] Able to perfectly match ROI location
### .33Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### .5Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 1Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 2Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 4Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable

## CH5
- [ ] Able to perfectly match ROI location
### .33Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### .5Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 1Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 2Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 4Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable

## CH6
- [ ] Able to perfectly match ROI location

### .33Hz
**Didn't load**
- [ ] Stims correctly align without editing position
- [ ] Analysis results comparable
### .5Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 1Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 2Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 4Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable

## CH7
- [ ] Able to perfectly match ROI location
### .33Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### .5Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 1Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 2Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable
### 4Hz
- [x] Stims correctly align without editing position
- [ ] Analysis results comparable

## CH8
** No data**
### .33Hz
- [ ] Able to perfectly match ROI location
- [ ] Stims correctly align without editing position
- [ ] Analysis results comparable
### .5Hz
- [ ] Able to perfectly match ROI location
- [ ] Stims correctly align without editing position
- [ ] Analysis results comparable
### 1Hz
- [ ] Able to perfectly match ROI location
- [ ] Stims correctly align without editing position
- [ ] Analysis results comparable
### 2Hz
- [ ] Able to perfectly match ROI location
- [ ] Stims correctly align without editing position
- [ ] Analysis results comparable
### 4Hz
- [ ] Able to perfectly match ROI location
- [ ] Stims correctly align without editing position
- [ ] Analysis results comparable


# Software Improvement Notes- 
- See paper notes for LabView improvements
- [ ] Image stacks take over a minute to load on office computer. Speed of connection probably not a huge factor, many I/O operations the likely cause. #io 
- [ ] Load next protocol in background while analyzing current one?
- [x] Can load image stack and stim file with the same command, already implemented for video
- [x] Add keyboard shortcut to quickly toggle through active ROI
- [x] Bug with video looping range when changing between image stacks? (Seems to be working as expected)
- [x] Indicate the loaded protocol name in interface
- [ ] Add in automated tetanus detection so user only has to select ROIs at the first frequency and will skip past data not in tetanus
- [ ] Pop out window that only loads data from that ROI, lets user see how it performs in the future and compare all protocols simultaneously
- [x] Move ROI Export from File menu to menu bar
- [x] Add button to clear all ROIs, put in Edit menu dropdown
- [x] Place new tetanus regions just before the first stimulation to the end of the where the last contraction would be allowed to end. If tetanus already exists, place it in the same default location as it is now
- [ ] If user hasn't exported ROIs when attempting to go to the next protocol, make popup that notifies the user
- [x] Add some denoising, the compression in LabView is adding quite a bit of fuzz
    - This will not be implemented now because methods available in openCV are way too slow for online processing, the minimum amount of processing takes ~80ms 
- [ ] Combine all output files in a chamber, probably put this next to Export ROI in File menu
- [x] Remove all stims button, put it next to the Reset Stims button in the Edit menu
- [ ] Check how fidelity works

## Tetanus Updates
- [x] Add tetanus values to ROI
- [x] Remove tetanus values from ROI only when removed through menu
    - [x] Add structure to ROI to indicate which protocol tetanus belongs to
- [x] Update tetanus in ROI when the region is moved
- [x] Update rendering of tetanus when changing active ROI
    - [x] remove rendered tetanus for leaving activeROI
    - [x] add tetanus for arriving activeROI
- [x] Update outputs to aggregate
- [x] Tetanus only shows up after interaction when switching protocols or loading in
- [ ] Prevent opening ROIs without having a video open first
# Data Notes
- Timestamps are not perfectly even but don't seem to have a significant impact on data stream visually, need to characterize:
    - Max distance
    - frequency of deviations
    - Does it affect the analysis
    - How uneven is it compared to old LabView
- Should we allow some ROIs to be in tetanus and others not?
- Investigate the sensitivity of tetanus data shapes to ROI placement
- Overall the old data is higher amplitude, probably a results of down sampling, make some comparisons to confirm
- Make the ROI handles a bit bigger
## Metrics to Report
- Amplitude mean and stdev differences
- baseline at the beginning and end of a recording per ROI
- percent ROI in tetanus
    - emphasize limitations of current implementation
- fatigue index
## Obvious data improvements
- Less instance of baseline drift from images
- CH4 is a good example of the improvements from new software
## Notes from personal ROI selection
- CH1 
    - ROI 4 does not do behave at faster stim freqs
    - ROI 2 misbehaves at last freq
- CH6
    - Looks like the table kept getting bumped

## Analysis Actions
- [ ] Compare Image analysis results as is vs down sampled again. Expect the additional down sampling to result in lower amplitude data
- [ ] Select ROIs from data without trying to replicate ROI selection