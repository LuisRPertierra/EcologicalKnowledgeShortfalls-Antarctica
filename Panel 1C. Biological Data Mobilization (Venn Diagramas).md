## FIGURE 1C VENN DIAGRAMS OF BIOLOGICAL DATA MOBILIZATION TO CENTRALIZED META-REPOSITORIES

SUPPORTING DATASET 1. SPECIES INVENTORIES AND ASSOCIATED INDICATOR DATA For all anaysis in the work we used a curated checklist of eukaryotic species, available as a supporting data paper: Pertierra et al. 2023 in review in the Biodiversity Data Journal https://digital.csic.es/handle/10261/307449. The list of species comprises about 2000 organisms with taxonomic classification from where all indicator data is compiled and analyzed. 

FILE NAME: S1. IND_DATABASE_COVERAGE.CSV

##### Methods for the visualization of ecological indicator coverage per discipline

We depict the completeness of ecological data mobilization towards centralized public meta-repositories. Venn diagrams of data mobilization to global online repositories were generated from all (or some) subclades for eight biodiversity groups (2 per realm in sensu lato): 
1. VERTEBRATES Seals and birds
2. INVERTEBRATES see breakdown in supplementary: free-living arthropods, free living non-arthropod invertebrates, and Parasites (arthropod and non-arthropod), 
3. HIGHER PLANTAE Vascular plants 
4. BRYOPHYTES Mosses, hornworts and liverworts
5. LICHEN Lichenized fungi 
6. FUNGI Free-living fungi
7. ALGAE Algae (green plus diatoms).
8. PROTISTS Heterotrophic SAR/Chromista.

No diagrams could be made for microorganism groups due to their large numbers and absence of consolidated lists of species. Thus, we restricted the analysis to the species of the eukaryotic checklist of Antarctic biodiversity
One representative data mobilization indicator was selected per shortfall (excluding Linnean as we use the realized inventories as reference and Prestonian since there is only meta-population information for vertebrates and vasculars, using as reference the IUCN red list). It must be noted that these indicators do not provide a general view of the status for each shortfall as they cover many elements, but rather provide an example of a widespread metric typically used in some elements of the shortfall dimension. These are:
1. BIOGEOGRAPHICAL SPATIAL DATA. 'Wallacean' data mobilization was retrieved using the indicator “sps. with spatial records in GBIF repository”. See GBIF download methods in 2B. https://github.com/LuisRPertierra/EcologicalKnowledgeShortfalls-Antarctica/blob/main/Panel%201B.%20Completeness%20of%20Spatial%20Inventories.md
2. EVOLUTIONARY DATA. 'Darwinian' was evaluated with the indicator “sps. with genetic sequences in GenBank repository”.
See supporting PEARL code for data retrieval in Genbank https://github.com/LuisRPertierra/EcologicalKnowledgeShortfalls-Antarctica/blob/main/PEARL%20Code%20for%20Genbank%20Accesion
3. TRAIT DATA. 'Raunkieran' was evaluated with the indicator “sps. with trait data in centralized repositories”. For the case of vertebrates it was Pantheria. For lichen and vascular it was TRY database. 
For bryophytes it was an extension of TRY named BryoForTraits (largely biased towards forest species, but the most complete compendium available). Fungi data was complemented from FungalTraits repository. 
For other groups we could not find centralized trait repositories and so were all scored to 0. TRY data accession queries were requested via https://www.try-db.org/TryWeb/dp.php Using the fungaltraits v0.0.3 r package we obtained all fungal functional traits available for our curated species inventory. We recorded which fungal species had functional traits present as well as counting how many individual records of functional traits have been recorded for each species on our inventory.
4.	ABIOTIC TOLERANCE DATA. 'Hutchinsonian' was evaluated with the indicator “sps. with thermal tolerance limits in Globtherm compendium”. However, in this case we updated the compendium with additional entries of 
Antarctic species with thermal profiles from Antarctic research literature. Data compilation was searched with literature retrieval queries at Web of Science https://www.webofscience.com/wos/woscc/basic-search 
5.	BIOTIC INTERACTION AND NETWORK DATA. 'Eltonian' was evaluated with the binary indicator “sps. with reported interactions listed in GLOBI repository). See supporting R code for data accession in https://github.com/LuisRPertierra/EcologicalKnowledgeShortfalls-Antarctica/blob/main/Supporting%20R%20Code%20for%20GLOBI%20Data%20Accession

All indicator information per species can be seen in the supporting dataset 1 in the corresponding columns of the same name as a binary character (1= presence, 0=absence). See supporting R code for data retrieval in GLOBI

REFERENCES

J. Hortal, F. d. Bello, J. A. F. Diniz-Filho, T. M. Lewinsohn, J. M. Lobo, R. J. Ladle, Seven Shortfalls that Beset Large-Scale Knowledge of Biodiversity. Annual Review of Ecology, Evolution, and Systematics 46, 523-549 (2015)10.1146/annurev-ecolsys-112414-054400).

H. S. Wauchope, J. D. Shaw, A. Terauds, A snapshot of biodiversity protection in Antarctica. Nature Communications 10, 946 (2019); published online Epub2019/02/26 (10.1038/s41467-019-08915-6).

M. Leray, N. Knowlton, S.-L. Ho, B. N. Nguyen, R. J. Machida, GenBank is a reliable resource for 21st century biodiversity research. Proceedings of the National Academy of Sciences 116, 22651-22656 (2019)doi:10.1073/pnas.1911714116).

J. Kattge et al. TRY plant trait database – enhanced coverage and open access. Global Change Biology 26, 119-188 (2020) https://doi.org/10.1111/gcb.14904).

