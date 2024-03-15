# Image compression with K-means

In this Project, I applied K-means to image compression. 

* In a straightforward 24-bit color representation of an image, each pixel is represented as three 8-bit unsigned integers (ranging from 0 to 255) that specify the red, green, blue, and Alpha intensity values. This encoding is often referred to as the RGBA encoding.
* The image contains thousands of colors, and I reduced the number of colors to 15 colors.
* By making this reduction, it is possible to represent (compress) the photo in an efficient way. 
* Specifically, I only need to store the RGBA values of the 15 selected colors, and for each pixel in the image you now need to only store the index of the color at that location (where only 4 bits are necessary to represent 15 possibilities).

For the first part, I used the K-means algorithm to select the 15 colors that would be used to represent the compressed image.
* Concretely, I treated every pixel in the original image as a data example and use the K-means algorithm to find the 16 colors that best group (cluster) the pixels in the 4- 4-dimensional RGBA space.
* Once I have computed the cluster centroids on the image, I then used the 15 colors to replace the pixels in the original image.

The provided photo used in this exercise is generated by Gemini.

## Dataset

### Load image

First, I used `matplotlib` to read the original image.

As you can see, this creates a four-dimensional matrix `original_img` where 
* the first two indices identify a pixel position, and
* the third index represents red, green, blue, and Alpha. 

### Processing data

To call the `run_kMeans`, I transformed the matrix `original_img` into a two-dimensional matrix.

After finding the top $K=15$ colors to represent the image, I could assign each pixel position to its closest centroid using the `find_closest_centroids` function. 
* This allowed me to represent the original image using the centroid assignments of each pixel. 
* Notice that I have significantly reduced the number of bits that are required to describe the image. 
    * The original image required 32 bits (i.e. 8 bits x 4 channels in RGB encoding) for each one of the $128\times128$ pixel locations, resulting in total size of $128 \times 128 \times 32 = 524,288$ bits. 
    * The new representation requires some overhead storage in form of a dictionary of 15 colors, each of which require 32 bits, but the image itself then only requires 4 bits per pixel location. 
    * The final number of bits used is therefore $15 \times 32 + 128 \times 128 \times 4 = 66,016$ bits, which corresponds to compressing the original image by about a factor of 6.
