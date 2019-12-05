Step by step guide fo creating your own point cloud and mesh dataset for training unsupervised machine learning models.

The aim is to get .binvox files which is the most common format used so far for creating machine learning models. More details on .binvox will come later. The .binvox open software allows to transform .ply meshes into .binvox. This tutorial will outline the steps to get .ply meshes from point clouds, which can then be transformed into .binvox files.

## Setup and Downloads
1. We will be using Cloud Compare, an open source software to manipulate the 3D point cloud files. You can download Cloud Compare at http://www.danielgm.net/cc/release/. It is better if you use the stable version.

2. This tutorial will be based off an example for the DURAARK Datasets http://duraark.eu/data-repository/. The files in this dataset are either in IFC format or E57 formats.

I suggest you download and extract all folders and place all .ifc files in one folder (call it IFC) and all E57 (call it E57) files in another.

## File Structure
I suggest you create the following folders to store your data:

* `IFC`: for original .ifc files.
* `E57`: for original .e57 files.
* `OBJ`: for .obj meshes we will create for .ifc files.
* `PCD`: for .pcd point cloud files which are obtained from .e57 files.
* `PLY_IFC`: for .ply meshes that we will convert from .obj meshes (which are obtained from the ifc files).
* `PLY_E57`: for .ply_e

## IFC files
1. Download the IFC zip file and extract all the content in the `IFC` folder.
2. Transform your files to load into CloudCompare:
CloudCompare can load many open point cloud formats (ASCII, LAS, E57, etc.) as well as some manufacturer's formats (DP, Riegl, FARO, etc.). It can also load triangular meshes (OBJ, PLY, STL, FBX, etc.) and a few polylines or polygon formats (SHP, DXF, etc.). Some SfM formats are aslo supported (Bundler, Photoscan PSZ, etc.).

CloudCompare does not accept IFC files. To transform IFC files into another format, you can use this 3D Model file Converter available online: https://www.meshconvert.com.

3. Choose .obj as the output format. 

4. Click start to convert the file. Some might not work and you can ignore them. Download all the converted files and store them into the same place (`OBJ` folder). Unfortunately, it is only possible to get meshes out of .ifc files. I could not find any other software to get point clouds instead.

5. Open the .obj file in CloudCompare: open File -> Open -> Select the .obj file to open.

6. Select the .obj file you just opened and save it as a .ply file in `PLY_IFC` via File -> Save, then select file type as `PLY mesh (*.ply)`.

7. You will get a prompt tp choose a formtat for the file: Choose ASCII.

Note: I tried other converters that convert the .ifc files into .ply meshes immediately but many of conversions were not entirely correct and had misplaced part of the structures.
 
Message me if you find or know another converter for .ifc files. Interesting file conversions would be .pcd, .ply, .obj or .e57 formats.

## E57 Files and Other Formats
E57 files can be loaded in Cloud Compare without connversions. These steps should work with any other format for point clouds accepted by CloudCompare.

We will create subsampled point cloud files, cropped point clouds and meshes from these Files.

0. Download the E57 file and unzip the content of the folder in the `E57` folder.
1. Load the point cloud in CloudCompare via File -> Open.
The point cloud might contain many scans.
For simplicity, we will treat each scan as a seperate structure.

2. Subsample the structure: When you click on a scan, you will find Properties in the bottom left of CloudCompare. Under the cloud section, you will find the number of points in the scan. If this number if higher than 600000 points, it is better to subsample it. This is important for training machine learning models, and loading files for subsequent steps.

To subsample a file, Click Edit -> Subsample. If you cannot select Subsample, it means you did not sleect a scan, but rather a structure with multiple scans.

A prompt for subsampling will pop out: choose the Octree method and subdivision level of 15. If you do not get a less than 600000 points file after the subsampling, try a different subdivision level.

Alternatively, choose the random method and set the remaining points to 600000.

More info on how to subsample here: https://www.cloudcompare.org/doc/wiki/index.php?title=Edit%5CSubsample.

3. Save the subsampled point cloud as a PCD file: Click File -> Save. Choose the PCD folder and select file type as `Point Cloud Library (*.pcd)`.

4. Create a mesh from the point cloud. First make sure you have selected a scan rather than a group of scans.
You can either select the original or subsampled scan.
Click Edit -> Mesh -> Delaunay 2.5D (best fitting plane).
You will get a prompt for the triangulation: choose Max edge length as 1.0.

5. Save the mesh in the mesh folder PLY_E57 via File -> Save, then select file type as `PLY mesh (*.ply)`.
You will get a prompt tp choose a formtat for the file: Choose ASCII.

6. Crop the scan: to augment our dataset with more data points, we crop a structure. It is important to crop the structure in a way that makes sense visually. Whatever part you crop, make sure that part makes sense on its own.

Make sure you are selectingone scan only.

To crop the scan: Edit -> Crop.
The standard 3D box editing dialog appears:

By default, the box is initialized to the bounding-box of all selected entities (this default box can be restored anytime by clicking on the 'Default' button).

The user can define the cropping box in multiple ways:

by defining the center and dimensions of the box [default].
by defining the min corner and the dimensions of the box.
by defining the max corner and the dimensions of the box.

If you are unable to make sense of the measures, I suggest you check `Keep Square` and choose something close to but smaller than the width, for X. The value you choose will replicate everywhere.

Feel free to create many crops.

Mpre details on cropping: http://www.cloudcompare.org/doc/wiki/index.php?title=Crop

7. Repeat part 4 and 5 for every cropped structure.

## Obtaining Binvox files.
More details coming soon. I will take care of creating the binvox files automatically on my machine. 