K. E. Jones, J. Bielby, M. Cardillo, S. A. Fritz, J. O'Dell, C. D. L. Orme, K. Safi, W. Sechrest, E. H. Boakes, C. Carbone, C. Connolly, M. J. Cutts, J. K. Foster, R. Grenyer, M. Habib, C. A. Plaster, S. A. Price, E. A. Rigby, J. Rist, A. Teacher, O. R. P. Bininda-Emonds, J. L. Gittleman, G. M. Mace, A. Purvis, PanTHERIA: a species-level database of life history, ecology, and geography of extant and recently extinct mammals. Ecology 90, 2648-2648 (2009)https://doi.org/10.1890/08-1494.1).

J. H. Poelen, J. D. Simons, C. J. Mungall, Global biotic interactions: An open infrastructure to share and analyze species-interaction datasets. Ecological Informatics 24, 148-159 (2014); published online Epub2014/11/01/ (https://doi.org/10.1016/j.ecoinf.2014.08.005).

GBIF: The Global Biodiversity Information Facility (year) What is GBIF?. Available from https://www.gbif.org/what-is-gbif 

Põlme, S., Abarenkov, K., Henrik Nilsson, R. et al. FungalTraits: a user-friendly traits database of fungi and fungus-like stramenopiles. Fungal Diversity 105, 1–16 (2020). https://doi.org/10.1007/s13225-020-00466-2

Bennett, J., Calosi, P., Clusella-Trullas, S. et al. GlobTherm, a global database on thermal tolerances for aquatic and terrestrial organisms. Sci Data 5, 180022 (2018). https://doi.org/10.1038/sdata.2018.22

The R code written for the calculations and visualization is the following:

#### 1. Read dataset with information by shortfall (0/1 indicators)

```{r message=FALSE, warning=FALSE}

library(data.table)

indicators <- fread('Inventory5IndicatorsF.csv', header=TRUE, sep=";")
head(indicators[,c(1, 15:19)])
indicators$SpeciesAccepted <- NULL
indicators$Phylum <- NULL
```

#### 2. Delete species with all 0's

```{r, message=FALSE, warning=FALSE}
indicators <- indicators[Darwinean ==1 | Wallacean ==1 |Eltonian ==1 |Raunkieran ==1 |Hutchinsonian ==1, ]
```

#### 3. Unify kingdom name

```{r, message=FALSE, warning=FALSE}
kingdom <- unique(indicators$Kingdom.x)
kingdom # Join Amoebozoa + SAR/Protist

indicators <- indicators %>% 
  mutate(Kingdom.x = case_when(Kingdom.x == "SAR/Protist" ~ 'Protist',
                             Kingdom.x == "Amoebozoa" ~ 'Protist',
                             TRUE ~ Kingdom.x))
```

#### 4. Change to logic (0 = FALSE & 1 = TRUE)

```{r, message=FALSE, warning=FALSE}
LogValues <- as.data.frame(apply(indicators[,14:18],2, as.logical))

LogValues <- cbind(indicators$Kingdom.x, LogValues)

colnames(LogValues)[1] <- 'Kingdom'

LogValues$Kingdom <- as.factor(LogValues$Kingdom)
```

#### 5. Create venn plot using ellipse

### *All kingdoms*

```{r, message=FALSE, warning=FALSE}
library(eulerr)

colors <- c("#56B4E9", "yellow", "orange", "blue", "grey") # "#009E73","orchid4"

plot(euler(LogValues[,2:6], shape = "ellipse"), 
     fills = colors, 
     legend = TRUE, 
     edges = FALSE,
     quantities = TRUE)
```
### *By kingdom*

```{r echo=TRUE, message=FALSE, warning=FALSE}
library(dplyr)
kd <- unique(LogValues$Kingdom)
kd

for(i in kd){
df <- LogValues %>% filter(Kingdom == i)
  df <- df %>% dplyr::select(where(~ is.logical(.) && any(.)), where(negate(is.logical)))
  df <- df %>% relocate(Kingdom)
assign(paste0('df_', i), df)
print(plot(euler(df[,2:length(df)], shape = "ellipse"), 
         fills = colors, 
         legend = TRUE, 
         edges = FALSE,
         main=i,
         quantities = TRUE))
}

```
## *By functional group*
Here the coverage per functional group is displayed. To this end each species was assigned to a basic functional group characterization among each kingdom under the following groups: Animalia: macrofauna, soil invertebrates and parasitic invertebrates. Plantae/Algae: Embryophytes & Algae. SAR/Protist: Heterotrophic & Algae. Fungi: Lichenized & Non-lichenized fungi. 

```{r, message=FALSE, warning=FALSE}
func_indicators <- fread('Func_Inventory5Indicators.csv', header=TRUE, sep=",")
head(func_indicators[,c(1, 16:20)])
func_indicators$SpeciesAccepted <- NULL
func_indicators$Phylum.x <- NULL
func_indicators$Kingdom.x <- NULL

Func_Group <- unique(func_indicators$Func_group)
Func_Group

```

```{r echo=TRUE, message=FALSE, warning=FALSE}
LogValues <- as.data.frame(apply(func_indicators[,13:17],2, as.logical))

LogValues <- cbind(func_indicators$Func_group, LogValues)

colnames(LogValues)[1] <- 'Func_group'

LogValues$Func_group <- as.factor(LogValues$Func_group)

kd <- unique(LogValues$Func_group)
kd

for(i in kd){
df <- LogValues %>% filter(Func_group == i)
  df <- df %>% dplyr::select(where(~ is.logical(.) && any(.)), where(negate(is.logical)))
  df <- df %>% relocate(Func_group)
assign(paste0('df_', i), df)
print(plot(euler(df[,2:length(df)], shape = "ellipse"), 
         fills = colors, 
         legend = TRUE, 
         edges = FALSE,
         main=i,
         quantities = TRUE))
}

```

