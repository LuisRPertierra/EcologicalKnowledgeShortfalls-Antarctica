## 	FIGURE 2A. EXPERT ELICITATIONS OF ECOLOGICAL KNOWLEDGE SHORTFALLS BY BIODIVERSITY GROUP

A complementary screening and systematic revision of the state of knowledge under the shortfalls’ framework was performed under a structured IDEA elicitation process (Hemming et al. 2018) 
paired with a literature narrative synthesis. A diverse group with expertise in different aspects of ecological research were approached to jointly assess the global state of shortfalls 
in knowledge of Antarctic biodiversity and ecosystem functioning. Group experts were selected to confer diversity (age, sex and nationality) and specialization in different disciplines. 
Estimates were provided in a frequency completion format (%). These experts were asked to provide insight on the levels of our understanding of Antarctic biodiversity patterns and processes, 
identify knowledge gaps and unknowns, discuss the implications of the current state of knowledge for both theoretical understanding and practical conservation, and propose targeted mitigation efforts. 
Evaluations were organized into four groups of terrestrial, semi-terrestrial and/or freshwater species: vertebrates (breeding birds and seals), invertebrates (arthropods and other microinvertebrates, free-living and parasitic), 
flora (vascular plants, bryophytes, lichen and fungi), algae and protists, and microorganisms (bacteria, archeae and viruses). Experts were questioned about the status of the different groups of biota present
 in Antarctic terrestrial & freshwater ecosystems following a four-step expert elicitation protocol (IDEA). We used standardized credible intervals (80%) together with levels of confidence given 
 by the participants following the Hemmings et al. (2018) formula. In cases where the adjusted intervals fell beyond reasonable bounds (such as [0, 100%] for probabilities), we
 truncated distributions at their extremes. Experts were encouraged to revise their estimates in Round 2 if the extrapolation did not represent their true belief.

Main summary results are then summarized in 8 categories (2 per realm in sensu lato)

REFERENCES HERE
Hemming et al. 2018

The R code written for the calculations and visualization is the following:

#For all main functional groups

Load dataframe

```{r message=FALSE, warning=FALSE}
data <- read.csv("IDEA_dataframe.csv", sep=";")
head(data)
```

Aggregate information of experts (mean)

```{r message=FALSE, warning=FALSE}
data1 <- data %>%
            group_by(Shortfall, Domain, FuncGroup) %>%
            summarize(BestEstimateR = mean(BestEstimateR), 
                     MinKnowledge = mean(MinKnowledge), 
                     MaxKnowledge = mean(MaxKnowledge),
                     ConfExpert = mean(ConfExpert),
                     CredExpert = mean(CredExpert))
head(data1)

listShortfall <- c("WALLACEAN","DARWINIAN","RAUNKIAERAN",
                   "HUTCHINSONIAN","ELTONIAN","PRESTONIAN","LINNEAN")
# Order FuncGroup By domain
data1$FuncGroup <- factor(data1$FuncGroup, 
              levels = c("Aves","Mammalia","Free living invertebrates","Parasite",
               "Bryophytes" ,"Vascular",
               "Lichenic fungi", "Non-lichenic fungi and yeast",
               "Algal mix","Archaea","Bacteria" ,"Protozoans","Virus"))
```

#### Plot (as original)

```{r fig.height=8, fig.width=9}
ggplot(data1) +
          aes(x = factor(Shortfall, level=listShortfall), 
              y = BestEstimateR, fill = BestEstimateR) +
          geom_col() +
          geom_errorbar(aes(factor(Shortfall, level=listShortfall), 
                            ymin=MinKnowledge, ymax=MaxKnowledge), 
                         width = 0.4, colour="darkgrey", alpha=0.9, size=0.5) +
          scale_fill_distiller(palette = "Spectral", direction = 1) +
          theme_minimal() + 
          xlab('')+ ylab('')+
          coord_flip()+
          facet_wrap(vars(FuncGroup), scales = "free_x")
```

#### Plot (new palette)

