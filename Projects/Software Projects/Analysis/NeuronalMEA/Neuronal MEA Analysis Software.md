# Bug Fixes
## 031422
### Problem

### Notes
#### Template Length
Templates lengths are tied to [[sample rate]]. An sample rate of 25000 will have 250 indices in the template.
```
lid_pos: 498, good_pos: 498, lid_neg: 200, good_neg: 200, spikes: 250
```
Implemented [[Resampling]] to force all templates to be 500 samples which translates to a sample rate of 50000 Hz. This is the maximum rate that we record all data so no recordings will receive down-sampling from this in the foreseeable future. Th
#### PCA features
The number of features is different for positive and negative templates
```
POS n_feats | template: 25, shape: 8, total: 33 
NEG n_feats | template: 29, shape: 8, total: 37
```
This difference is due to the different number of templates that exist for lidocaine traces. Each template contributes 4 features to the total for each time region of interest within the spike. These regions are defined in the code. 

This differences are also indicative that the existing PCA was trained with the negative spikes and an entirely new PCA should be trained for positive spikes on the Intan.


# Git Pushes
## 032322
- Force sample rates to integers
- Hide some numpy empty operation warnings
- Labeling for progress bars
- Additional comments and documenting
- Disable new PCA creation if existing one fails
- Force Spikes in difference index to be the same length
- Minor refactoring for legibility and efficiency
- [[Resampling# Up-sample|Up-sample]] all spikes and templates to 500 indices
- Up sample functions