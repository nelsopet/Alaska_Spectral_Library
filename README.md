# Processing Headwall Hyperspectral Rasters using Tiling and Parallelization:  R (tutorial)

## Author & Contributor List
*1. Dr. Peter Nelson*      
*2. Kevaughan Smith*  
*3. Jane Pettit*  
*4. Catherine Chan*  

## Contents
- [Introduction](#Introduction)
- [Preparing Spectral Libraries](#Introduction)
- [Building classification model](#Introduction)
- [Classifying Hyperspectral Data](#Introduction)
    
### Introduction
This tutorial demonstrates how to use functions developed to read, process and create large spatial (Headwall raster) data sets. In principle, both examples follow the same systematic approach:
1. Preparing your spectral library
2. Building classification model 
3. Classifying Headwall Hyperspectral Raster

JANE'S ERDS (IMAGE HERE)

### Preparing Spectral Libraries using PSR Data
SAMPLE CODE SHOULD  (Use function that ads metadata, removes uncalibrated scans from spectral library,equalizes the number of observation per class and plots the median spectra for each class)
``` 
SpecLib<-Reduce(spectrolab::combine,list_of_SpecLib)%>% # dim(n_samples=1989, n_wavelegths=2151)
  as.data.frame()%>% # Converts Spectral Object to a dataframe
  dplyr::select(-sample_name)%>% # Removes unwanted column 
  inner_join(Species_groups,by="PFT")%>% #Joins dataframe with all the species info to our spectral library
  dplyr::select(ScanID,PFT,PFT_2,PFT_3,PFT_4,Area,everything()) #Reorders columns  
  ```
  
FACET PLOT SHOWING SPECTRAL FEATURES OF EACH CLASS

![](/Cladonia.jpg)

### Preparing Spectral Libraries using didgitized Raster Pixels
SAMPLE CODE SHOULD  (Use a function that adds metadata, removes uncalibrated scans from spectral library,equalizes the number of observation per class and plots the median spectra for each class)
``` 
SpecLib<-SpectralLibrary_prep(filename = path/to/shapefiles-0f-didgitizedpolygons
                                outfile = path/to/outputfolder)
  ```

### Building classification model
SAMPLE CODE SHOULD (use a function to build a Build a classification model)
``` 
Classification_Model<-Spectral_Classifier(spectral_Library = path/to/SpectralLibrary,
                                          Out_file = path/to/outputfolder)
  ```
MODEL RESULTS
```
Call:
 randomForest(formula = Classes ~ ., data = newdf, mtry = sqrt(ncol(newdf)),      ntree = 1001, localImp = TRUE) 
               Type of random forest: classification
                     Number of trees: 1001
No. of variables tried at each split: 7

        OOB estimate of  error rate: 25.69%
```  
CONFUSION MATRIX 
```
                 Abiotic_Litter Abiotic_Rock Abiotic_Soil Dwarf_Shrub_Broad Dwarf_Shrub_Needle Forb Graminoid_Grass
Abiotic_Litter                  4            0            0                 0                  1    0               0
Abiotic_Rock                    1           13            5                 0                  0    0               0
Abiotic_Soil                    0            5            2                 0                  0    0               0
Dwarf_Shrub_Broad               0            0            0                79                  0    3               0
Dwarf_Shrub_Needle              0            0            0                 4                 13    0               0
Forb                            0            0            0                 1                  0   28               0
Graminoid_Grass                 0            0            0                 0                  0    0               3
Graminoid_Sedge                 0            0            0                 1                  0    1               0
Lichen_Dark                     0            0            0                 4                  0    2               0
Lichen_Light                    0            0            0                 0                  0    2               0
Lichen_Yellow                   0            0            0                 1                  0    0               0
Moss_Acrocarp                   0            0            0                 0                  2    0               0
Moss_Pleurocarp                 0            0            0                 0                  0    0               0
Moss_Sphagnum                   0            0            0                 0                  0    0               0
```

### Classifying Hyperspectral Data
SAMPLE CODE (use a function to created a predicted layer of a datacube)
```
PredictedRaster_Layer<-HyperspecImageClassifier(filename = Path/to/datacube.envi,
                                                spectral_Library = path/to/SpectralLibrary.csv,
                                                Out_file = path/to/outputfile)
  ```

IMAGE OF DATACUBE 
```
plot(PredictedRaster_Layer)
```

![](/EightMileTest_Plot_Prediction.jpg)
















