# Data Upload Utility
#io 
## Problem
- Data upload and download to/from the share drive is slow and can cause both analysis and recording software to hang up while waiting for data transfer. Especially with [[recording software]], slow data transfer can cause data timing to be inconsistent and result in partial data loss. This has been observed in the [[Intan recording software]].
- The organization of files is often poorly defined and changes over time. This makes determining data structure in [[analysis software]] a difficult task with many additional considerations for how the data may be stored.
## Approach
- A script running in the background manages data upload from a predefined folder to the share drive. This occurs independently of the recording process.
- Save data from [[recording software]] to a local folder. This folder may be predefined in the software by removing direct file saving location control from the user and only letting them determine certain aspects of the file name.
### Advantages
- Retains a simple user interaction for normal operation
- Reduce possibility of malformed file structures or typos preventing normal analysis of data
### Drawbacks
- It may be difficult to determine where a recording should be stored on the share drive. This may be managed by letting the user determine the final folder path from the recording software. This information would be saved in a config file along with the recorded data.
- A separate script must be running on a computer to be able to upload data to the [[share drive]]. This could inhibit the availability of data if the script crashes and the users must know to activate the script if it fails for reasons such as the computer restarting.

# Analyze CardiacMEA by Row
**Originally created:** 3/28/22
[[Cardiac MEA Analysis Software]]
#refactor 
## Problem
- Our [[MEA#Custom MEA|Custom MEAs]] are often patterned so that the cells on the two rows of the [[MEA]] are electrically connected, but this is not always the case.
- There has been talk of dosing the MEA rows independently to get two different outputs from a single system.
- The [[Cardiac MEA Analysis Software]] and [[LuminArray]] backend were developed to use the [[LuminArray#System|System]] as the smallest unit of aggregation. 
## Approach
- Having two different discrete outputs per [[LuminArray#System|System]] would require the creation of a new, smaller aggregation layer for an MEA [[LuminArray#Row|Row]]
- Much of the automation logic would need to be moved from the System to the Row subclass
- This would also be a good opportunity to implement the [[#LuminArray API Refactor]]
## Reasons
- This would be a requirement from the biology side, not implementing this would make analysis more difficult as they would need to run the analysis twice. Once analyzing only the top row, and once the bottom row.
## Challenges
- Time consuming
- Adding this now without the refactor will make refactoring more complicated to do later


# [[LuminArray]] API Refactor
**Originally created:** 3/28/22
#refactor 
# Problem
- The [[LuminArray]] package contains all of the various facets of analysis for both the [[Cardiac MEA]] and [[Cantilever]] analyses.
- The API has some standardized naming and organization conventions but they are implemented semi-haphazardly and there is no definitive guide for how code and naming should be devised.
- I would propose splitting the LuminArray package into multiple smaller packages that can be updated independently of one another. 
- This could make implementing portions of the codebase into other projects much easier without having to totally rewrite them to conform to the LuminArray structures. For example, the [[Monday.com]] boards can be used without direct implementation of other aspects of the LuminArray API and should be able to be maintained independently.
## Approach
- Divide LuminArray into multiple packages with independent versioning
### Package Structure
- **LuminArray Signals**
    - Contains API for loading and processing digital signals
    - Includes automation logic which should potentially be split away from the Signal classes and put into their own files
- **Monday Board automations**
    - Logic related to starting analyses from Monday.com boards
    - This should be able to run independently of all other LuminArray modules and can implement non-LuminArray analysis jobs
- **Utilities**
    - General stuff that should be shared across other packages
- **Intan utils**
    - Code that has been modified from Intan systems for loading in code or other possible open source software they release
    - This is independent of other LuminArray packages as it may be used for general purpose or in non-LuminArray code
-  **Analyses** (multiple packages)
    - A package for each analysis software
    - Example: [[Cardiac MEA Analysis Software]], [[Cantilever Analysis Software]], etc.

### Advantages
- Significant improvements to modularizing code
- Utilize code that is currently tied to other modules in the package but doesn't need to be
- Improve ability to update individual pieces of analysis software without having to update everything
### Disadvantages
- Major overhaul of code organization could take a while to implement.
- Reviewing past versions from before the split could be difficult