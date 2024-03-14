![image](https://github.com/erickburci/TextureQuilting/assets/159087967/50e1e309-a7e4-4ee4-adbd-a0c4754747a7)

# Texture Quilting

In this project, I use code to stitch together image patches sampled from an input texture in order to synthesize new texture images.

# Part 1 - Shortest Path 

The function ***shortest_path***  takes an 2D array of ***costs***, of shape HxW, as input and finds the *shortest vertical path* from top to bottom through the array. A vertical path is specified by a single horizontal location for each row of the H rows. Locations in successive rows should not differ by more than 1 so that at each step the path either goes straight or moves at most one pixel to the left or right. The cost is the sum of the costs of each entry the path traverses. Your function should return an length H vector that contains the index of the path location (values in the range 0..W-1) for each of the H rows.

I solved the problem by implementing a dynamic programming algorithm. I used a for-loop over the rows of the "cost-to-go" array, computing the cost of the shortest path up to that row using the recursive formula that depends on the costs-to-go for the previous row. Once at the last row, we can then find the smallest total cost. To find the path which actually has this smallest cost, we will need to do  backtracking. The easiest way to do this is to also store the index of whichever minimum was selected at each location. These indices will also be an HxW array. We can then backtrack through these indices, reading out the path.

# Part 2 - Image Stitching

The function ***stitch*** takes two gray-scale images, ***left_image*** and ***right_image*** and a specified ***overlap*** and returns a new output image by stitching them together along a vertical seam where the two images have very similar brightness values. If the input images are of widths ***w1*** and ***w2*** then the stitched result image returned by the function should be of width ***w1+w2-overlap*** and have the same height as the two input images.

We will want to first extract the overlapping strips from the two input images and then compute a cost array given by the absolute value of their difference. I can then use my ***shortest_path*** function to find the seam along which to stitch the images where they differ the least in brightness. Finally we need to generate the output image by using pixels from the left image on the left side of the seam and from the right image on the right side of the seam. 

![Screenshot 2024-03-13 173220](https://github.com/erickburci/TextureQuilting/assets/159087967/3f54bb21-a50f-400b-a77f-2b9901f1764f)

# Part 3 - Texture Quilting

The function ***synth_quilt***  takes as input an array indicating the set of texture tiles to use, an array containing the set of available texture tiles, the ***tilesize*** and ***overlap*** parameters and synthesizes the output texture by stitching together the tiles. ***synth_quilt***  utilizes my stitch function repeatedly. First, for each horizontal row of tiles, construct the stitched row by successively stitching the next tile in the row on to the right side of your row image. Once we have row images for all the rows, we can stitch them together to get the final image. Since my stitch function only works for vertical seams, I have to transpose the rows, stitch them together, and then transpose the result.

# Part 4 - Texture Synthesis Demo

The function ***quilt_demo*** puts together the pieces. It takes a sample texture image and a specified output size and uses the functions implemented previously to synthesize a new texture sample.

## Results

### Cobblestone Pattern
![image](https://github.com/erickburci/TextureQuilting/assets/159087967/d8a94296-a42c-4220-ba37-85ccb0ac2793)

#### Discussion/Analysis
***For each result shown, explain here how it differs visually from the default setting of the parameters and explain why:***

. (1) **Increased Tile Size**
    Increasing the tile size creates a larger synthesized texture image that fits more of the original sample and has smaller, more complete rocks. When tile size is increased we are able to get more detail from the original texture sample. We can see the detail in the form of more natural looking rocks. In the original image we see a lot of elongated rocks which are easier to notice with the smaller overall image size. In the new image, this effect is less noticeable as the rocks appear smaller overall which gives the illusion of more naturally shaped rocks. 


. (2) **Decreased Overlap**
    With decreased overlap there is an increase in visible artifacts (i.e. the seams between the tiles became more visible), causing the overall synthesized texture image to appear more blocky. As a result we see a lot of rocks with odd shapes or rocks that seem to collide and merge with each other in unnatural ways. This is because with less overlap, there are fewer pixels to choose from when finding a seam. Also, less overlap means less opportunity for finding matching pixels between neighboring tiles, hence the appearance of more artifacts. Lastly, with less overlap, more of each original tile is used causing the final synthesized texture image to be slightly larger.


. (3) **Increased K**
    When k was increased, there seemed to be less artifacts (i.e. less little black smudges within the rocks). The individual tiles appear to have blended more seamlessly and there seems to be more variation in the shapes and sizes of the rocks. This is because when k is increased there is a larger set of randomly selected sub-tiles to choose from, which then increases the opportunity of finding better matching candidates when trying to find a tile that has the least sum of squared differences (within the topkmatch function). Having a larger set of random k patches also contributes to more variety in the texture. 

### Random Texture
![image](https://github.com/erickburci/TextureQuilting/assets/159087967/68ceec65-48fe-4233-b6c2-b2a95475cefa)

### Raspberries
![image](https://github.com/erickburci/TextureQuilting/assets/159087967/1a235e1e-2abb-4119-8202-0eecc5c7d848)




