# -*- coding: utf-8 -*-
"""
Created on Tue Nov 24 15:23:31 2020

@author: Amir Nakar
Title: Assignment 1: Nonlinear filters
"""

 #### Task 1: Filtering Cosmic Ray Events ####
"""
In nearly all measurement modalities you will eventually find yourself 
dealing with so called "Hot Pixels". 
Even though there might be a plethora of reasons on how they occur 
(and why they are not already cleaned by the sensor-internal-processing) 
we will assume now to have a measurement which suffers from the influence of
 "Cosmic Rays". In this task, we will first simulate such rays, then remove 
 them and finally test the limitations of our applied technique.
"""
#%% Task 1 
"""
# 1a) Write a function that simulates the Cosmic Rays.
# This function should receive an image and introduce NumShots “hot pixels" at random locations. Thus the functions has the parameters 𝜔𝑠𝑖𝑚=(𝑛ℎ𝑜𝑡𝑃𝑖𝑥,𝐴ℎ𝑜𝑡𝑃𝑖𝑥) for number 𝑛ℎ𝑜𝑡𝑃𝑖𝑥 and value 𝐴ℎ𝑜𝑡𝑃𝑖𝑥 of the hot pixels.

# Hints:
# np.random.randint?
# np.copy?
# np.flat?
"""


#%%
# imports 
import numpy as np
import NanoImagingPack as nip
import copy
from scipy.ndimage import median_filter
%gui qt
# get_ipython().run_line_magic('gui', 'qt')

#%%
# function definitions 
def CosmicRaySim(im,n_hot=500,A_hot=255):
    '''
    Function that generates Cosmic Rays (=Hot Pixels). 

    :PARAMS:
    ========
    :im:        (IMAGE) input image
    :n_hot: (INT)   number of hot shot-pixels
    :A_hot: (INT)   value of hot-shot pixels

    :OUTPUTS:
    =========
    :im_dirty:  (IMAGE) noisy image

    :EXAMPLE:
    =========
    im = nip.readim('orka')
    im_dirty = CosmicRaySim(im,n_hot=250,A_hot=210)
    nip.vv(im_dirty)
    '''
    #your code goes here
    imDirty = copy.deepcopy(im)         # Locks the original image in place
    
    indices = np.random.choice(            # Chooses random indices in the array
        im.shape[1]*im.shape[0],            # Defines the array size
        replace = False, size = int(n_hot)) # Defines how many random indices to take
    
    imDirty[np.unravel_index(indices, im.shape)] = A_hot #Replaces the indexed pixels with the "hot" value
    return imDirty                          #finish the function by returning the output

# %% Test 
im = nip.readim('orka') # loads up a smaple image
im                      # Displays the image
im_dirty = CosmicRaySim(im) #Creates a new spiked image with default settings
#im_dirty = CosmicRaySim(im, n_hot=5000, A_hot=120) #Creates a spiked image

im_dirty                #Displays the spiked image
nip.vv(nip.catE((im,im_dirty)))  # Views both images in napari

### Save the files ###
import matplotlib

path = "C:/Users/di58lag/Desktop/PhD/Courses/WiSe 2021 - Image Analysis/Tasks/Task 1"
matplotlib.image.imsave('im_original.png', im)
matplotlib.image.imsave('im_spiked.png', im_dirty)



#%% Task 2

"""
1c) Remove Shot Events.
The “hot pixels" and the threshold Value need to be identified first
 and then removed. Hence, we now have the following set of tuneable
 parameters:  𝜔𝑓𝑖𝑙𝑡𝑒𝑟=(𝐴𝑡ℎ𝑟𝑒𝑠ℎ,𝑟𝑘𝑒𝑟𝑛𝑒𝑙)  for the threshold value  𝐴𝑡ℎ𝑟𝑒𝑠ℎ 
 and the kernelsize  𝑟𝑘𝑒𝑟𝑛𝑒𝑙 . The input image should not be changed in 
 the output except at the “hot” pixel location!

Hints:


    mymask=anImage > threshValue
    sp.ndimage.median_filter?
    sp.signal.medfilt?
    
"""

import scipy as sp

from scipy.signal import medfilt
from scipy.ndimage import median_filter

#%% Function
def RemoveHotPixels(im_dirty, A_thresh=200, r_kernel=3):
    '''
    Removes the "hot pixels" above a thresh_val 
    within a kernel of size kernel_size.

    :PARAMS:
    ========
    :im:         (IMAGE) input image
    :A_thresh:   (INT)   threshold_value
    :r_kernel:   (INT)   size of thresholding kernel

    :OUTPUTS:
    =========
    :im_clean:  cleaned image

    :EXAMPLE:
    =========
    im = nip.readim('orka')
    im_dirty = CosmicRaySim(im,n_hot=250,A_hot=210)
    im_cleaned = RemoveHotPixels(im, A_thresh=190, r_kernel=4)
    nip.vv(nip.catE((im,im_dirty,im_cleaned))) 
    '''
      
    # your code goes here
    im_dirty = copy.deepcopy(im_dirty)         # Locks the original image in place
    # Step 1: identify the spikes

    im_clean = sp.signal.medfilt(im_dirty, kernel_size = r_kernel)
    return nip.image(im_clean)
 
    
 #%%  Testing
im_cleaned = RemoveHotPixels(im_dirty, A_thresh=190, r_kernel=5)
nip.vv(nip.catE((im,im_dirty,im_cleaned))) 
