# Video Augmentation for Machine Learning Datasets

This project contains python scripts that can be used for augmenting video datasets x10 to help feed machine learning models. Each video is augmented by varying illumination factors using gamma correction techniques.

### Limitations
These scripts are meant to be implemented on Google Colab with Google Drive serving as a storage unit and are thus limited by the capacities of these non-local environments and by the dataset sizes. 

At the time of executing this project we were successfully able to augment dataset folders weighing approximately 1GB at about 45s/MB without accounting for the time needed for rotations that some augmented videos required. Certain execution issues came about when attempting to augment dataset folders larger than 2GB.

### Future Work Ideas
- Applying other techniques to add video variations such as noise and color changes.
- Incorporate parallel programming libraries and techniques to increase the amount of datasets that can be augmented within a given time.
- Implement these video augmentation scripts in a local environment.

## Main Script
For the purposes of this [video augmentation](scripts/video_augmentation.ipynb) script, we assume that the user has a root folder containing multiple dataset folders where each dataset folder contains multiple MP4 videos that were recorded vertically.

1. Connect to Google Drive
2. Define **rootPath** with the directory of the root folder containing your collection of dataset folders.
   - Example: *`rootPath = '/content/gdrive/MyDrive/RootFolder'`*
3. Define **savePath** with the name of the new root folder where you want to save your augmented dataset folders.
   - Example: *`savePath = '/content/gdrive/MyDrive/RootFolderAugmented'`*
4. Define **datasetFolder** with the dataset folder containing the set of videos you want to multiply.
   - Example: *`datasetFolder = 'Pushups'`*
5. Execute the MAIN SCRIPT and wait for it to finish executing.
6. [Check](#check-for-correct-augmentation) to make sure that all of the original videos in the rootFolder were multiplied by 10.
7. Repeat steps 4-6 to augment another dataset folder x10.




## Troubleshooting
Considering that video augmentation is a lengthy process that may take many hours, it may take a while for Google Drive to show you the changes being made in real time, especially when dealing with large folders.
This can be inconvenient when wanting to compare their true contents in order to verify whether or not the augmentation was a success. Therefore, we recommended using the following commands and functions to get an idea of what's really inside your augmented folders.

### Visualize the contents of a folder:

- Lists the entire contents inside a folder with their respective sizes.
```
! du -h -a --max-depth=1 /content/gdrive/MyDrive/RootFolder
```

- Shows the size of a single file or folder.
```
! du -sh /content/gdrive/MyDrive/RootFolderAugmented/Pushups
```

- Lists out all of the dataset folders within root folder (i.e. rootPath) and the augmented folder (i.e. savePath) to simplify comparing them.
```
listWords()
```

### In case *datasetFolder* did not completely augment x10:
- (Simple solution):
Use this function if you want to delete an entire augmented dataset folder, afterwhich you should restart the augmenting process for that folder using the main script.
```
aux = 'Pushup'
borrarCarpeta(aux)
```
- (Faster solution): 
Instead of deleting the entire augmented dataset folder, identify the original videos from that folder that didn't correctly augment x10, copy them to a new temporary folder, augment that temporary folder using the main script, and add those augmented videos to the original augmented dataset folder.

### Check for correct augmentation:
- This checks the number of videos in the original dataset folder.
```
! ls /content/gdrive/MyDrive/RootFolder/Pushups | wc -l
```

- This checks the number of videos in the augmented dataset folder. This result should be 10 times greater than the previous.
```
! ls /content/gdrive/MyDrive/RootFolderAugmented/Pushups | wc -l
```



## Correction Script
In some cases, due to recording issues not yet fully understood, some of the resulting augmented videos are rotated 90 degrees. If this is an issue, we provide a [video correction](scripts/video_correction.ipynb) script to help rotate these videos into the correct position.

1. Connect to Google Drive
2. Define **Path** with the directory of the augmented folder containing the videos you wish to rotate.
3. Define **Degree** with the degree value that you wish to rotate the videos.
4. Choose the cell block that best applies to your specific situation. For this, we provide code for three possible scenarios.

Cell Block 1:
   - This will take a list of videos and rotate all of the videos within that list. This is useful when the number of rotated videos are relatively low.

Cell Block 2:
   - This will rotate all of the videos in an augmented dataset folder (aside from the original one). This is useful when all of the augmented videos were produced rotated.

Cell Block 3:
   - This will iterate through all of the augmented videos in a folder, find the videos where the horizontal dimension is greater than the vertical dimension, and proceed to rotate those. This can be useful in some extreme cases where the total number of augmented videos are in the hundreds with about half of those needing to be rotated.
   - *WARNING:* This method currently has an issue where the dimensions extracted from the videos aren't always the correct ones, meaning that videos might be mistaken as needing or not needing to be rotated. Due to this level of unpredictability, it's better to refer to the previous cell blocks for rotating videos. 
