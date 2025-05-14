# Supported Cell/Data Types
- [[Cardiac]]
- [[Skeletal Muscle|Skm]]

# Output Aggregation
As of 3/3/22
## Amplitude
- Get the [[Amplitude|amplitude]] for each contraction on a [[Cantilever|cantilever]]
- Average contraction [[Amplitude|amps]] together to get a single average value for a [[Cantilever|cantilever]]
- Add identifying information about the [[Cantilever|cantilever]] to the output value
- Do the above for every [[Cantilever]] individually and pass the results to a function for aggregation
    - Normalizing the values can be done at this point before averaging together the values from the [[Cantilever|cantilevers]] 
- Multiple levels of aggregation are created here for outputting to Excel file
    - the raw information for each [[Cantilever]] (unaggregated)
    - Aggregated each system, every time point remains a unique row
    - Aggregating by system and timepoint, has the same averaged values as previous level of aggregation but the columns are now each timepoint and the rows are an averaged system
## Beat Frequency
Uses the same process as amplitude

## Notes
- [[Cantilever|cantilevers]] with NaN values will be dropped during the aggregation process. This only applies for the timepoint in which the cantilever had missing data. If it has data for other timepoints, those will still be included in the fully aggregated output