
# 	FIGURE 2B. VALUE OF INFORMATION ASSESSMENTS 
## VALUE-OF-INFORMATION. VOI-Potential and Heatmaps

SUPPORTING DATASET 3. EXPERT ELICITATION ON THE POTENTIAL VALUE OF INFORMATION. All the provided responses (coded in the form of annonymous numerical scores) from the opinions of all the participant experts were stored in a summary table (Supplementary File 3). These materials were generated with the following methodological procedures and used for the below calculations and visualizations.

File Name: S3. VOI_BY_EXPERT_RESPONSE.CSV

#### Methods to define the potential value-of-information elicitation and generate the applied value of information of Antarctic ecological knowledge discipline for conservation with radar charts 

The Antarctic expert evaluations inform on the relative perceived knowledge gaps per group. However they do not attribute a value of relevance for practical applications since all knowledge disciplines have the same potential for future interest in the theoretical comprehension of the functioning of the ecosystems. Nonetheless specific practical applications require specific knowledge bits to maximize their applied inference. To evaluate the different value of information for distinct purposes here we used the knowledge of broad experts in conservation and shortfalls theorycrafting. 

The experts defined the relevance of the different ecological knowledge disciplines for a set of usages. These were: climate change action, biosecurity management, sustainable resource extraction, strategic area protection and bioprospecting. Recreational management was not included as they are directly benefited by a combination of these aspects. In addition, the value of cultural scientific gains were simbolically assinged to constant intermediate levels across disciplines based in the principle that all science can be theoretically equally relevant in the future. The value of information was also subdivided in 3 categories per discipline in attention to the complexity of the required information for the purpose since not all information has the same complexity and demands for construction as advanced information is nested on previos base knowledge. The description of categories is the following:

1.	LINNEAN

1.1 baseline	having species inventories

1.2 intermediate	progress in new species descriptions with inclusion of guess-estimates

1.3 advanced	having holistic repositories with accurate taxonomically-resolved intraspecific variation (subspecies, morphotypes) 

2.	PRESTONIAN

2.1 baseline	Having a description of total abundances

2.2 intermediate	Mapping population trends & endangered status

2.3 advanced	Comprehending population dynamics (including stability and critical size)

3. WALLACEAN

3.1 baseline	Having a description of species occurrence ranges

3.2 intermediate	Mapping dynamic models of species distributions

3.3 advanced	Comprehending biogeographical patterns and processes

4. DARWINIAN

4.1 baseline	Having a baseline description of evolutionary relationships

4.2 intermediate	Mapping the motion of trait evolution

4.3 advanced	Comprehending evolutionary processes (patterns of speciation nurseries, processes of diversification & drivers)

5. RAUNKIAERAN

5.1 baseline	Having a description of representative traits & functions

5.2 intermediate	Mapping the trait space, diversity and plasticity

5.3 advanced	Comprehending the underlying mechanisms shaping trait diversity & plasticity

6. HUTCHINSONIAN

6.1 baseline	Having a general characterization of abiotic tolerance limits

6.2 intermediate	Mapping exact metabolomics (productivity levels) beyond discrete levels

6.3 advanced	Comprehending trade-offs in life strategies (resource management) shaping productivity levels

7. ELTONIAN

7.1 baseline	Having a general description of representative species-to-species interactions

7.2 intermediate	Mapping functional diversity (levels of non-trophic interactions at the community level)

7.3 advanced	Comprehending the resilience of trophic interaction networks (cascade effects at ecosystem level)

In attention to the implications of these categories five experts responded with their view on the respective potential value of information (see below summary information of the averaged values). This provides a theoretical potential value-of-information of each ecological discipline for specific conservation practice goals (1 to 5, i.e. going from  minimally relevant to very relevant).

Lastly, the results of the attributed knowledge gaps from the Antarctic expert elicitation (all 21 participants from the Expert Elicitation - see section 2A methods) and the potential value of information attributed from general conservation experts were combined (with output values now ranking from 1 > non-prioritary by knowledge gap being either filled already, non-essential or both, to 500 > very prioritary to fill). The results indicate the current applicable value of information for different usages from deepeing the Antarctic ecological knowledge and severing the gaps. This estimate thus accounts for the severity of the knowledge gaps and their relevance. Main results (figure 2b) are shown in form of a set of radar graphs per biodiversity subgroup (7 categories).

REFERENCES HERE
Hortal et al. 2015

The R code written for the calculations and visualization is the following:

Load nonAQ experts estimates

