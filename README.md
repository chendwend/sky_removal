Sky removal
============================  
Process images to crop out the sky.
## General Information
   In this project, using COCO dataset we process it to create a new dataset in which the sky is cropped.  
   This is done using Facebook's Detectron2 panoptic segmentation model. 

## Installation
   All required packages to be installed are in 'Installations' cell. 


## Project Structure
    .
    Sightbit_exercise.ipynb
    └──  data                   # datasets folder
### Data Folder Structure
    .
    ├── ...
    ├── data                    # datasets folder
    │   ├── original_datasets   # original COCO datasets 
    │   └── processed_datasets  # processed COCO datasets
    └── ...
    
## Usage
1. Make sure to change device to GPU: *Runtime* $\rightarrow$ *Change Runtime Type* $\rightarrow$ GPU.  
The notebook will run even if GPU isn't available, but it will take longer.
2. Choose COCO dataset under 'Choose dataset' cell. format shoud be '{train/val}{year}'.
See available datasets at https://cocodataset.org/#download.
3. At the notebook menu bar, click: *Runtime* $\rightarrow$ *Run all*. This will execute the whole notebook.

## Implementation
### General Idea
for each image in the dataset, we identify if sky pixels exist.  
If yes, we verify that the maximal row of pixel with sky is not the whole image, and if so we crop the image.   
If no, we leave the image as it is. 

### Detailed Implementation
1. Create the directories as described above and download the dataset.
2. Load the panoptic segementation model and object detection model using *load_model* function.
3. Identify and store the sky category id from the Metadata of detectron2 model, which is part of the 'stuff_classes' categories.
4. For each image in the dataset:
    1.  load the image
    2.  get panoptic segmentation tensor (which containts category id for each pixel) and segments info that contains which categories are present in the image.
        This is acquired via applying the loaded model on the image.
    3.  Identify the corresponding sky segment id for the image (which could be different for each image), or return 0 if not present
    4. if sky segment id is not present in the image, the processed image = original image.
    5. if sky segment id is present:
        1. find highest row index of sky segment id in the image.
        2. if highest row index = number of rows -1, continue to next image, else crop the image from index to 0.
  6. write the cropped image into processed dataset directory 
  7. Plot randomly selected processed images using Detectron2's visualizer, by applying panoptic segmentation model and object detection model.
