# Survival Analysis and Demographic Tables

This project performs survival analysis and generates demographic tables based on various groups in the dataset.

## Table of Contents
- [Description](#description)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Files](#files)
- [Contributing](#contributing)
- [License](#license)

## Description

The project includes two main parts:
1. **Survival Analysis**: 
   - Generates survival curves based on different age categories.
   - Uses the `survfit` function from the `survival` package and `ggsurvfit` for plotting.
   - Adds risk tables to the plots for enhanced analysis.
   
2. **Demographic Tables**:
   - Summarizes demographic data and statistical measures for various groups.
   - Outputs demographic data into CSV files.

## Requirements

- R (version 4.0 or higher)
- R packages:
  - `survival`
  - `ggsurvfit`
  - `dplyr`

## Installation

1. Install R from [CRAN](https://cran.r-project.org/).
2. Install the required R packages by running:
    ```R
    install.packages(c("survival", "ggsurvfit", "dplyr"))
    ```

## Usage

1. **Survival Analysis**:
   - The script generates survival plots for different age groups based on the `AHI_cat3` variable.
   - To run the survival analysis, execute the code snippet:
     ```R
     categories <- levels(data00_drop_CPAPpre_drop_nSACPAP$AHI_cat3)
     categories_names <- c("n-SA", "m-SA", "s-SA")
     colors <- c("black", "black", "black")
     linetype <- c("solid", "dotdash", "dotted")

     x <- data00_drop_CPAPpre_drop_nSACPAP[which(data00_drop_CPAPpre_drop_nSACPAP$Age_cat == 1),]
     mdl <- survfit(Surv(TtoD/365.25, DeceasedFlag) ~ AHI_cat3, data = x)
     plot1 <- mdl %>%
       ggsurvfit(size=1.2) +
       ggtitle("Young adults (all)") +
       theme(plot.title=element_text(hjust=0.5)) +
       xlab("Time to Death(years)") +          
       ylab("Survival Probability (%)") +  
       ylim(0.98, 1) +
       scale_linetype_manual(values=linetype, labels=categories_names) +
       theme(axis.text.x = element_text(size = 12, face="bold"),
             axis.text.y = element_text(size = 12, face="bold")) +
       geom_line(aes(linetype=linetype))
     plot1 + add_risktable(
       risktable_stats = c("{n.risk} ({round(estimate*100,2)})%)"),
       stats_label = c("At Risk (Survival %)"),
       size=5,
       face="bold"
     )
     ```

2. **Demographic Tables**:
   - The script generates summary tables for different demographic categories and groups.
   - To create and export the tables, run:
     ```R
     Table_1_All <- data00_drop_CPAPpre_drop_mSA_drop_nSACPAP %>% group_by(all) %>% summarise(
       n = n(),
       Age_M_SD = paste(round(mean(Age, na.rm = TRUE), 2), "(", round(sd(Age, na.rm = TRUE), 2), ")"),
       Young_N_P = paste(round(length(which(Age_cat == 1)), 2), "(", round(length(which(Age_cat == 1)) / n() * 100, 2), ")"),
       Middle_aged_N_P = paste(round(length(which(Age_cat == 2)), 2), "(", round(length(which(Age_cat == 2)) / n() * 100, 2), ")"),
       Older_adults_N_P = paste(round(length(which(Age_cat == 3)), 2), "(", round(length(which(Age_cat == 3)) / n() * 100, 2), ")"),
       BMI_M_SD = paste(round(mean(BMI, na.rm = TRUE), 2), "(", round(sd(BMI, na.rm = TRUE), 2), ")"),
       BMI30_N_P = paste(round(length(which(BMI >= 30)), 2), "(", round(length(which(BMI >= 30)) / n() * 100, 2), ")"),
       Death_N_P = paste(round(length(which(DeceasedFlag == 1)), 2), "(", round(length(which(DeceasedFlag == 1)) / n() * 100, 2), ")"),
       CCI_M_SD = paste(round(mean(CCI, na.rm = TRUE), 2), "(", round(sd(CCI, na.rm = TRUE), 2), ")"),
       CCI2_N_P = paste(round(length(which(CCI >= 2)), 2), "(", round(length(which(CCI >= 2)) / n() * 100, 2), ")"),
       WHITE_N_P = paste(round(length(which(Race_cat == 1)), 2), "(", round(length(which(Race_cat == 1)) / n() * 100, 2), ")"),
       BLACK_N_P = paste(round(length(which(Race_cat == 2)), 2), "(", round(length(which(Race_cat == 2)) / n() * 100, 2), ")"),
       OTHERS_N_P = paste(round(length(which(Race_cat == 3)), 2), "(", round(length(which(Race_cat == 3)) / n() * 100, 2), ")"),
       HISPANIC_N_P = paste(round(length(which(Ethnicity_cat == 1)), 2), "(", round(length(which(Ethnicity_cat == 1)) / n() * 100, 2), ")"),
       Male_N_P = paste(round(length(which(Sex == 1)), 2), "(", round(length(which(Sex == 1)) / n() * 100, 2), ")"),
       Insomnia_N_P = paste(round(length(which(Insomnia == 1)), 2), "(", round(length(which(Insomnia == 1)) / n() * 100, 2), ")"),
       Card_N_P = paste(round(length(which(Card == 1)), 2), "(", round(length(which(Card == 1)) / n() * 100, 2), ")"),
       Met_N_P = paste(round(length(which(Met == 1)), 2), "(", round(length(which(Met == 1)) / n() * 100, 2), ")"),
       Neuro_N_P = paste(round(length(which(Neuro == 1)), 2), "(", round(length(which(Neuro == 1)) / n() * 100, 2), ")"),
       Psy_N_P = paste(round(length(which(Psy == 1)), 2), "(", round(length(which(Psy == 1)) / n() * 100, 2), ")"),
       Ren_N_P = paste(round(length(which(Ren == 1)), 2), "(", round(length(which(Ren == 1)) / n() * 100, 2), ")"),
       Pulm_N_P = paste(round(length(which(Pulm == 1)), 2), "(", round(length(which(Pulm == 1)) / n() * 100, 2), ")"),
       CPAP_N_P = paste(round(length(which(CPAP == 1)), 2), "(", round(length(which(CPAP == 1)) / n() * 100, 2), ")"),
       CPAP_post_N_P = paste(round(length(which(CPAP_post == 1)), 2), "(", round(length(which(CPAP_post == 1)) / n() * 100, 2), ")"),
       Tfoll_M_SD = paste(round(mean(Tfoll, na.rm = TRUE), 2), "(", round(sd(Tfoll, na.rm = TRUE), 2), ")")
     )

     Table_1_AHI <- data00_drop_CPAPpre_drop_mSA_drop_nSACPAP %>% group_by(AHI_cat) %>% summarise(
       n = n(),
       Age_M_SD = paste(round(mean(Age, na.rm = TRUE), 2), "(", round(sd(Age, na.rm = TRUE), 2), ")"),
       Young_N_P = paste(round(length(which(Age_cat == 1)), 2), "(", round(length(which(Age_cat == 1)) / n() * 100, 2), ")"),
       Middle_aged_N_P = paste(round(length(which(Age_cat == 2)), 2), "(", round(length(which(Age_cat == 2)) / n() * 100, 2), ")"),
       Older_adults_N_P = paste(round(length(which(Age_cat == 3)), 2), "(", round(length(which(Age_cat == 3)) / n() * 100, 2), ")"),
       BMI_M_SD = paste(round(mean(BMI, na.rm = TRUE), 2), "(", round(sd(BMI, na.rm = TRUE), 2), ")"),
       BMI30_N_P = paste(round(length(which(BMI >= 30)), 2), "(", round(length(which(B
