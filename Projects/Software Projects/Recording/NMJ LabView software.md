# Counting Short Video
5 seconds before, 

# AVI Codecs
```
Path
----
D:\data_store\nmj\image output testing 033022\image_recording\CH 6\18v_PRTCL4_F200_PW50_NP10_LOC1
```
| Index | Time (s) | Size (MB) | Loads in Windows |
| :--- | : ---| :--- | :--- |
| 0 | 26.55 | 604 | TRUE |
| 1 | 12.13 | 87.2 | FALSE |
| 2 | 13.18 | 192 | FALSE |
| 3 | 7.29 | 47 | TRUE |
| 4 | 36.55 | 22.9 | TRUE |
| 5 | 396.85 | 42.5 | TRUE |
| 6 | 5.35 | 403 | FALSE |
| 7 | 7.77 | 604 | TRUE |
Should we switch back to directly saving video?
Should the frame rate be locked at 30?

make warning for opening a file too large

# Desktop Setup
Make exe to launch directly from desktop

# Output cutoff Exploration 4/13/23
- Check INPUT_SIZE in Arduino code and how it compares to the length of the protocols that get uploaded
    - Stored in "Staged Protocol" on LabView side
    - May lead to stop command ("S") getting sent from stimulator to LabView early
        - Sent in Arduino code on line 374
        - Early "S" command would cause red recording inticator to turn off early in LabView

- Number of video timestamps saved and frames in the video match
    - Variable frame rate of video is not directly saved in the output, so videos may appear to be shorter than they actually are
    - check the getframe_16 camera function in labview to verify timestamp accuract


- Increase baud rate
- Control when video files  close within LabView instead of "S" command
- Check stim file saving VI deprecated function