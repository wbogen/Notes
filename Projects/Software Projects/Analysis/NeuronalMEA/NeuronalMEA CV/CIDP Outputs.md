# Plots
## Bar plots
**Peak amplitudes by row
- with error bars
- grouped by conducting from and to

**Half peak width by row
- same as above

## Channel Reports
**spikes by each channel
- number of spikes
- amplitude with error
- half peak width with error
- ``se_hpw = round(np.std(hpws) / np.sqrt(amps.size), 3)``
- separated into both incoming and outgoing conducting spikes

## CV plots
- average cv for each conducting pair of channels
- number of conductions
- conduction rate (transmissions per second)
- for 2 and 3 channel groupings
- include standard deviation?

# Organization
## Raw
**Each [[Neuronal MEA#Spikes|spike]] is a row**
- channel
- timestamp
- [[Amplitude]]
- half peak width
- spike conducting from timestamp
- spike conduction to timestamp

## By Conduction
**Each conduction is a row**
- channel this
- channel next
- channel 3rd
- timestamp this
- timestamp next
- timestamp 3rd
- 2 channel cv
- 3 channel cv

## By Channel Pair Channels
**Each channel pair is a row**
**Consists of all conductions on a channel pair**
- channel from
- channel to
- Average [[Conduction Velocity|CV]]
- average [[Amplitude]]
- Average [[Half-Peak Width]]
- number of spikes
- conduction rate (transmissions per second)

## By 3 Channel Grouping
**Same as [[#By Channel Pair Channels]] but with 3 channels**
