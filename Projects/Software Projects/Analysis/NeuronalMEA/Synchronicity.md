# Burst Grouping Approaches

## Density
- Iteratively build a burst, checking that the density is above a cutoff at each step. 
- If the density falls below the cutoff, can try removing the first spike in the burst to see if that raises the density above the cutoff.
- Density may be calculated as (last_spike_time - first_spike_time) / n_spikes_in_burst

## Center
- Iteratively build a burst, recalculating the center point as each spike is added
- If the next spike that has not been added is within the distance cutoff, add it and recalculate center
**Potential center calculations**
1. first_spike_time + ((last_spike_time - first_spike_time) / 2)
2. average of all spike times

## Clustering
- Start with an educated guess for number of bursts
    - Some percentage of the number of spikes
- Evenly space the cluster starting centers across the entire recording
- Minimize the distance of spikes between all cluster centers
- Try different cluster numbers or use a method that does not assume cluster numbers 
    - DBSCAN?

## FFT
- Spectrogram to find moments of high power across the signal, each one is considered a burst

## Existing Algorithms

### The “rank surprise” statistic

# Bursting Literature
## [Burst detection methods](https://arxiv.org/pdf/1802.01287.pdf)
- In rodents for spontaneous activity 
    - bursting emerges after 1 week in vitro
    - peaks around 3 weeks in vitro
        - Corresponds to peak synaptic density
    - Then enters a period of shortened network bursts indicating pruning
        - associated with maturation
- Bursts have a higher signal-to-noise ratio

### Fixed threshold-based methods
- minimum firing rate
- max ISI (inter-spike interval)
- min interval between bursts
### Adaptive threshold-based methods
### Other Methods
- examine the slope of the plot of spike time against spike number to detect bursts as periods of high instantaneous slope
- Separate bursts by intervals of at least 2 std > average within-burst ISI
- Sum of within-burst ISIs are less than ISIs immediately before and after the burst
- HMM

## [A comparison of computational methods for detecting bursts in neuronal spike trains and their application to human stem cell-derived neuronal networks](https://journals.physiology.org/doi/prev/20160420-aop/pdf/10.1152/jn.00093.2016)
- **MaxInterval** and **logISI** have good performance across a variety of bursting conditions

# Active Channels
CDI-CN__E33--PLO__Baseline__S3 (57, 56)
CDI-CN__E33--PLO__Post-Stimulation (B. row)__S3.mcd (47, 45, 56, 58, 57)
CDI-CN__E35--FB__Baseline__S1 (55, 56, 58, 57)
CDI-CN__E35--FB__Baseline__S17 (56, 58)
CDI-CN__E35--FB__PostStim__S1 (57, 58, 56, 55, 38, 45, 46, 47
CDI-CN__E35--PLO__PostStim(TR)__S7 (57, 58, 56, 68, 48, 47)
CDI-CN__E35--PLO__PostStim(TR)__S16 (57, 58, 56, 55, 68, 38, 45, 46, 48, 47)
CDI-CN__E35--PLO__Baseline__S7 (57, 58, 68, 45, 46, 48, 47)
CDI-CN__E35--PLO__Baseline__S16 (57, 58, 56, 68, 38, 45, 46, 48, 47)
CDI-CN__E35--PLO__Baseline__S17 (57, 58, 56, 46, 48, 47)

## Notes
CDI-CN__E35--PLO__Baseline__S16
- See if spikes are different without downsampling
CDI-CN__E35--FB__PostStim__S1
- 57 and 58, a single bursting area or change in baseline? (~175-190)