```{r message=FALSE, warning=FALSE}
VOI <- read.csv("VOI_BY_EXPERT_SUMMARY.csv", sep=";")
head(VOI)
# Summarize info of experts
sVOI <- VOI %>% dplyr::select(-Expert) %>% 
        group_by(Shortfall,Shortfall_Sub, Aplicacion1, Aplicacion1.1) %>%
        summarize(meanVOI = mean(VOI, na.rm=TRUE))
# Create a new field to join shortfall+level strings
sVOI$SF <- paste0(sVOI$Shortfall,'_', sVOI$Shortfall_Sub)
head(sVOI)
```

Load IDEA dataframe (AQ experts BestEstimateR)

```{r message=FALSE, warning=FALSE}
ELICIT <- read.csv("IDEA_dataframe.csv", sep=";")
head(ELICIT)
# Summarize info of experts
sELICIT <- ELICIT %>% dplyr::select(Shortfall, BestEstimateR,Group) %>% 
          group_by(Shortfall, Group) %>%
          summarize(BestEstimateR = mean(BestEstimateR, na.rm=TRUE)) %>%
          pivot_wider(names_from = Group, values_from = BestEstimateR)
# Inverted values of estimates
sELICITinv <- 100 - sELICIT[,2:8]
sELICITinv <- cbind(sELICIT$Shortfall, sELICITinv)
head(sELICITinv)
```

Reorder variables classes to improve visualization

```{r message=FALSE, warning=FALSE}
level_order <- c("WALLACEAN_advanced","WALLACEAN_intermediate",
                 "WALLACEAN_baseline","DARWINIAN_advanced",
                 "DARWINIAN_intermediate","DARWINIAN_baseline",
                 "RAUNKIAERAN_advanced","RAUNKIAERAN_intermediate",
                 "RAUNKIAERAN_baseline","HUTCHINSONIAN_advanced",
                 "HUTCHINSONIAN_intermediate", "HUTCHINSONIAN_baseline",
                 "ELTONIAN_advanced","ELTONIAN_intermediate","ELTONIAN_baseline",
                 "PRESTONIAN_advanced","PRESTONIAN_intermediate",
                 "PRESTONIAN_baseline",
                 "LINNEAN_advanced","LINNEAN_intermediate","LINNEAN_baseline")

group_order <- c('Vertebrata', 'Invertebrata','Embryophyta', 'Holomycota',                           'Procaryota','SAR/Chromista','Virus')
```

Create a loop to calculate estimates of VOI\*IDEA by Application and Shortfall+level

Print heatmaps by application

```{r message=FALSE, warning=FALSE , fig.height=10, fig.width=10}
for(j in unique(sVOI$Aplicacion1.1)){ # loop each aplication
  appVOI <- sVOI %>% 
              filter(Aplicacion1.1 == j) %>% #select subset of each application
              dplyr::select(Shortfall, Shortfall_Sub, SF, meanVOI) 

  heatmap <- data.frame() # empty dataframe to insert heatmap values

  for(i in unique(appVOI$Shortfall)){ # loop each shortfall
    
    a <- sELICITinv[sELICIT$Shortfall == i,] # Filter values of Elici. by shortfall
    b <- appVOI[appVOI$Shortfall == i,] # Filter values of VOI by shortfall
    c <- merge(b, a, by.x='Shortfall', by.y='sELICIT$Shortfall', all.x=TRUE)
    # Multiplication of IDEA estimate of each group * meanVOI value by Shortfall level 
    d <- apply(c[ , 6:12], 2, function(x) x*c[,'meanVOI'])
    e <- cbind(b,d)
    f <- e %>% pivot_longer(cols = 6:12, names_to = 'Group', 
                values_to = 'VOI_IDEA') 
    heatmap <- rbind(heatmap, f) #Join new rows by shortfall in each loop
  }

  print(ggplot(heatmap,
        aes(x = factor(Group, level = group_order),
            y = factor(SF, level = level_order),
            fill = VOI_IDEA)) +
        geom_tile(color = "black") +
        scale_fill_distiller(palette = "RdYlBu", direction = -1)  +
        labs(title = j) +
        xlab('') + ylab('') +
        theme_minimal() +
        geom_text(aes(label = round(VOI_IDEA,2)),
                  color = "black", size = 4))
}
```

