## Project: Search and Sample Return

---


**The goals / steps of this project are the following:**  

**Training / Calibration**  

* Download the simulator and take data in "Training Mode"
* Test out the functions in the Jupyter Notebook provided
* Add functions to detect obstacles and samples of interest (golden rocks)
* Fill in the `process_image()` function with the appropriate image processing steps (perspective transform, color threshold etc.) to get from raw images to a map.  The `output_image` you create in this step should demonstrate that your mapping pipeline works.
* Use `moviepy` to process the images in your saved dataset with the `process_image()` function.  Include the video you produce as part of your submission.

**Autonomous Navigation / Mapping**

* Fill in the `perception_step()` function within the `perception.py` script with the appropriate image processing functions to create a map and update `Rover()` data (similar to what you did with `process_image()` in the notebook). 
* Fill in the `decision_step()` function within the `decision.py` script with conditional statements that take into consideration the outputs of the `perception_step()` in deciding how to issue throttle, brake and steering commands. 
* Iterate on your perception and decision function until your rover does a reasonable (need to define metric) job of navigating and mapping.  

[//]: # (Image References)

[image1]: ./misc/rover_image.jpg
[image2]: ./calibration_images/example_grid1.jpg
[image3]: ./calibration_images/example_rock1.jpg 

## [Rubric](https://review.udacity.com/#!/rubrics/916/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it!

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.
Color thresholding and coordinate transformation functions are tested on both given test data and my own training data. Transformed images are embedded in notebook. 

`select_rock()` is added for thresholding yellow color, which represents rock

`do_some_plotting()` is extracted as a helper function to plot intermediate transformed images

#### 1. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 
The output video is embedded in the end of the notebook.

`process_image()` is filled the tranformation functions, following the instruction in the comment. Basically, it does a perspect transformation followed by color thresholding, rover-centric coord transformation and world coords transformation. 

Navigable coords is calculated using the given `color_thresh` function; Obstacle coords is derived by subtracting `255 - navigable * 255`; And rock coords is calculated by first converting RGB to HSV and then do filter out the yellow range as in [`select_rock`](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_colorspaces/py_colorspaces.html)


### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.
`perception_step` is modified basically as in `process_image` and `decision_step` is used as it is. There's a lot that can be improved but I find these changes are enough to pass the minimum requirement.


#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

**Note: running the simulator with different choices of resolution and graphics quality may produce different results, particularly on different machines!  Make a note of your simulator settings (resolution and graphics quality set on launch) and frames per second (FPS output to terminal by `drive_rover.py`) in your writeup when you submit the project so your reviewer can reproduce your results.**

The setting of simulator is:
1. Resolution: 800x600
2. Quality: Good

A typical run in autonomous mode will map 40% in about 2 minutes with fidelity > 70%, identifying at least 2 rocks. Then it'll continue to map more most likely reaching about 70% and fidelity dropping to 60%. In the end, the Rover usually end up driving to already-mapped area or circling around a wide open area (e.g. the open area in far right).

The rover drives quite smoothly most of the time, avoiding obstacles and keeping a distance from walls. When it reaches a dead end, it can stop and turn around successfully. However, since I didn't make any optimization in `decision_step`, it can get into the same dead end multiple times.

Usually at least 3 rocks can be mapped and shown correctly on the bottom right map.

The results above show that the simple implemention of `perception_step` and `decision_step` is good enough to let rover drive reliably and meet the minimum requirements. However, it's not intelligent enough to achieve higher goals nor can it achieve in a more efficent manner. Here're improvement I'd like to make in the future:

1. record track so that to avoid driving in area already mapped
2. try picking up rocks when found
3. improve fidelity by taking pitch and roll into account (they should be close to 0)
