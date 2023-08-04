
# FIGURE 1A SPECIES DISCOVERY RATES

SUPPORTING DATASET 1. SPECIES INVENTORIES AND ASSOCIATED INDICATOR DATA
For all anaysis in the work we used a curated checklist of eukaryotic species, available as a supporting data paper: Pertierra et al. 2023 in review in the Biodiversity Data Journal https://digital.csic.es/handle/10261/307449.
The list of species comprises about 2000 organisms with taxonomic classification from where all indicator data is compiled and analyzed. This information is found in the supporting dataset 1.

REFERENCES HERE
Pertierra et al. 2023 Biodiversity Data Journal (currently in review)

For all the S1 species we retrieved the original discovery year (worldwide), and where available, the first record in Antarctica. The original description information was used to generate species accumulation curves. 
Approximate total diversity per group was estimated from the projection of the accumulation curves. 
The R code written for the calculations and visualization is the following:

## Variables

##### Set colors by Domain 

```{r}
colAnimalia = "darkred" 
colPlantae = "#fdae61" 
colProtist = "#2c7bb6" 
colFungi = "#fee090"
```

##### Set colors by shortfall (#now only 3)

```{r, message=FALSE, warning=FALSE}
setwd("C:/Users/Usuario/Desktop/figuresShortfallsData")
colors <- c("#56B4E9", "yellow", "orange") # "#009E73","orchid4"
```

## Packages

```{r message=FALSE, warning=FALSE}
library(data.table) 
library(tidyverse)
library(eulerr)
library(ggradar)
```

## Species Accumulation Curves (SACs)

### Load data from terrATANLIFE and prepare the fields to plot

```{r}

tANTA <- fread('terrANTALIFE_Eucaryots_1.txt', sep="\t", encoding="UTF-8", header=TRUE)
tANTA <- tANTA[!is.na(YearSACs),] # exclude NA's in Year data
tANTA <- tANTA[YearSACs >= 1850,] # filter temporal range of data
# tANTA <- tANTA[autoGBIFBiogeogrClassAssign != 'UncertainGlobally',] # filter uncertain Species
tANTA <- tANTA[order(tANTA$YearSACs),] # sort the year in ascending order 
tANTA$duplicated <- duplicated(tANTA$SpeciesAccepted) # assign a false to each new species 
tANTA$specacum <- ifelse(tANTA$duplicated == FALSE, 1, 0)# assign "1" to each new species and 0 to duplicated
year <- tANTA[,c('SpeciesAccepted','Kingdom','YearSACs','specacum')][specacum == 1,]
year$Counts <- as.integer(1) # assign 1 to each occurrence record
# prepare data for plotting Counts x year
occ_year <- aggregate(Counts ~ YearSACs, year, sum) #Sum species by year
occ_year <- aggregate(cbind(Counts, specacum)  ~ YearSACs, year, sum) # groups by year
occ_year$acumulado <- cumsum(occ_year$specacum) # sums the number of species described each year
```

### Estimating the value of the end of the curve (by 10 years)

```{r}
y10 <- nrow(occ_year)-10
y0 <- nrow(occ_year)
sum(occ_year[y10:y0,]$Counts)
y20 <- nrow(occ_year)-20
sum(occ_year[y20:y10,]$Counts)
```

## Plot SACs by kingdom

```{r, message=FALSE, warning=FALSE}
kingdom <- unique(tANTA$Kingdom)
kingdom # Join Amoebozoa + SAR/Protist
tANTA <- tANTA %>% 
  mutate(joinKingdom = case_when(Kingdom == "SAR/Protist" ~ 'Protist',
                                 Kingdom == "Amoebozoa" ~ 'Protist',
                                TRUE ~ Kingdom))
kingdom <- unique(tANTA$joinKingdom) 
kingdom

for (i in kingdom){
  dfk <- tANTA[joinKingdom == i,]
  dfk <- dfk[order(dfk$YearSACs),] # sort the year in ascending order 
  dfk$duplicated <- duplicated(dfk$SpeciesAccepted) # assign a false to each species new species 
  dfk$specacum <- ifelse(dfk$duplicated == FALSE, 1, 0)# assign "1" to each new species and 0 to duplicated
  # prepare data for plotting Counts x year
  dfk$Counts <- as.integer(1)
  occ_yeark <- aggregate(Counts ~ YearSACs, dfk, sum)
  occ_yeark <- aggregate(cbind(Counts, specacum)  ~ YearSACs, dfk, sum) # groups by year
  occ_yeark$acumulado <- cumsum(occ_yeark$specacum) # sums the number of species described each year
  
  assign(paste0(i,'_data'), dfk)
  # fwrite(occ_yeark, paste0(i, '_occYear.csv'), sep=";")
  assign(paste0(i,'_occYear'), occ_yeark)
}
```

## SACs Figure:

```{r, message=FALSE, warning=FALSE}
(plot1b <- ggplot() +
    geom_line(mapping = aes(x = occ_year$YearSACs, y = occ_year$acumulado, color='All'), size = 1) +
    geom_line(mapping = aes(x = Plantae_occYear$YearSACs, y = Plantae_occYear$acumulado, 
                            color = "Plantae"), size = 1) +
    geom_line(mapping = aes(x = Animalia_occYear$YearSACs, y = Animalia_occYear$acumulado, 
                            color = "Animalia"), size = 1) +
    geom_line(mapping = aes(x = Fungi_occYear$YearSACs, y = Fungi_occYear$acumulado, 
                            color = "Fungi"), size = 1) +
    geom_line(mapping = aes(x = Protist_occYear$YearSACs, y = Protist_occYear$acumulado, 
                            color = "Protist"), size = 1)+
    scale_color_manual(name='',
                       breaks = c('All', 'Plantae', 'Animalia', 'Fungi', 'Protist'),
                       values = c("All"="black",
                                  "Animalia"= colAnimalia,
                                  "Plantae"= colPlantae,
                                  "Protist"= colProtist,
                                  "Fungi"= colFungi)) +
    scale_x_continuous(name = "Year", breaks = seq(1850, 2025, by = 25))+
    ylab("Observed number of species")+
    theme_bw() +
    theme(legend.position = c(0.1, 0.75),
          panel.border = element_blank())
)
```