```
VOI <- read.csv("VOI_BY_EXPERT_SUMMARY.csv", sep=";")
head(VOI)
sVOI <- VOI %>% dplyr::select(-Expert) %>% 
  group_by(Shortfall,Shortfall_Sub, Aplicacion1, Aplicacion1.1) %>%
  summarize(meanVOI = mean(VOI, na.rm=TRUE))
sVOI$SF <- paste0(sVOI$Shortfall,'_', sVOI$Shortfall_Sub)
head(sVOI)


ELICIT <- read.csv("IDEA_dataframe.csv", sep=";")
head(ELICIT)
sELICIT <- ELICIT %>% dplyr::select(Shortfall, BestEstimateR,Group) %>% 
  group_by(Shortfall, Group) %>%
  summarize(BestEstimateR = mean(BestEstimateR, na.rm=TRUE)) %>%
  pivot_wider(names_from = Group, values_from = BestEstimateR)
sELICITinv <- 100 - sELICIT[,2:8]
sELICITinv <- cbind(sELICIT$Shortfall, sELICITinv)
head(sELICITinv)


level_order <- c("WALLACEAN_advanced","WALLACEAN_intermediate",
                 "WALLACEAN_baseline","DARWINIAN_advanced",
                 "DARWINIAN_intermediate","DARWINIAN_baseline",
                 "RAUNKIAERAN_advanced","RAUNKIAERAN_intermediate",
                 "RAUNKIAERAN_baseline","HUTCHINSONIAN_advanced",
                 "HUTCHINSONIAN_intermediate", "HUTCHINSONIAN_baseline",
                 "ELTONIAN_advanced","ELTONIAN_intermediate","ELTONIAN_baseline",
                 "PRESTONIAN_advanced","PRESTONIAN_intermediate",
                 "PRESTONIAN_baseline",
                 "LINNEAN_advanced","LINNEAN_intermediate","LINNEAN_baseline")

group_order <- c('Vertebrata', 'Invertebrata','Embryophyta', 'Holomycota', 'Procaryota','SAR/Chromista','Virus')

for(j in unique(sVOI$Aplicacion1.1)){ # loop each aplication
  appVOI <- sVOI %>% 
    dplyr::select(Aplicacion1.1, Shortfall, Shortfall_Sub, SF, meanVOI) 
}

appVOI <- appVOI[, -(1)]
  
  heatmap <- data.frame() # empty dataframe to insert heatmap values
  
  for(i in unique(appVOI$Shortfall)){ # loop each shortfall
    
    a <- sELICITinv[sELICIT$Shortfall == i,] # Filter values of Elici. by shortfall
    b <- appVOI[appVOI$Shortfall == i,] # Filter values of VOI by shortfall
    c <- merge(b, a, by.x='Shortfall', by.y='sELICIT$Shortfall', all.x=TRUE)
    # Multiplication of IDEA estimate of each group * meanVOI value by Shortfall level 
    d <- apply(c[ , 6:12], 2, function(x) x*c[,'meanVOI'])
    e <- cbind(b,d)
    f <- e %>% pivot_longer(cols = 6:12, names_to = 'Group', 
                            values_to = 'VOI_IDEA') 
    heatmap <- rbind(heatmap, f) #Join new rows by shortfall in each loop
  }
  
  print(ggplot(heatmap,
               aes(x = factor(Group, level = group_order),
                   y = factor(SF, level = level_order),
                   fill = VOI_IDEA)) +
          geom_tile(color = "black") +
          scale_fill_distiller(palette = "RdYlBu", direction = -1)  +
          labs(title = j) +
          xlab('') + ylab('') +
          theme_minimal() +
          geom_text(aes(label = round(VOI_IDEA,2)),
                    color = "black", size = 4))
}
```
Here we calculate and display first the mean VOI score given by the Biodiversity Experts averaged for Overall Conservation by merging all conservation practice goals

