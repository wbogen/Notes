# General Process
- Make spectrogram for 5 second chunks of signal
- Save 2d output
    - either the raw values or a smoothed PNG
- Pass to model
    - CNN -> FC -> FC -> activation
- outputs a state for the equivalent of ~50ms
    - no spikes, spikes, bursting
- Use thresholding to find the spike/s within the region
    - Template matching not necessary as already have high confidence that there is a spike in this region
    - Look for both positive and negative spikes

# Training Data
## Process
- Use SignalView to identify the location of spikes
- Translate those spikes into label format
    - 0 = no spikes
    - 1 = low density spikes
    - 2 = burst
## Data to save for reproducibility
- individual channel from amplitudes/timestamps
    - save each channel as its own file
    - save raw signal
- metadata file for the data
    - location where amp/time data is stored
    - original name of the RHS file path
    - indices of when spike peaks occur
        - construct categories from this at train time


$\beta^{223}$  