"# longitudinal_microbiome_analysis" 

# 



## Longitudinal microbiome 분석 방법 

### Alpha diverseity 검정 
- 통계검정: 
  - Linear mixed mode (`lme4`)
  - generalized mixed model (`glmmTMB`)
  - GAM: 비선형 시간 변화
  - GEE: repeated/clustered average effect

### Beta diversity 
- 통계검정 
  - Group*Time 혼합구조 PEMANOVA (`vegan::adonis2`)
  - Group*Time*repeated measure PEMANOVA + strata 
  
### 시간에 따른 biomarker
- 통계검정
  - ANCOM-BC2
  - MsAsLin3

  - Mixed model

### 어느 시점에서 달라지는가?
- 통계검정
  - GAM
  - MetagenomeSeq::filTimeSeries
  
  



# Reference
- Kodikara S et al. 2022.  Statistical challenges in longitudinal microbiome data analysis. *Briefings in bioinformatics*. https://doi.org/10.1093/bib/bbac273
- Lyu, R et al. 2023. Methodological Considerations in Longitudinal Analyses of Microbiome Data: A Comprehensive Review. *Genes*. https://doi.org/10.3390/genes15010051