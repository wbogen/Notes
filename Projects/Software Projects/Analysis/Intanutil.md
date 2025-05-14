- Each note in the Notes section is limited to a length of 4 bytes unsigned int
- What is number_of_signal_groups?
    - Includes data ports (A,B,C,D), analog in and out ports, digital in and out ports


# Protocol File
## Feedback 080922
- Define "Pause" in seconds instead of minutes
- "Total Chained" redundant?
- What are all the "... Index" values in the "Protocols"
- Is giving each protocol a number key instead of having a list redundant?
- Add units to "Pulse Current"
- Is "numTrains" redundant?
- Do each train in "Arduino Timing" need to be key/value or can it be a list

# Read order
Each char after "<" (represents little endian) creates an output value
The number provided is the number of bytes to read that gets parsed into each output variable
![[Pasted image 20220810114719.png]]
![[Pasted image 20220810114733.png]]
https://docs.python.org/3/library/struct.html
## Header
- magic_number = <I, 4 
    - Reads an integer and checks against a hardcoded base 16 hex value for match. We can leverage this to determine certain information about the file and manybe manipulate it to version RHX file types
- Version major, version minor = <hh, 4
- sample rate = <f 4
- dsp_enabled, actual_dsp_cutoff_frequency, actual_lower_bandwidth, actual_lower_settle_bandwidth, actual_upper_bandwidth, desired_dsp_cutoff_frequency, desired_lower_bandwidth, desired_lower_settle_bandwidth, desired_upper_bandwidth = <hffffffff, 34
    - General hardware(?) filter information that isn't really important
- notch_filter_mode = <h, 2
    - Either 50 (1) or 60 (2) Hz filter used, we always do 60Hz in analysis so it doesn't matter
- desired_impedance_test_frequency, actual_impedance_test_frequency = <ff, 8
- amp_settle_mode, charge_recovery_mode = <hh, 4
- stim_step_size, recovery_current_limit, recover_target_voltage = fff, 12
    - No "<" in the byte type string
- note1 = read_qstring_rhs()
    - length, = <I, 4
    - (length/2) unicode characters read as c = <H, 2
- note2 = read_qstring_rhs()
- note3 = read_qstring_rhs()
- dc_amplifier_data_saved, eval_board_mode = <hh, 4
- ref_channel_name = read_qstring_rhs()
- number_of_signal_groups, = <h 2
    - Not directoy saved, used to iterate for the total number of channels from 1 to (number_of_signal_groups + 1)
    - Seems to consistenly have value of 8, maps to the number of times chip_channel in amplifier_channels restarts the iteration (twice per port)
- signal_groups iteration
    - Each channel contains
        - signal_group_name = read_qstring()
        - signal_group_prefix = read_qstring()
        - signal_group_enabled, signal_group_num_channels, signal_group_num_amp_channels = <hhh, 6
            - Skips to next iteration if first values are 0
        - iterate over each channel
            - native_channel_name = read_qstring()
            - custom_channel_name = read_qstring()
            - native_order, custom_order, signal_type, channel_enabled, chip_channel, command_stream, board_stream = <hhhhhhh, 14
                - signal_type communicates what list in the header this channel goes under
            - voltage_trigger_mode, voltage_threshold, digital_trigger_channel, digital_edge_polarity = <hhhh, 8
                - belong to trigger_channel which goes into spike_triggers, doesn't seem important
            - electrode_impedence_magnitude, electode_impendence_phase = <ff, 8
                - Impendence is important, are these numbers accurate for our purposes?

IOBase.tell() gives the current position in the file, can be leveraged to determine where to start reading data

## Data
Each block is 128 samples
The size of the block is determined by the number of channels read in earlier and added to the header dict

- t.append() = <128i, 128 * 4
- amplifier_data.append() = np.uint16, 128 * num_amplifier_channels
    - reshape to 


In 'Mapping', 'Display' each column is connected in a row
Use number before the timestamp in file names coressponds to the stimulation index in 'Protocol', 'Protocols' and 'Channels Displayed' is the mapping by system

# API Core Methods and Attributes
## Signal
### Attributes
#### Base
- sid: unique signal id
- signal: amplifier data
- timestamps: timestep for each amplifier value
- filepath: source file path
- recording_time: when data was recorded (decide on standard format)
- system: grouping within a given timepoint
- label: name for this channel
- dose: experimental condition for this channel
- sample rate
- recording_length: duration of this recording
- filters: Which filters are available
- applied_filters: which filters were applied

#### MEA
- events: container for Event classes
- stimulated: stimulations are present in this data, may not have been directly applied to this channel
- side: which side of a MEA this was added
- order: location in the MEA of this channel
- direct_stim: This channel was directly stimulatied
- stim_points: start, phase change, and end of the stimulation

### Methods
#### Base
- clear: Empty amplifier, stim, and timestamp data from memory
- downsample
- dx_signal: get the derivative of the amplifier data
- filter_signal: apply filters
- fft: get the fft of the amplifier data
- spectral_components: Transformation for spectrogram
- subset: get snippet of data
- reset_to_raw: Reset filtered to raw signal

#### MEA
- make_stim_events: create events for stimulations (internal?)
- plot: make an axes subplot for this data

## Controller
### Attributes
#### Base
##### External
signals: list of objects of Signal class
##### Internal
- batches: list of controller objects of Batch class
- files: list of controller objects of File class
- recordings: list of controller objects of Recording class
- systems: list of controller objects of System class
#### MEA
### Methods
#### Base
##### External
- properties
    - ordered_paths: Sorts filepaths chronologically by signal recording_time
    - paths: Unique file paths
    - sids: list of sids
    - systems: calls get_systems
- add_signals: Change the parent of a Signal to this object, add it to the signals list
- apply: apply a method to all signals
- get: retrieve value from all signals with from provided attr
- get_systems: split loaded Signals into System Controllers
##### Internal
- getitem: indexes on signal attr
- len: number of signals
- repr: name and number of signals
- signal_method: See docstring
#### MEA