#3D Modeling Using Uncalibrated Images

Build a 3D model of any object simply by clicking pictures of them from your smartphone. 

This project aims to build 3D models of everyday objects with Uncalibrated Images.
Meaning: Images from everyday devices (smartphones) whose camera parameters are not known.

The proposed system uses uncalibrated images for three dimensional modelling, unlike most of the existing systems that use calibrated 
images, thereby eliminating the immense burden of image calibration. Our aim is to make a three dimensional model of an object by taking 
its multiple two dimensional images from various angles. The input images are taken with the camera of a smart phone. The various 
transformation parameters of camera are found using the gyro sensor and the accelerometer present in the device.

How it works:

1) Input Images and sensor Data
Initially, multiple images of the desired object are captured from different viewing angles. These images will be used to produce the 
desired 3D model. A user mobile phone consisting of accelerometer and gyro sensor is used for taking images. Every time there is change 
in the gyro sensor and accelerometer sensor, data is process to update the orientation of device and its position. The data obtained is
used to name the corresponding image file and is saved with the name consisting of this data.

2) Projective Matrix Calculation
The sensor data of the device is continuously read and then tagged as soon as an image is captured. The tagged data from sensor are 
written as name of the image. In other words, sensor data are embedded into the image. From the sensor data embedded in the filename 
of our image, we compute the projective matrix by parsing filename. From the gyro sensor data, we get the orientation of device and 
from accelerometer we get the translation parameters. Thus, from these values, we calculate the projective matrix for each image.

3) Feature Detection and Matching
Then the images are loaded in a program in PC. Here SURF detectors are used to identify the features in images. The first two images 
are taken, with the features detected using SURF detector; we then match the features between the images. Then, for better feature 
points detection, we use Lowe’s Ratio Test. Then, we go on to the next pair of consecutive images until all the images are read.

4) Colour Extraction
After we know the position of the feature points in the images, the colour for that point is taken as average from the colour of the 
2D matched points from both images. The colour for each vertex is then stored in a numpy array.

5) Triangulation
With the known matched points and the respective perspective matrix for images we can use triangulation to get 3D points. 
Triangulation is done for 2 images at a time and SVD algorithm is used to generate depth value from matched key points with known 
x and y coordinates. After getting the required 3D points or vertices, and the corresponding colour of the vertices, we can write 
it to PCD file. We can visualize this point clouds using PCL viewer. The PCD file is made so as to assist in creating the surface 
from point clouds. The PCD file is easy to use with PCL library as well.

6) Surface Development using PCL
If all the images are used for the steps mentioned above, the point clouds are imported into our program using PCL library. 
The point clouds are used to create triangular meshes which are ideal for creating 3D objects. 3D objects in computer graphics 
are described by their vertex table and polygon table with additional necessary information. Thus, by using Ball-Pivoting Algorithm
we create a triangular mesh which is used as polygon table. The mesh is stored.

7) 3D Model
The PLY or OFF file represents a 3D model. This file can be opened with Meshlab and with use of Meshlab we can export the file into 
required format of our choice. Thus we get a 3D model that can be used with popular software like Blender, 3ds Max or SketchUp. 
In this way we get 3D model of an object with sequence of images of the object separated by some angle.
