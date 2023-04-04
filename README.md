# fire-detection-yolov4tiny

# Procedure

## 1) Creating Folders
Create a folder named 'yolov4tiny' in Google Drive 

Create a folder named 'training' inside 'yolov4tiny' folder

## 2) Clone darknet git repository

## 3) Label your dataset
We will be using around 1000 images of fire and annotating and labelling it using LabelImg tool. 
The images have been collected from various Kaggle datasets and by taking frames from Youtube videos.

Install LabelImg by 'pip install labelimg' in your local system and annotate the images in yolo format. 

While annotating, make sure the bounding box is exactly capturing the object you want to detect.

![annot eg](https://user-images.githubusercontent.com/85955796/229371769-d7908b13-1110-4d79-aca1-b5900657d973.png)

This creates a folder with the images and their respective annotations saved in a .txt file.

![annotate folder](https://user-images.githubusercontent.com/85955796/229371845-189a1d9f-15a7-437a-877d-8c51c1c8d0ba.png)

Name this folder as obj and zip it.

## 4) Create obj.names and obj.data
obj.names contains the names of classes of objects you are detecting. For this case, we have only one class 'fire'.

obj.data contains information such as number of classes, 1 in our case, paths to obj.names file and path to the folder in which we will be creating our training and testing folders from the given labelled image dataset.

We will also give a path to where our model weights will get saved every 100 iterations, in case of runtime disconnection so we can continue from the most recently saved iteration.

These files are added to the [Github](https://github.com/akshayravi13/fire-detection-yolov4tiny) repository for your reference.

## 5) YOLOv4 Tiny Custom configuration file.
Download the 'yolov4-tiny-custom.cfg' from the 'cfg' folder inside the darknet repository which we cloned. 

This file helps us to configure certain attributes of the YOLOv4 Tiny architecture to suit our custom object classification requirement.

Make the following changes:

1) Change the number of divisions = 64, subdivisions = 16.

2) Height and width to 416.

3) Max batches=6000, steps = 4800, 5400

4) Change the number of filters in the convolutional layers before the 2 yolo layers in the cfg file to 18  = (classes + 5) x 3, in our case, classes=1.

## 6) process.py file 

(To divide all image files into 2 parts. 90% for train and 10% for test)

This process.py script creates the files train.txt & test.txt where the train.txt file has paths to 90% of the images and test.txt has paths to 10% of the images. 

The file has been uploaded to the [Github](https://github.com/akshayravi13/fire-detection-yolov4tiny) repository.

## 7) Adding files to Drive
Add the obj.zip, obj.data, obj.names, process.py and yolov4-tiny-custom.cfg files to the yolov4tiny folder

## 8) Enable GPU and OPENCV

## 9) Run make command to build darknet 

## 10) Copy content into darknet folder
Clear the data folder in darknet but keep the labels folder in it intact as it will be needed for labelling predictions on images.

Unzip obj.zip to data folder

Copy process.py file to darknet folder and run it.

Clear the cfg folder in darknet and add the yolov4-tiny-custom.cfg file to it.

Copy the process.py file to darknet folder and run the file. This will create the test.txt and train.txt files in the darknet/data folder which will be used for training our model.

Get the yolov4 tiny weights from github so that it can be used as initial weights for training

## 11) Train the custom object detector
The process takes around 2 hours, with average loss decreasing from 200 to 0.3. 
In case of runtime disconnection use the code to get the backup weights stored in training folder every 100 iterations

## 12) Test the custom object detector
Check map for the saved weights at every 1000th iterations.

5000th iteration gives the best MAP (35.58%), so we will use these weights

We test the model on test image, giving the below result.

![predictions](https://user-images.githubusercontent.com/85955796/229372358-330c6cca-c084-4bda-a06c-8306af998e5e.jpg)


We test the model on test video, giving the below result.

https://user-images.githubusercontent.com/85955796/229372774-c19702b7-f6ed-42b6-8eec-fd2fe407b980.mp4

