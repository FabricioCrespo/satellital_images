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

## Overview
### Summary 
The project was oriented to generate a satellite images dataset of two areas of interest: Campo1 with [-71.817933, -35.452880, -71.807166, -35.446550] coordinates and campo2 with [-71.800228, -35.457577, -71.786453, -35.449821] coordinates. The format of the coordinates is as follows: FORMAT(LONGITUDE1, LATITUDE1, LONGITUDE2, LATITUDE2). On Google Earth, they are lower left and upper right corners. The dates for the images should be old as possible to 2020. In addition, it was required to get one image per band (or channel), crop the area of interest according to each camp, and save them. In addition, it was required to construct vegetation indices from the images per channel, make the crop, and save them too. To fulfill the objective, the following activities were performed.
1. Search for satellite image sources and test them.
- A first search was made to know some satellite image sources provided by the article https://mappinggis.com/2015/05/como-descargar-imagenes-landsat/ . The sources were: LandsatLook/Sentinel2Look visors, Landsat viewer, Earth Explorer, Global Imagery Browse Services in EO browser, Planet explorer, Snap and QGIS. In most of the cases, the images provided by them were not HD or they were not available for the area of interest. For this reason, it was necessary to do a second search. Time: 4 days
- As the required images should be as high resolution as possible, a second search was needed. On this occasion, Sentinel-Hub for Python was found, Planet Explorer, and SkyWatch. As the cost of each high definition image was expensive and more information was required, it was necessary to contact the sales department of Planet Explorer and SkyWatch. On Planet Explorer, images for the area of interest were no available. On the other hand, a meeting with the director of Sales Latam was developed to get access to the platform to do some tests. He gave us access to the platform but the images were not optimum. For this reason, we decided to use Sentinel-Hub for Python package. In addition, it was necessary to learn about the satellite bands and their meaning. Time: 4 days.
2. Learning to use Sentinel-Hub for Python package and downloading some images for the test. 
- Sentinel-Hub package let us make a request to a server to get satellite images from an area of interest. As with any package, it has its own functions and variables to make the request. First of all, it was necessary to create an account on the Sentinel-Hub website to get an id. This id should be put at the beginning of the code to be able to access the images. The first test was done by downloading images from December 2019 from SentinelL1C, SentinelL2A, and Landsat8 satellites. The code was executing using the Colab platform. Time: One week.
3. Downloading some images, separate them by bands/channels, and crop them only for the area of interest.
- On this opportunity, it was necessary to separate the satellite images by bands and then save them. As the original images from the Sentinel-Hub server have .tiif format, it was necessary to use the proper libraries. The test was using satellite images from SentinelL1C. Each image from this satellite has thirteen bands. Therefore, we obtain thirteen additional images from the original. In addition, it was required to crop the images only for the area of interest. It was done using the OpenCV library. This activity was oriented to develop the necessary codes to get the objective. It was made with images from SentinelL2A, SentinelL1C, and Landsat 8 images. Time: one week. 
4.Vegetation indices generation and with their respective cropped image.
- Reading papers to know about vegetation indices as to which of them are the most important and their formulas. Time: 2 days.
- Implementation of the code to extract the vegetation indices from the satellite images. It was only possible with SentinelL1C images because the images from SentinelL2 have atmospheric correction; therefore we can only extract 3 channels from these images. On the other hand, the Sentinel-Hub server stops providing Lansat8 images due to the migration of the platform to another server. Time: 4 days.
5. Downloading the complete dataset accompanied by the images per channel, crops, and vegetation indices. Time: 2 weeks.
6. Preparation of the code to crop images of campo2 per quarter. **Time**: 1 day.
7. Correction of the code because we had crossing images between original images and images per channel. **Time**: 2 days.

## Main Resources:

- Information about satellite image sources: [https://drive.google.com/file/d/1MQMlC2ltF4iEFwV7XIJVVqg7R5wI8rzg/view?usp=sharing](https://drive.google.com/file/d/1MQMlC2ltF4iEFwV7XIJVVqg7R5wI8rzg/view?usp=sharing)
- Second search for high resolution satellite image: [https://drive.google.com/file/d/15dpH3kgHSFQ3fkThwMKBwiKmHyp5ShY6/view?usp=sharing](https://drive.google.com/file/d/15dpH3kgHSFQ3fkThwMKBwiKmHyp5ShY6/view?usp=sharing)
- Sentinel2 and Landsat8 information: [https://docs.google.com/document/d/1eAjZtSas6ex2qjm3e2y6uUARrmn6tfeMb76OQR4xCbA/edit?usp=sharing](https://docs.google.com/document/d/1eAjZtSas6ex2qjm3e2y6uUARrmn6tfeMb76OQR4xCbA/edit?usp=sharing)

## Documentation
## Link to the repository:

[https://github.com/FabricioCrespo/satellital_images.git](https://github.com/FabricioCrespo/satellital_images.git)

## Link to the folder where is the downloaded satellite image:

[https://drive.google.com/drive/folders/1Fiq2DB6yOwMM_H7d6nyY3QYBLgKxtFYM?usp=sharing](https://drive.google.com/drive/folders/1Fiq2DB6yOwMM_H7d6nyY3QYBLgKxtFYM?usp=sharing)

## Link where we can find the complete dataset (satellite images + channels + vegetation indices):

[https://drive.google.com/drive/folders/1Ohmo7nykACUC6g9gxHq4IghF69FNoFh_?usp=sharing](https://drive.google.com/drive/folders/1Ohmo7nykACUC6g9gxHq4IghF69FNoFh_?usp=sharing)

## Link to satellites information and examples:

[https://drive.google.com/drive/folders/1qfTQwDGKn8P0f_2yXrCq3rHTuN61043C?usp=sharing](https://drive.google.com/drive/folders/1qfTQwDGKn8P0f_2yXrCq3rHTuN61043C?usp=sharing)

## Results

Approx. one thousand original satellite images were downloaded from 2016 to 2020. Each folder has one image per band, one crop image per band, ten vegetation indices, and ten cropped vegetation indices.
