https://github.com/HesperosDev/HPLC_analysis.git

# Current Workflow

- Receive Monday form submission
    - < Link to HPLC-MS Sample Requests board>
- Experimental plan delivered from requesting team
- Pass plan to batch generator script (written by Thai and Kevin)
    - Need to format experimental plan to be compatible with script
    - `run_batch-generator.bat`
- Manual upload to LCMS software
- run batches from software
    - generates raw file for each injection (multiple per file)
- Open back up in software to examine and generate report from template included in software
- Run report through analysis script
    - `run_analysis.bat`
    - Makes additions and summaries to provided report
    - Includes graphs
- Experiment specific additional calculations


# Proposed Workflow
