# Tests
## Failures
- Job fails during actual analysis execution
    - **Expected Outcome**
        - script continues running
        - job sent to "Rejected" board
        - Email sent to submitter and developer
- Board fails to query Monday API
    - **Expected Outcome**
        - script continues to query same board for N iterations
        - After N consecutive failures, send an email to developer


Hi there! This is an automated message from the processing engine providing a status update.

Board:
NMJ Image Processing

Status:
IN PROGRESS

Copy ATF to server location after each file created, redo functionality permanently enabled, better console logging, 

# TODO 071922
- Return success status and additional details from each analysis

# Overall Flow
## User Flow
1. Define analysis_paths.json
2. Define batch file with command line args
    1. boards
    2. wait time
    3. send email messages
    4. server drives

## Code Flow
Run analysis_submission.py
1. Load command line args
2. Verify that parameters were set to valid state
3. Initialize board connections
    1. Emailing service
    2. Monday.com connection
4. Search for analysis path files

Refactor command line parsing

# Validation 080922
Make sure the main script writes errorlog and raises exception on failure
## Server Room Right
- [x] ATF
    - [x] Loaded
    - [x] Processed
    - [x] Batch file opens
- [x] Cantilever Cardiac
    - [x] Loaded
    - [x] Processed
    - [x] Batch file opens
- [x] Cantilever Skm
    - [x] Loaded
    - [x] Processed
    - [x] Batch file opens
- [x] CardiacMEA
    - [x] Loaded
    - [x] Processed
    - [x] Batch file opens
- [x] neuronalMEA CV
    - [x] Loaded
    - [x] Processed
    - [x] Batch file opens
- [ ] neuronalMEA
    - [ ] Loaded
    - [ ] Processed
    - [ ] Batch file opens
- [x] NMJ Image
    - [x] Loaded
    - [x] Processed
    - [x] Batch file opens

## Server Room Left
- [x] ATF
    - [x] Loaded
    - [x] Processed
    - [x] Batch file opens
- [x] Cantilever Cardiac
    - [x] Loaded
    - [x] Processed
    - [x] Batch file opens
- [x] Cantilever Skm
    - [x] Loaded
    - [x] Processed
    - [x] Batch file opens
- [ ] CardiacMEA (Tensorflow conflict)
    - [ ] Loaded
    - [ ] Processed
    - [ ] Batch file opens
- [x] neuronalMEA CV
    - [x] Loaded
    - [x] Processed
    - [x] Batch file opens
- [ ] neuronalMEA
    - [ ] Loaded
    - [ ] Processed
    - [ ] Batch file opens
- [x] NMJ Image
    - [x] Loaded
    - [x] Processed
    - [x] Batch file opens
Z:\~1010.001 Drug Induced Dementia (Tabula Rasa)\Intan Testing\DID B7\DID Intan Testing B7
## UCF
- [x] ATF
    - [x] Loaded
    - [x] Processed
    - [x] Batch file opens
- [x] Cantilever Cardiac
    - [x] Loaded
    - [x] Processed
    - [x] Batch file opens
- [x] Cantilever Skm
    - [ ] Loaded
    - [ ] Processed
    - [ ] Batch file opens
- [ ] CardiacMEA
    - [ ] Loaded
    - [ ] Processed
    - [ ] Batch file opens
- [ ] neuronalMEA CV
    - [ ] Loaded
    - [ ] Processed
    - [ ] Batch file opens
- [ ] neuronalMEA
    - [ ] Loaded
    - [ ] Processed
    - [ ] Batch file opens
- [ ] NMJ Image
    - [ ] Loaded
    - [ ] Processed
    - [ ] Batch file opens

