# Questions
## Data Format
- Are there collected channels that do not contain any data?
- Are there any transformations done to the data?
- Are there specific channel combinations that are useful and some that are not?
- Some channels have negative flourescence, what does that mean?
- There are extreme values that have a big time gap either before, after, or both. Are these clumps of cells that pass through together?
- Is the data relative to each run or do we expect different cell types to consistently be within the same flourescent range?
## Goals
- What are we trying to learn that our current tools don't already tell us?
- What are we trying to automate and how time consuming is it?
- Are we trying to do something that commercially available software can't already do?
## Experimental
- Are there other Flow measurements that we are not taking now that would help achieve the above goals? (width measurements)
- How are the analyzed files stored so we can get some ground truth for PCA analysis?

## CytExpert
- what gating strategy is used?
- Are there any hard cutoffs defined for infected population?
- Do we want to consider not using the original gate for looking at the malaria infected cells

# PCA
- First 2 principal components account for >70% of variance
- Populations are not clearly separated. A result of important transforms not being applied?

Select Tetanus and contractions simultaneously
- Remove python-analytics
- X-axis shared across plots
- Simplified handling of tetanus
- Removed reduncancy of plotting calls
- Fixed fidelity output while in tetanus
- Remove contractions that only overlap with tetanus
- Remove tetanus by right clicking over a tetanus region

# Gates
## FSC-H, FSC-A
clockwise starting from top left
Values are x10^4
(41, 65)
(52, 64)
(45, 46)
(30, 23)
(16, 22)
## FITC-A
12000

# Useful comparisons to FITC-A for segmenting parasite life stages
- FITC-Width?
- SSC-A
- FSC-A

Group the same system
try other clustering models
panel plot for all cluster variations

PCA on FSC-H and FSC-A and find peak closest to starting guess

Add color-coded histograms to output

# All Data Plot Questions 110422
Made histograms for all experiments and timepoints grouped by timepoint. Using these plots we want to ask some questions to get a better idea of what our average data looks like.
## Experiments with multiple stages
Z:\~Gates Foundation\Malaria Project 2020\~Phase II\~Data\Milestone 2\Parasites\Flow Data\07.2022
Z:\~Gates Foundation\Malaria Project 2020\~Phase II\~Data\Milestone 2\Parasites\Flow 
Data\08.2022
Z:\~Gates Foundation\Malaria Project 2020\~Phase II\~Data\Milestone 3\Chloroquine\Multiorgan Kill Curves\Parasites\Batch 1 (07.26.22)
Z:\~Gates Foundation\Malaria Project 2020\~Phase II\~Data\Milestone 3\Grouped Compound Batches Flow Files\CQ and LUM final batch (10.6.22_10.13.22)
Z:\~Gates Foundation\Malaria Project 2020\~Phase II\~Data\Milestone 3\Grouped Compound Batches Flow Files\LUM B3_ARTB1 Kill Curve Files
Z:\~Gates Foundation\Malaria Project 2020\~Phase II\~Data\Milestone 3\Grouped Compound Batches Flow Files\CQ Batch 3 LUM Batch 1 Kill Curve Flow Files
Z:\~Gates Foundation\Malaria Project 2020\~Phase II\~Data\Milestone 3\Grouped Compound Batches Flow Files\LUM Batch 2_ART 2.4_CQ 1 100 Kill Curve Flow Files

Milestone 3:
- 01-LUM

## Questions
- What is the experimental protocol?
    - When are more RBC added?
    - Why does 4HR tend to dip and come back up at 8 and 12 HR
- How to interpret filenames
    - W2, RBC, NDC, 3D7
- Ring stage peak shifts over time
- Could peak width/height by useful?
## Especially interesting plots
Two clear populations ![[__07.2022__07.11.22__Exp_20220711_Parasite Quantification__mal_hist_panelExp_20220711_Parasite Quantification.png]]
Ring stage peak is shifted![[__07.2022__07.17.22__Exp_20220717_1_parasite quantification__mal_hist_panelExp_20220717_1_parasite quantification.png]]
Flat other secondary populations![[__07.2022__07.25.22__Exp_20220725_1_parasite quant__mal_hist_panelExp_20220725_1_parasite quant 1.png]]
Mix of secondary population present and not present![[__Artesunate__Multiorgan Kill Curves__Parasites__Batch 2 (09.13.22_09.20.22)__ART Treatment Batch 2 Flow Files__Exp_20220914_D1 Art Quant__mal_hist_panelExp_20220914_D1 Art Quant.png]]
Ring stage peaks at different points in different batches![[__Chloroquine__Multiorgan Kill Curves__Parasites__Batch 1 (07.26.22)__Exp_20220731_1_systems+flask quant__mal_hist_panelExp_20220731_1_systems+flask quant 1.png]]
![[__Chloroquine__Multiorgan Kill Curves__Parasites__Batch 2 (08.10.22)__Exp_20220810_0HR CQ System Quantification__mal_hist_panelExp_20220810_0HR CQ System Quantification.png]]


## Ring stage peak shifts across different days in the same system
![[Figure_1.png]]
# Samples with multiple phases
## 07.2022
01-F

# Meeting Notes 111022
- Add RBC on D4
- Collect sample then add RBC so see on D5
- We don't sync our culture so the parasites are not all at the same stage at the same time
- Replication every 48 hours
- We don't know why the 4HR timepoint is weird
- 01-F2 would be flask
- 