```{r fig.height=8, fig.width=9}
ggplot(data1) +
          aes(x = factor(Shortfall, level=listShortfall), 
              y = BestEstimateR, fill = BestEstimateR) +
          geom_col() +
          geom_errorbar(aes(factor(Shortfall, level=listShortfall), 
                            ymin=MinKnowledge, ymax=MaxKnowledge), 
                         width = 0.4, colour="darkgrey", alpha=0.9, size=0.5) +
          scale_fill_distiller(palette = "RdYlBu", direction = 1) +
          theme_minimal() + 
          xlab('')+ ylab('')+
          coord_flip()+
          facet_wrap(vars(FuncGroup), scales = "free_x")
```

#By 8 main groups (phylogenetic unification)

```
data <- read.csv("IDEA_dataframe2.csv", sep=";")
head(data)




data1 <- data %>%
  group_by(Shortfall, Domain, FuncGroup) %>%
  summarize(BestEstimateR = mean(BestEstimateR), 
            MinKnowledge = mean(MinKnowledge), 
            MaxKnowledge = mean(MaxKnowledge),
            ConfExpert = mean(ConfExpert),
            CredExpert = mean(CredExpert))
head(data1)



listShortfall <- c("ELTONIAN","HUTCHINSONIAN","RAUNKIAERAN",
                   "DARWINIAN","PRESTONIAN","WALLACEAN","LINNEAN")                

# Order FuncGroup By domain
data1$FuncGroup <- factor(data1$FuncGroup, 
                          levels = c("Aves","Mammalia","Free living invertebrates","Parasite",
                                     "Bryophytes" ,"Vascular",
                                     "Lichenic fungi", "Non-lichenic fungi and yeast",
                                     "Algal mix","Archaea","Bacteria" ,"Protozoans","Virus"))


data2 <- data1 %>% mutate(PhyloGroup = case_when(
  FuncGroup == "Aves" ~ "Vertebrates",
  FuncGroup == "Mammalia" ~ "Vertebrates",
  FuncGroup == "Parasite" ~ "Invertebrates",
  FuncGroup == "Vascular" ~ "Embryophytes",
  FuncGroup == "Bryophytes" ~ "Embryophytes",
  FuncGroup == "Free living invertebrates" ~ "Invertebrates",
  FuncGroup == "Non-lichenic fungi and yeast" ~ "Fungi",
  FuncGroup == "Lichenic fungi" ~ "Fungi",
  FuncGroup == "Bacteria" ~ "Procaryots",
  FuncGroup == "Archaea" ~ "Procaryots",
  FuncGroup == "Algal mix" ~ "Algal mix",
  FuncGroup == "Virus" ~ "Virus",
  FuncGroup == "Protozoans" ~ "Protozoans")
  )

unique(data1$FuncGroup)

unique(data2$PhyloGroup)

data3 <- data2 %>%
  group_by(Shortfall, Domain, PhyloGroup) %>%
  summarize(BestEstimateR = mean(BestEstimateR), 
            MinKnowledge = mean(MinKnowledge), 
            MaxKnowledge = mean(MaxKnowledge),
            ConfExpert = mean(ConfExpert),
            CredExpert = mean(CredExpert))
head(data3)


# Order FuncGroup By domain
data3$PhyloGroup <- factor(data3$PhyloGroup, 
                          levels = c("Vertebrates", "Invertebrates", "Embryophytes", "Fungi", "Algal mix", "Protozoans", "Procaryots", "Virus"))

unique(data3$Shortfall) 

ggplot(data3) +
  aes(x = factor(Shortfall, level=listShortfall), 
      y = BestEstimateR, fill = BestEstimateR) +
  geom_col() +
  geom_errorbar(aes(factor(Shortfall, level=listShortfall), 
                    ymin=MinKnowledge, ymax=MaxKnowledge), 
                width = 0.4, colour="darkgrey", alpha=0.9, size=0.5) +
  scale_fill_distiller(palette = "RdYlBu", direction = 1) +
  theme_minimal() + 
  xlab('')+ ylab('')+
  coord_flip()+
  facet_wrap(vars(PhyloGroup), scales = "free_x", ncol = 4, nrow = 2)
```
