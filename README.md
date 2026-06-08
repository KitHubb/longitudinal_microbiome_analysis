# longitudinal_microbiome_analysis

이 글의 목적은 종단적 마이크로바이옴 데이터의 분석 법을 학습 하고자 작성하였음.

최종 목적은 다음과 같음

1)  아토피 종단 코호트 데이터 분석

2)  SimpleMicrobiome R package 에서 종단 데이터 분석 기능 추가를 위한 선행 분석

------------------------------------------------------------------------------------------------------

26-06-08

-   <https://www.youtube.com/watch?v=K3gzz5DGTc0&t=682s>이거 보고 있었음 ㅇㅇ

\| 봐야 하는 것

\| 확인 한 자료

------------------------------------------------------------------------------------------------------

## 1. 종단 연구란?

### 1) 개념

-   Employ continuous or repeated measures from an initial sample

-   Follow individuals (or communities) over a prolonged period of time

-   Generally observational in nature, with quantitative and/or qualitative data being collected on any combination of exposures and outcomes

-   Reveal complex patterns of change

[ref](ttps://www.youtube.com/watch?v=cMRc2KiZZdk)

### 2) 종단 연구의 특징

(1) 장점

-   Identify and relate events to exposures or treatments 언제 그 이벤트가 일어났는지?

-   Define these exposures with regards to time (How long do they last?, When do they happen?, etc.) 얼마나 지속되는지? 등 추가 적으로 알 수 있음

-   Establish sequence of events and patterns

-   Measure individual subject variability against the cohort

-   Provides information on microbial community development, stability, response, and recovery

[ref.1](Caruana,%20E.%20J.,%20Roman,%20M.,%20Hernández-Sánchez,%20J.,%20&%20Solli,%20P.%20(2015).%20Longitudinal%20studies.%20Journal%20of%20thoracic%20disease,%207(11),%20E537–E540.%20https://doi.org/10.3978/j.issn.2072-1439.2015.10.63) [ref.2](ttps://www.youtube.com/watch?v=cMRc2KiZZdk)

(2) 단점

-   Loss of participants over time - can reduce representative nature of the cohort 시간에 따라 참가자의 수가 하락할 수 있음

-   Difficulty in assessing the impact of exposure to outcome 노출의 결과를 측정하기가 어려움

-   Deeper understanding of statistical testing is needed to avoid inaccurate conclusions 통졔적으로 이론에 빠삭해야 함..

-   Increased time commitment and financial demands 시간이 갈 수록 연구비가 ㅠㅠ

### 3) 종단 연구의 실험설계

우리가 고려해야 하는 것은?

-   어떤 간격으로 측정해야 하는가?

-   어떤 결과를 측정해야 하는가?

-   등등

### 4) 종단 연구 수행 시 원칙 

1.  Methods of data collection and recording must be standardized and consistent over time 데이터 수집 및 기록 방법은 시간에 걸쳐 표준화되고 일관되게 유지되어야 한다.
    -   연구 시작부터 종료까지 동일한 방법으로 데이터를 수집해야 함

    -   마이크로바이옴 연구의 경우 동일한 body site, 동일한 sampling protocol 사용
2.  Frequency and degree of sampling should vary according to the specific research goals 샘플링 빈도와 강도는 연구 목적에 맞게 결정되어야 한다.
    -   너무 길게도, 너무 짧게 구성해서도 안된다.

    -   연구 질문에 따라 채취 주기를 설계해야 함
3.  Effort should be made to ensure maximal retention of participants 가능한 한 많은 참여자가 연구 종료 시점까지 남아 있도록 노력해야 한다.
    1.  종단 연구의 가장 큰 문제 중 하나는 탈락률(dropout) 시간이 지나면서 참여자가 중도 이탈할 수 있음
4.  Implementation of longitudinal research projects can require an extensive amount of time - plan accordingly! 종단 연구는 매우 오랜 시간이 필요할 수 있으므로 충분한 계획이 필요하다.
    -   연구 기간이 수개월\~수년 이상인 경우가 많음 예산, 인력, 시료 보관, 데이터 관리 등을 장기적으로 계획해야 함

    -   연구자가 졸업하거나 연구비가 종료되는 상황도 고려해야 함
5.  Perform statistical analyses with care to avoid misrepresentation of data 데이터를 잘못 해석하지 않도록 통계 분석을 신중하게 수행해야 한다.
    -   종단 데이터는 동일한 사람에게서 반복 측정된 자료이므로 독립 표본이 아님

    -   다음과 같은 종단 데이터 맞춤 통계 방법을 사용하여야 함

        -    Repeated measures ANOVA

        -    Linear mixed model (LMM)

        -    Generalized linear mixed model (GLMM)

        -    Longitudinal trajectory analysis

[ref](https://www.equator-network.org/reporting-guidelines/consort-routine/)

## 2. 종단 데이터 분석의 어려움

### 1) Microbiome 데이터의 특징

(1) Compositionality (조성성)

-   상대적인 비율을 사용, 특정 군이 증가하면 다른 군이 감소, 서로 영향을 미침

(2) Sparsity(희소성) 과 Zero-inflated

-   데이터의 70\~90%가 0으로 이루어져 있음

### 2) 시계열 분석 시 고려해야 하는 마이크로바이옴 데이터의 특징

(1) within-subject Correlation (개체 내 상관)
    -   동일 샘플의 반복 정의 경우, 독립성 가정을 충족하지 않는다

    -   이때, 개인 간 변이와 개인 내 변이를 분리해야 함
(2)  Fixed and dynamic/random effects 를 고려해야 한다
    -   Fixed effect: 모든 사람에게 공통적으로 적용되는 효과

        -   e.g. Time, Sex, Treatment

    -   Random effect: 개인마다 다른 효과 (=개인 간 차이)
(3)  Differences between time intervals; missing data 측정 간격의 차이와 결측치 고려
    -   불규칙한 간격과 누락데이터로 특정 통계 방법의 사용이 어려움을 확인

## 2. 종단 연구 통계 방법 

### 1) ANOVA & MANOVA

### Repeated Measures ANOVA

#### Multivariate ANOVA

### 2) Mixed-effect regression models

#### Linear Mixed Model (LMM)

### 3) Generalized Estimating Equations (GEE)

### 4) Generalized Linear Mixed Model (GLMM)

## 자인 

### 1) L1

### 2) Cohort 연

## 3. Longitudinal microbiome 분석 방법

### Alpha diverseity 검정

-   통계검정:
    -   Linear mixed mode (`lme4`)
    -   generalized mixed model (`glmmTMB`)
    -   GAM: 비선형 시간 변화
    -   GEE: repeated/clustered average effect

### Beta diversity

-   통계검정
    -   Repeated-measures PERMANOVA (strata = SubjectID)
        -   Group\*Time 혼합구조 PEMANOVA (`vegan::adonis2`)
    -   Group*Time*repeated measure PEMANOVA + strata

### 시간에 따른 biomarker

-   통계검정
    -   ANCOM-BC2

    -   MsAsLin3

    -   Mixed model

### 어느 시점에서 달라지는가?

-   통계검정
    -   GAM
    -   MetagenomeSeq::filTimeSeries

## 4. 종단 데이터의 시각화

### 1) volatility plot

-   목적
    -   특정 taxqa 혹은 alpha diversity

# - Reference -

## Review Article

-   Kodikara S et al. 2022. Statistical challenges in longitudinal microbiome data analysis. *Briefings in bioinformatics*. <https://doi.org/10.1093/bib/bbac273>
-   Lyu, R et al. 2023. Methodological Considerations in Longitudinal Analyses of Microbiome Data: A Comprehensive Review. *Genes*. <https://doi.org/10.3390/genes15010051>

## Research Article

Microbiome 연구에서는 FMR나 infant gut microbiome연구에서 자주 접할 수 있고, Top 저널에서도 많이 보임

-   Bokulich, NA et al. (2016). Antibiotics, birth mode, and diet shape microbiome maturation during early life. *Science translational medicine*, *8*(343), 343ra82 (citation =1,800)

    -   1달 간격으로 총 6m 측정

    -   Vaginal birth vs Cesarean birth / Formular-fed vs Breast-fed

    -   결과

        -   언제 미생물 군집 조성이 달라지는지?

    -   alpha diversity 의 차이에 집중

-   

## Youtube

-   QIIME2:
    -   <https://www.youtube.com/watch?v=cMRc2KiZZdk>
    -   <https://www.youtube.com/watch?v=K3gzz5DGTc0&t=682s>

## 분석 도구

1)  QIIME2: q2-longitudinal
    -   homepage: <https://docs.qiime2.org/2024.10/tutorials/longitudinal/>

    -   article: <https://pmc.ncbi.nlm.nih.gov/articles/PMC6247016/>

    -   지원하는 기능

        -   Linear Mixed Effect

        <!-- -->

        -   ANOVA

        -   Pairwise distance/difference

        -   Maturity Index Prediction

            -   ["시간에 따라 군집이 정상 상태로 회복되는가?"]{.underline}

                -   언제 성숙하는가? (포화에 다 다르는가? 언제 군집의 더 이상 변하지 않는가?, 언제 변동석이 적어지는가?)

            -   [Persistent gut microbiota immaturity in malnourished Bangladeshi children]{.underline} (Subramanian et al., 2014)

            -   

        -   Non-parametric Microbial interdependence test(NMIT)

            -   **"시간에 따라 미생물 네트워크 구조가 재구성되는가?"** = 각 종들의 상호관계(network)가 시간에 따라 어떻게 유지되는가?

            -   [A multivariate distance-based analytic framework for microbial interdependence association test in longitudinal study]{.underline} (Zhang et al., 2017)

            -   
