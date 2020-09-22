# ResizeRight
The correct way to resize images or tensors. For Numpy or Pytorch (differentiable).
--------

This image-resize funuction is used for image enhancement and restoration challenges, especially super-resolution.   
It is made to address some crucial issues that are not solved by other resizing packages.   
The code is mostly based on MATLAB's imresize function, but with a few crucial extra features. It is specifically useful due to the following reasons:

1. None of the Python packages I am aware of, currently resizes images with results similar to MATLAB's imresize (which is a common benchmark for image resotration tasks, especially super-resolution).

2. You can be accurate and consistent by specifying **both scale-factor and output-size**. This is an important feature for super-resolution and learning because one must acknowledge that the same outpu-size can be resulted with varying scale-factors. best explained by example: say you have an image of size 9x9 and you resize by scale-factor of 0.5. Result size is 5x5. now you resize with scale-factor of 2. you get result sized 10x10. "no big deal" ,you must thinking now, "I can resize it to output-size 9x9", right? but then you will not get the correct scale-fcator which is calculated as output-size / input-size = 1.8.  
This is one of the main reasons for creating this as this consistency is often crucial for learning based tasks.

3. In addition to the common resizing methods (linear, cubic, lanczos etc.), you can specify a numeric array as a resizing kernel and use it to resize the image.

4. You can resize N-D images.

5. Some existing packages return misaligned results. it is visually not apparent but can cause great damage to image enhancement tasks.(https://hackernoon.com/how-tensorflows-tf-image-resize-stole-60-days-of-my-life-aba5eb093f35)

6. You can specify a list of scale-factors to resize each dimension using a different scale-factor.

--------

### Cite / credit
If you find our work useful in your research or publication, please cite this work:
```
@misc{Resizer,
  author = {Assaf Shocher},
  title = {Resizer: Only way to resize},
  year = {2018},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/assafshocher/resizer}},
}
```

--------

### Usage:
```
resizer.imresize(im, scale_factor=None, output_shape=None, kernel=None, antialiasing=True, kernel_shift_flag=False)
```

__im__ :   
the input image

__scale_factor__:    
can be specified as-  
1. one scalar scale - then it will be assumed that you want to resize first two dims with this scale.  
2. a list or array of scales - one for each dimension you want to resize. note: if length of the list is L then first L dims will be rescaled.  
3. not specified - then it will be calculated using output_size. this is not recomended (see advantage 2 in the list above).   

__output_shape__:   
A list or array with same length as im.shape. if not specified, can be calcualated from scale_factor

__kernel__:   
Can be one of the following strings: "cubic", "lanczos2", "lanczos3", "box",  "linear" (or other methods you may add). 
Or a 2D numpy array if the numeric kernel option is wanted.

__antialiasing__:   
This is an option similar to MATLAB's default. only relevant for downscaling. if true it basicly means that the kernel is stretched with 1/scale_factor to prevent aliasing (low-pass filtering)

__kernel_shift_flag__:    
this is an option made to automatically fix misalignment of kernel center of mass, by shifting it so that the resized result is aligned exactly (resizing with any regular method will align it perfectly to input image) (see comment inside the function for further info). the drawback here is that shifting the kenrel uses interpolation which can potentially harm accuracy. however it is sometimes crucial and furthermore this damage is neglegable in any application I tested.
