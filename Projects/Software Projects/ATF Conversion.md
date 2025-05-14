# Description
Takes in [[#rhx]] or [[#mcd]] files and converts them to [[#atf]] files.

Utilizes the [[Monday.com]] API conversion submission and execution.

# Bugs
**3/4/22**
The "Is in use" flag was being activated but then blocks access to the job later in the same instance of the script. This is currently not a problem because only one instance of the script is running at a time.

# Updates
## 061322 HTP Update
### Problem
ATF conversion script accepts up to 10 channels to process from an MCD or RHD/RHS file which is intended to only represent a single system per file. HTP RHD/RHS files can contain up to 16 systems per file which requires a separate submission to the Monday.com board for each system, a manual and repetitive process. Entering each channel number is an error prone process and the channel mapping may not be easily accessible to the user.
The labeling of these file names does not clearly indicate which channels are included in the ATF output or which system is represented by these channels. A separate mapping file needs to be referenced against the channel numbers in the file to know which system is being viewed.
### Goals
- Update the ATF conversion process to support HTP files.
- Refactor the code to use the unified Monday.com processing script
### Inputs
- A folder of RHD/RHS or MCD files that share the same channel mapping
- A single Monday.com submission per folder of RHD/RHS or MCD files needing to be converted
### Outputs
- One ATF file per system from the included RHD/RHS  or MCD files with clear labeling communicating which port and system is represented in the ATF file.
### Changes
- Update ATF conversion script to support processing of HTP RHD/RHS files
- Update Monday.com form to allow inputs for specific channel maps instead of just channel numbers
- Move Monday.com querying implementation to the format that is used by most of the other analysis processes. The only other analysis not using common processing code is the neuronal MEA analysis.

### Cost
- Time to update the code.
    - Development time will be a few days
    - Validation can be done entirely on Engineering side

### Bug Fixing
#### Current Status

#### Runs Report
- [x] Failing does not remove local copies
    - Right now this only works when the data is copied to C:/mea_data_copy
- [ ] Some files seem to have invalid values saved specific to channels
    - Occurs when writing entire file in memory
- When passing a single system channel map, there are still 4 systems created
##### MCD

##### HTP RHD

##### HTP RHS

##### RHS Single System

##### seconds to convert

##### Port selection RHS

##### Port selection RHD

