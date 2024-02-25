# Video Augmentation for Machine Learning Datasets

This project contains Python scripts for augmenting video datasets by a factor of 10 to enhance machine learning model training. Each video undergoes augmentation with varying illumination factors using gamma correction techniques.

## Table of Contents
1. [Introduction](#video-augmentation-for-machine-learning-datasets)
   - [Limitations](#limitations)
   - [Future Work Ideas](#future-work-ideas)

2. [Main Script](#main-script)

3. [Troubleshooting](#troubleshooting)
   - [Visualize the Contents of a Folder](#visualize-the-contents-of-a-folder)
   - [Incomplete Augmentation](#if-datasetfolder-augmentation-is-incomplete)
   - [Check for Correct Augmentation](#check-for-correct-augmentation)
   - [Check for Rotation](#check-for-rotation)

4. [Correction Script](#correction-script)

### Limitations
These scripts are designed for implementation on Google Colab, utilizing Google Drive as a storage unit. They are thus constrained by the capacities of these non-local environments and by dataset sizes.

When executed, the project successfully augmented dataset folders weighing at least 1GB at about 45s/MB. However, certain execution issues arose when attempting to augment dataset folders larger than 2GB.

### Future Work Ideas
- Apply other techniques like noise and color changes for more video variations.
- Incorporate parallel programming libraries and techniques to augment datasets more efficiently.
- Implement these video augmentation scripts in a local environment.

## Main Script
For the [video augmentation script](scripts/video_augmentation.ipynb), follow these steps:

1. Connect to Google Drive
2. Define **rootPath** as the directory of the root folder containing dataset folders.
   - Example: *`rootPath = '/content/gdrive/MyDrive/RootFolder'`*
3. Define **savePath** as the name of the new root folder to save augmented dataset folders.
   - Example: *`savePath = '/content/gdrive/MyDrive/RootFolderAugmented'`*
4. Define **datasetFolder** as the dataset folder to multiply.
   - Example: *`datasetFolder = 'Pushups'`*
5. Execute the MAIN SCRIPT and wait for completion.
6. [Check](#check-for-correct-augmentation) if all original videos in the dataset folder were multiplied by 10.
7. [Check](#check-for-rotation) to ensure that none of the augmented videos were rotated. If rotation is an issue, refer to the [correction script](#correction-script) to fix this.
8. Repeat steps 4-7 to augment another dataset folder by a factor of 10.

*Note:* We assume that a dataset folder contains multiple MP4 videos recorded vertically.

### General Suggestions
* Make sure you have editor access to the folder you want to augment on Google Drive.
* The Google Colab environment may sometimes disconnect from your Drive. If an unexplainable error is produced, try reconnecting to your Drive to fix it.
* After augmenting all of the necessary dataset folders, erase the temporary folder called *save_frames* directly from the Drive.




## Troubleshooting
Considering that video augmentation is a lengthy process, it may take a while for Google Drive to show you the changes being made in real time, especially when dealing with large datasets.
This can be inconvenient when wanting to compare their true contents in order to verify whether or not the augmentation was a success. Therefore, we recommended using the following commands and functions to get an idea of the contents inside your augmented folders.

### Visualize the Contents of a Folder:

- List the entire contents inside a folder with their sizes.
```
! du -h -a --max-depth=1 /content/gdrive/MyDrive/RootFolder
```

- Show the size of a single file or folder.
```
! du -sh /content/gdrive/MyDrive/RootFolderAugmented/Pushups
```

- List out all of the dataset folders in both the root folder and the augmented root folder.
```
listWords()
```

### If *datasetFolder* Augmentation Is Incomplete:
- (Simple solution):
Use this function if you want to delete an entire augmented dataset folder, afterwhich you should restart the augmenting process for that folder using the main script.
```
borrarCarpeta('Pushup')
```
- (Faster solution): 
Instead of deleting the entire augmented dataset folder, identify the original videos from that folder that didn't correctly augment x10, copy them to a new temporary folder, augment that temporary folder using the main script, and add those augmented videos to the original augmented dataset folder.

### Check for Correct Augmentation:
- This checks the number of videos in the original dataset folder.
```
! ls /content/gdrive/MyDrive/RootFolder/Pushups | wc -l
```

- This checks the number of videos in the augmented dataset folder. This result should be 10 times greater than the previous.
```
! ls /content/gdrive/MyDrive/RootFolderAugmented/Pushups | wc -l
```

### Check for Rotation:
In some cases, due to recording issues not yet fully understood, some augmented videos are rotated 90 degrees. This will be clearly visible upon reviewing the resulting augmented videos.




## Correction Script
For the [video correction script](scripts/video_correction.ipynb), follow these steps:
1. Connect to Google Drive
2. Define **Path** as the directory of the augmented folder containing videos to rotate.
3. Define **Degree** as the degree value to rotate the videos.
4. Choose the cell block that best applies to your specific situation. For this, we provide code for three possible scenarios.

Cell Block 1:
   - This will take a list of videos and rotate all of the videos within that list. This is useful when the number of rotated videos are relatively low.

Cell Block 2:
   - This will rotate all of the videos in an augmented dataset folder (aside from the original copy). This is useful when all of the augmented videos were produced rotated.

Cell Block 3:
   - This will iterate through all of the augmented videos in a folder, find the videos where the horizontal dimension is greater than the vertical dimension, and proceed to rotate those. This can be useful in some extreme cases where the total number of augmented videos are in the hundreds with about half of those needing to be rotated.
   - *WARNING:* This method currently has an issue where the dimensions extracted from the videos aren't always the correct ones, meaning that videos might be mistakenly categorized as needing or not needing to be rotated. Due to this level of unpredictability, it's better to refer to the previous cell blocks for rotating videos. 
