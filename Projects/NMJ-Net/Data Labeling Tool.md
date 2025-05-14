# Main Features
## Instance Segmentation
- Users draw a polygon around a myotube
- Double click to start a new polygon
- Vertices can be modified once placed
- Display a draggable centroid to place over the cell body
    - Bounds limited to inside the polygon
    - Can only be edited once polygon is closed
- Toggleable color in the area of a polygon
- Automatically generate a bounding box based on the polygon and default centroid
    - toggleable display
- Undo functionality (optional)
    - vertex mod/add/remove
    - polygon remove

## Class labeling
- Predefined list of class labels to choose from
- Each polygon can have one label
- Toggle between classes with number keys
    - Specific side toolbar for selecting active class

## Image Display
- Use ImageView
    - Or sub classed version used in NMJ-Imaging
- Display frames from one video at a time
    - Maybe do a whole chamber?
- Have tree structure for easily switching between videos rather than going back and forth with file explorer
- Only show frame from a set default time after each stimulation
- Toggleable subtraction
    - Calculate from first n frames prior to stimulation
    - Display subtraction as a mask for percent brightness of the raw image?
        - Make it a slider for intensity
- Have a label to show stimulation number in the protocol and timestamp

# Output Format
**Segmentation Info**
- COCO https://detectron2.readthedocs.io/en/latest/tutorials/datasets.html?highlight=coco
**Training Images**
- raw frames
- subtracted versions of the same frames

# Tools
- pyqtgraph
- openCV