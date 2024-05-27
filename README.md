# Detecting_Forest_Windthrow
Using Satellite Imagery to detect windthrow in forests

## Data 
The data can be found in the Data folder. Ground truth polygons can be found in shp files in the FORWIND sub-folder
The outline coordinates of polygons drawn around these areas to fit local models can be found in the accompanying txt file

## Data columns 
* **Id_poly** -> The polygon ID number in the original FORWIND database 	
* **StormName** -> The storm name - there are 3 unique storms
* EventType -> This is windStorm for all 
* Damage_deg -> The percentage of trees within the polygon which were deeemed to be damaged
* geometry -> The polygons geometry, all multipolygons have been unpacked	
* Area(Ha) -> The area of the polygon 
* pixels -> The number of satellite imagery pixels within the polygon assuming a 20m resolution

### Ground truth data source 
This data is derived from the FORWIND dataset by Forzieri et al. (2019), it is available [here:]([https://figshare.com/articles/dataset/A_spatially-explicit_database_of_wind_disturbances_in_European_forests_over_the_period_2000-2018/9555008). It contains shape files of damaged forest for many European storms over the period 2000-2018. Storms were assessed using either aerial photointerpretation, field survey or satellite imagery. 

The following preprocessing is carried out on the data:
* Removal of storms which don't have a defined damage degree (this is most of the dataset!)
* Removal of storms which occurred before 2015 when we do not have Sentinel data
* Explode any multipolygons
* Removal of extremely large polygons, some polygons were found to be massive e.g. 1000Ha and of low value, they had straight edges and a fixed damage degree. These are not particularly useful 
* Removal of polygons for which the damage degree is less than 60%
  * It was found from early models that some of the low damage levels were extremely difficult to detect, adding a 60% damage degree threshold meant that only higher quality polygons are used. These polygons were spot checked manually using Earth Engine Pro
* Removal of exremely small polygons - (less than 400m^2 in area), such small polygons are hard to classify as damaged/undamaged so they reduced the accuracy of early models and were  

This leaves 3 storms totalling 19,000 hectares, but there is significant spatial imbalance. 
1. storm Xavier in northern Germany 11Ha
2. storm Vaia in north-east Italy 17,854 Ha
3. a nameless storm in central Italy, 1,021 Ha, we only have Sentinel 1 data for this storm

## Potential revisions
If we end up using some LANDSAT or MODIS data it will open up the potential to use more storms to train on, given the significantly longer temporal availability of LANDSAT data 
