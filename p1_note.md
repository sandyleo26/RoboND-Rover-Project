
#### assignment destination is read-only
if second line is removed then it's fine. Probably due to array being set to read-only when
python tries to parallize these 2 operations.

Rover.vision_image[:,:,0] = np.ones_like(image[:,:,0]) - threshed
Rover.vision_image[:,:,0] = image[:,:,0]

https://stackoverflow.com/questions/39554660/np-arrays-being-immutable-assignment-destination-is-read-only
https://stackoverflow.com/questions/13572448/change-values-in-a-numpy-array

#### pyplot
import matplotlib.pyplot as plt
import matplotlib.image as mpimg

call plt.imshow() first then plt.show() to display an image in command line.
If it's binary, then use plt.imshow(image, cmap="gray")


#### 
How does this work in numpy? where above_thresh is binary and has same shape as the zero array color_select and 

    color_select[above_thresh] = 1

#### imshow() needs dtype to be np.uint8

#### good matplotlib, numpy, pandas tutorials
