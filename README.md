# SCRIPTS TO GENERATE SATELLITAL IMAGE DATASETS.

In order to get access to the Sentinel-Hub images, it is necessary to create an account on the official site https://www.sentinel-hub.com/ .

Log in and go to `Configuration Utility` section. Click on `+ New configuration` option and then put a `Configuration name` (whatever you want), then go to `Create configuration based on:`, go to ---Select from template instances--- and then select `Python scripts template`. Finally, click on `Create configuration` button. Now we can see our configuration and its information. The most important here is get the id of the instance that is a combination of letters and numbers. Ex `82f77578-eb9a-4909-9a28-30f5279ba6a3`. This id should be put on the scripts to get access to Sentinel-Hub server. (Each script has a section where we should put the id).

## Repository Structure
```
📦satellital_images
 ┣ 📂LANDSAT8
 ┃ ┣ 📜LANDSAT8.ipynb
 ┃ ┗ 📜LANDSAT8_CROPS.ipynb
 ┣ 📂SENTINEL_L1C
 ┃ ┣ 📜SENTINEL_L1C.ipynb
 ┃ ┗ 📜SENTINEL_L1C_CROPS.ipynb
 ┣ 📂SENTINEL_L2A
 ┃ ┣ 📜SENTINEL_L2A.ipynb
 ┃ ┗ 📜SENTINEL_L2A_CROPS.ipynb
 ┣ 📜README.md
 ┗ 📜vegetation_indices .ipynb
```

As we can see, there are 3 different folders, one per Satellite. Inside each folder there are two `.ipynb` scripts:
 1. The first one is in charge of download the satellite images and change the name of each folder and image. The format to rename the folder is as follows: `campo<number_of_field>_<Stellite_name>_<year> - <mont> - <day>`. For the image, the format is the following: `campo<number_of_field>_<year> - <mont> - <day>`. 

    - The number of the field can be 1 or 2.
    - The Satellite name can be `SentinelL1C, SentinelL2A or Landsat8`.
    - The date is gotten from the `.json` which are dowloaded with the images. 
2. The second one let us to get an image per channel, save them, then make the crops of the areas of interest and save them too. 

In both scripts, the rooth path should be set to `../test_dir/<Satellite_name>`. Inside `test_dir` folder, should be 3 folders: `LANDSAT8, SENTINEL_L1C and SENTINEL_L2A`.

**Note**: It is necessary to empty these folders before begin with a different request to avoid conflicts. 

In addition, there is one more file: `vegetation_indices .ipynb`. This script is used to generate the vegetation indices and their crops. Also, one section if focused to get campo2 per quarter. The rootPah is set to `../resultados/<folder>` where we have the satellite images and their channels.

It is important to know that after generating the datasets of the satellite images and their channels, we should take all the folders inside each satellite folder and move them to a folder inside `resultados` folder. Then we can execute the `vegetation_indices .ipynb` script. 
**Note 1**: It is necessary to use different folders to put the data when we want to generate new vegetation indices to avoid conflicts.
**Note 2**: Currently, vegetation indices are only able to work with SentinelL1C images. Lansat8 vegetation indices can be extracted in a similar way depending on the bands that it has. Finally, we can not extract vegetation indices from SentinelL2A because these images only have 3 channels.

Additional information about Sentinel-Hub for Python package can be found on this **[link](https://sentinelhub-py.readthedocs.io/en/latest/)**.
