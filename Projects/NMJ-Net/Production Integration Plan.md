- Monday form for generating predictions
    - Will run on NMJ analysis computer
    - can predict multiple chambers at once
        - because computer has lots of resources
    - Generates ROIs in chamber folder that can be loaded into NMJ-Imaging
- NMJ-Imaging will load and display predicted ROIs that haven't been used before
    - displayed differently from manually generated ROIs
    - cannot be moved, only deleted
    - once a prediction file has been loaded, it will not be loaded again into the gui on subsequent loads
        - first load changes the file name to mark it has been used
- DB log to track how much predictions are being used
    - Chamber folder path
    - timestamp
    - \\# pred ROIs are used
    - \\# pred ROIs there were originally
    - \\# ROIs were generated
- Chambers that have a low pred ROI to manual ROI rate should be added to the training set after labeling
    - Assumption that low pred hit rate means poor model performance
    - Balance with total ROIs selected for the chamber
    - Engineer manually checks to identify which chambers should be added
- 
# Stages of Integration
1. Generate ROIs to display in NMJ-Imaging (3 weeks)
    - Identify ROI locations from detectron2 mask predictions
2. Determine ROI quality in the detection pipeline
    - Cull ROIs that do not have strong contractions
    - Add contraction and tetanus identification
        - Use ML approach
        - Consider gradient as well as subtraction
    - Develop concurrently with step 1 testing
3. Only require GUI analysis for chambers that do not meet quality criteria
    - do some additional selection for adding to training set
    - 