```
conservVOI <- sVOI %>% filter(Aplicacion1 == 'Conservation') %>% 
  dplyr::select(-Aplicacion1.1) %>% 
  group_by(Shortfall, Shortfall_Sub, SF) %>%
  summarize(meanVOI = mean(meanVOI, na.rm=TRUE))%>% 
  ungroup()

heatmap <- data.frame()
for(i in unique(appVOI$Shortfall)){
  a <- sELICITinv[sELICIT$Shortfall == i,] # Filter values of Elici. by shortfall
  b <- conservVOI[conservVOI$Shortfall == i,] # Filter values of VOI by shortfall
  c <- merge(b, a, by.x='Shortfall', by.y='sELICIT$Shortfall', all.x=TRUE)
  d <- apply(c[ , 5:11], 2, function(x) x*c[,'meanVOI'])
  e <- cbind(b,d)
  f <- e %>% pivot_longer(cols = 5:11, names_to = 'Group', 
                          values_to = 'VOI_IDEA') 
  heatmap <- rbind(heatmap, f) #Join new rows by shortfall in each loop
}
print(ggplot(heatmap,
             aes(x = factor(Group, level = group_order),
                 y = factor(SF, level = level_order),
                 fill = VOI_IDEA)) +
        geom_tile(color = "black") +
        scale_fill_distiller(palette = "RdYlBu", direction = -1)  +
        labs(title = 'Conservation') +
        xlab('') + ylab('') +
        theme_minimal() +
        geom_text(aes(label = round(VOI_IDEA,2)),
                  color = "black", size = 4))

```
## VALUE-OF-INFORMATION. Radar charts
### Radar charts with VOI scores per conservation practice (exemplified by conservation practice) when applied to Antarctic Knowledge Gaps

The VOI-potential scores identified by the Shortfall experts are now met separately with each of the  knowledge gaps per ecological discipline and biodiversity group identified by the Antarctic biodiversity experts
All assessments are shown with attention to the VOI-potential of baseline knowledge.

```
climVOI <- appVOI %>% filter(Aplicacion1.1 == 'Climate Change ') %>%  ##replace the Aplicacion Field & rename climVOI to "..."VOI object with any other conservation practice to plot (e.g "Area Protection")
  group_by(Shortfall, Shortfall_Sub, SF) %>%
  summarize(meanVOI = mean(meanVOI, na.rm=TRUE))%>% 
  ungroup()

heatmap <- data.frame()
for(i in unique(appVOI$Shortfall)){
  a <- sELICITinv[sELICIT$Shortfall == i,] # Filter values of Elici. by shortfall
  b <- climVOI[climVOI$Shortfall == i,] # Filter values of VOI by shortfall
  c <- merge(b, a, by.x='Shortfall', by.y='sELICIT$Shortfall', all.x=TRUE)
  d <- apply(c[ , 5:11], 2, function(x) x*c[,'meanVOI'])
  e <- cbind(b,d)
  f <- e %>% pivot_longer(cols = 5:11, names_to = 'Group', 
                          values_to = 'VOI_IDEA') 
  heatmap <- rbind(heatmap, f) #Join new rows by shortfall in each loop
}
print(ggplot(heatmap,
             aes(x = factor(Group, level = group_order),
                 y = factor(SF, level = level_order),
                 fill = VOI_IDEA)) +
        geom_tile(color = "black") +
        scale_fill_distiller(palette = "RdYlBu", direction = -1)  +
        labs(title = 'climate') +
        xlab('') + ylab('') +
        theme_minimal() +
        geom_text(aes(label = round(VOI_IDEA,2)),
                  color = "black", size = 4))
head(heatmap)

```
Note: Heatmap object needs to be remapped per conservation practice by specifying: VOI <- appVOI %>% filter(Aplicacion1.1 == 'NAME OF ACTIVITY)

```
  library(ggradar)

  df <- heatmap %>%  select(c(Shortfall,Shortfall_Sub, Group, VOI_IDEA)) %>%
    pivot_wider(names_from = Shortfall, values_from = VOI_IDEA)
  
  df <- as.data.frame(df)
  
  df$Group <- paste(df$Shortfall_Sub, df$Group, sep=" ")
  
  df <- df[df$Shortfall_Sub=="baseline",]
  
  scenario <- df[,2:9]
  
  colnames(scenario) <- c("Group", "Evolutionary info", "Interactions", "Tolerances", "Diversity", "Population dynamics", "Traits", "Distributions")
  
  as.character(scenario$`Group Level`)
  
  plot(ggradar(scenario, grid.max= 500))
  
  scenario <- scenario[, c(1, 5, 8, 2, 4, 3, 7, 6)]
  
  plot(ggradar(scenario, 
               base.size = 20,
               grid.label.size = 0,
               axis.label.size = 4,
               group.point.size = 1,
               background.circle.colour = "grey",
               axis.line.colour = "gray60",
               gridline.min.colour = "gray60",
               gridline.mid.colour = "gray60",
               gridline.max.colour = "gray60",
               grid.line.width = 0.1,
               plot.legend = TRUE,
               grid.max= 500,
               plot.title = "climate", ##name here according to conservaction practice
               legend.position = "bottom",
               legend.text.size = 7))

```
  
  
  



