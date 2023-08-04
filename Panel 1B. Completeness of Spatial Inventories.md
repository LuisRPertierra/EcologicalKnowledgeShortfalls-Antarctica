
## FIG 1B SPATIAL COMPLETENESS OF BIODIVERSITY INVENTORIES 

SUPPORTING DATASET 1. SPECIES INVENTORIES AND ASSOCIATED INDICATOR DATA For all anaysis in the work we used a curated checklist of eukaryotic species, available as a supporting data paper: Pertierra et al. 2023 in review in the Biodiversity Data Journal https://digital.csic.es/handle/10261/307449. The list of species comprises about 2000 organisms with taxonomic classification from where all indicator data is compiled and analyzed. This information is found in the supporting dataset 1.

We calculated the spatial completeness of species inventories in the continent using the R package KnowBR.
All GBIF ocurrence records for any of the biodiversity checklist of species were retrieved from GBIF. This information was combined with extra spatial information from
an updated version of the spatial occurrence records of Wauchope et al. (2019) listed in the Antarctic Biodiversity Digital Database (doi:10.4225/15/59100ba9157f7) 
A total of >200.000 records were compilated. For this analysis we defined a spatial resolution of 50-50 Km. from where we generated a grid with cells for all the emerged lands of the continent.
Results display the slope of the inventory completeness as an indicator of saturation. We used the 'rational' method for the estimations and set the abundance to 1 for each observation.
Main results were masked to the cells containing ice-free areas and visually displayed with the licensed GIS software ArcMap 10.3.

REFERENCES HERE 
Lobo et al. 2018
Wauchope et al. 2019 

The R code written for the calculations and visualization is the following:

```{r message=FALSE, warning=FALSE}
# Load packages ---------------------------------------------------------
library(tidyverse)  # data manipulation
library(KnowBR) # completeness analysis
library(rgdal) # gis

data(adworld) # knowBR needs add world polygon to work 
# Load study area grid shapefile reprojected to CRS WGS84
grid <- readOGR("D:/Desktop/Science Resubmission/KnowBR/Antarctic Grids/Grid_AntarcticLandwgs84.shp")
plot(grid)
```

```{r message=FALSE, warning=FALSE}
# Load records
data <- read.csv('20230706120702_ant_SumUp_DataBase.csv', sep=';')
head(data)
# Records
# Add field of abundance for knowBR
data$abundance <- 1

data <- data[,c(1,5,6,7)]
```

Next we use the KnowBPolygon function to calculate inventory completeness using KnowBR. The following code is here disabled for knitting due to its long processing time running:

KnowBPolygon(data = data, 
             shape = grid, admAreas = FALSE,  # Use predefined grid as personalized polygons  
             shapenames = "PageNumber",    # WRITE HERE name of "id" cell from the grid
             jpg = TRUE, dec = ".")

Once the process ends a new object "Estimators.csv" with all the parameter estimates is generated in the working directory. From here we continue running code to visualize the output. Specifically we display the slope of new species discoveries per area.

```{r message=FALSE, warning=FALSE}
library(raster)
library(sf)

# Load output of estimators from knowBR analyses:
est <- read.csv('Estimators.CSV', header = TRUE, sep = ",")

grid2 <- merge(grid, est, by = "PageNumber",
                     all.x = TRUE)

shapefile(grid2, filename="C:/Users/Usuario/Desktop/figuresShortfallsData/grid2.shp", overwrite=TRUE)

Completeness <- raster::shapefile("grid2.shp") %>%
  sf::st_as_sf(crs = 4326)

Antarctic_Slope <- st_transform(Completeness, crs = 3031)

plot(Antarctic_Slope["Slope"])
```
