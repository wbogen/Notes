### Load/Display MEA and Cantilever recordings

#### Load
- Choose a folder/file from which to make Signals
- Tree which displays the all of the location information for the Signals selected
    - Hierarchically sorted with the structure filt -> port-system -> electrodes
    - Channels selected to be displayed using checkboxes
- Data is not loaded until specific channels are selected to be displayed and display button is clicked
- Filters are applied if already defined
#### Display
- Each system is rendered in its own column
- 

### Filter Recordings
- Select individual filters to apply from Signals
    - Individually define which filter parameters can be user selected
- Load a filter preset that renders a series of filters with preset parameter values
- Order in which filters are rendered matches the order in which they are applied
- Option to remove a selected filter from the list without having to remove all filters

### Event Selection
- Existing events are added at Signal load time
    - Mostly just stim events
- Direct selection
    - Toggle between multiple event types
    - Snap to specific range after selection
- Load from external source
- Assign unique color to each event type
- Displayed as a different color overlayed on the Signal
    - Minimized the number of additional items that need to be rendered onto the plot
    - May experiment with using LinearRegionItems instead if not 
- Whenever events are updated, save state of the controller using MEAController.export_signal_states()
    - May need to do this in a separate thread for large controllers

### Parameter Tree
