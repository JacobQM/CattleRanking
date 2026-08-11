# ABOUT

Project: Ranking Cattle Vaccine Candidates for Neosporosis Using Machine Learning and Consensus Algorithms

Purpose: This repository contains the data, notebooks, results, and documentation for a capstone research project that develops, compares, and validates individual and aggregation ranking models to prioritise vaccine candidate proteins for Neosporosis in cattle. The project focuses on combining multiple Multi-Criteria Decision-Making (MCDM) and consensus aggregation methods (VIKOR, EDAS, SPOTIS, Ranky, Borda, Copeland, MC4/MCT) and on validating those rankings using cross-validation, bootstrapping, Kemeny distance and conformal prediction methods (MAPIE, PUNCC).

Audience: Computational biologists, bioinformaticians, veterinary researchers, and data scientists interested in ranking/aggregation methods and vaccine candidate prioritisation without a predefined ground truth.

Repository structure (quick):
- notebooks/ — Jupyter notebooks (DataPrep.ipynb and analysis notebooks)
- data/raw/ — original datasets and raw CSV/Excel inputs
- data/processed/ — cleaned and normalized datasets used for ranking
- models/ — model artifacts and model-related notebooks/scripts
- results/ — spreadsheets and result artifacts (rankings, analysis, cross-validation outputs)
- figures/ — PNGs and visual artifacts
- docs/ — README, INDEX and this paper
- output/ — generated exports and other outputs

How to use this file: This document is the capstone research paper and includes an abstract, introduction, methods, results, discussion, conclusion and references. Read the ABOUT and Repository structure above to locate the code and data used to create each figure and table.

---

# Research Paper: Capstone

# Ranking Cattle Vaccine Candidates for Neosporosis Using Machine Learning and Consensus Algorithms

Jacob Ronald Vuong Bayne  
14328076  
Supervisor: Paul Kennedy  
Co-supervisor: John Ellis  
Capstone - Research  
University of Technology Sydney  

---

# Abstract

Livestock cattle health is critical to global agriculture and the economy, with diseases like Neosporosis posing significant threats due to reproductive issues they cause in cattle. Traditional vaccine development methods are often time-consuming and may overlook subtle data patterns crucial for candidate prioritisation. This study addresses the challenge of validating consensus ranking models for vaccine candidates in the absence of a 'golden standard' benchmark. This study applied machine learning (ML) algorithms, specifically individual and aggregation ranking models, to prioritise Neosporosis vaccine candidates based on selected protein attributes. Individual models included VIKOR, EDAS, SPOTIS, Ranky Score, and Ranky Rank, while aggregation methods encompassed Borda's Count, Copeland's Method, and Markov Chain-based models (MC4 and MCT). A comprehensive validation framework was implemented, utilising toy dataset testing, correlation analysis, Kemeny's distance, consensus-based cross-validation, bootstrapping, and conformal prediction methods (MAPIE and PUNCC) to assess model reliability and stability. Results indicated that while individual models effectively identified top candidate proteins, aggregation models demonstrated higher consistency and robustness, with Borda's Count and Copeland's Method showing near-perfect alignment in rankings. Validation methods confirmed the reliability of the aggregation models in synthesising complex data inputs into stable rankings, despite the absence of a predefined benchmark. The study highlights the potential of ML-based ranking models in vaccine candidate prioritisation but also underscores the need for tailored validation strategies specific to unsupervised ranking systems. Recommendations include adopting the developed validation framework, utilising multiple decision-making models, validating aggregation models with Kemeny's distance and correlation analysis, validating individual models with bootstrapping methods, developing specialised validation frameworks, optimising computational efficiency, integrating domain expertise, and extending the approach to other diseases. This research contributes to the field by providing a robust framework for vaccine candidate evaluation, leveraging ML techniques to enhance cattle health and support agricultural productivity, while addressing the critical challenge of validation without a 'golden standard'.

# 1. Introduction

Animal health is central to agriculture and the world economy since livestock cattle provide necessary products such as meat, milk and dairy besides supporting people’s livelihoods and other sectors related to farming. Neosporosis, a parasitic disease that leads to reproductive issues such as forced abortions, poses a significant threat to livestock. This disease can severely compromise cattle health and disrupt food production, affecting global economies and the overall food supply. As such, combating Neosporosis with a vaccine is a crucial step for maintaining agricultural productivity and economic stability

Research efforts in reverse vaccinology and immuno-informatics to tackle Neosporosis have identified potential protein targets for vaccines. However, translating these findings into effective vaccines is challenging due to the time and resources required for traditional vaccine candidate prioritisation methods, which can also miss critical data patterns. This makes the process of determining vaccine eligibility difficult and inefficient. Machine Learning (ML) algorithms offer promising solutions by analysing complex datasets and identifying subtle patterns. Consensus ranking methods can combine insights from multiple algorithms for more robust vaccine rankings, while uncertainty calibration methods like conformal prediction help quantify prediction confidence, reducing biassed or inaccurate rankings.

This research is of significant interest to biologists aiming to develop effective vaccines, agricultural economists studying the economic impacts of livestock diseases, and livestock farmers seeking improved cattle health and productivity through effective vaccination strategies. A key component of this research is the validation of the proposed ML-based ranking framework. While ML can generate powerful predictions, the reliability of these predictions must be rigorously tested. This research will employ advanced validation techniques to assess the robustness and accuracy of the ranking methods. However, a significant challenge arises in this context, leading to our primary research question:

How can we test the reliability and validate a consensus ranking model when there is no 'golden standard' to compare it against?

This question is crucial because, unlike many ML applications where a clear benchmark or ground truth exists, vaccine candidate selection often lacks a definitive standard for comparison. The absence of a 'golden standard' necessitates innovative approaches to validation that can provide confidence in the rankings without relying on predetermined correct answers.

To address this challenge, this study aims to leverage ML techniques, particularly aggregation ranking methods, to improve the accuracy and reliability of Neosporosis vaccine candidate prioritisation. By integrating diverse data sources and applying rigorous validation processes, this research will develop a robust framework for evaluating vaccine candidates, ultimately contributing to improved cattle health and agricultural productivity.

# 2. Literature Review

Machine learning (ML) has increasingly demonstrated its effectiveness in biological tasks, particularly in domains like vaccine development, where vast datasets must be efficiently prioritised or ranked. For instance, ML can streamline the prioritisation of vaccine candidates by processing complex datasets involving protein structures, immune responses, and pathogen interactions, reducing the need for resource-intensive traditional methods (Goodswen et al., 2013). These ML models improve ranking accuracy by integrating diverse biological data types, which enhances the reliability of predictions. Similarly, a review on computational vaccine research highlights how AI and ML are transforming every step of vaccine development, from creation to dissemination (Thalange et al., 2024). The authors note that "AI and ML anticipate and discover ideal antigen targets and immunogenic epitopes, expediting antigen selection". The ability of AI and ML to enhance efficacy in vaccine production and distribution is revolutionising the field, helping to accelerate vaccine discovery and improve public health outcomes. Ultimately, the integration of ML ranking models in my research on Neosporosis vaccine candidate selection holds the potential to develop a robust, data-driven framework for prioritisation, enhancing accuracy and efficacy in real-world applications.

ML applications for ranking algorithms can be categorised into individual ranking models and aggregation models. Individual models process raw data directly, each using unique strategies to produce a single ranking. Aggregation models, or consensus ranking methods, combine outputs from multiple individual models to create a consolidated ranking. While individual models may capture nuanced data aspects, aggregation models aim to reconcile conflicting rankings. This variability necessitates exploring different methods to identify the most effective approach for vaccine candidate ranking in veterinary healthcare and livestock management.

Multi-Criteria Decision-Making (MCDM) methods are individual models which are essential tools for evaluating and ranking alternatives based on multiple, often conflicting criteria. In the context of ranking vaccine candidates for Neosporosis in cattle, MCDM methods such as TOPSIS, VIKOR, and EDAS have been employed to process complex protein attribute data effectively. The Technique for Order Preference by Similarity to Ideal Solution (TOPSIS) ranks alternatives based on their proximity to an ideal solution and has been utilised in medical applications like detecting COVID-19 from cough sounds (Chowdhury et al., 2022). VIKOR focuses on finding a compromise solution among conflicting criteria and has been applied in multi-target feature selection problems (Hashemi et al., 2021). Similarly, the Evaluation Based on Distance from Average Solution (EDAS) method evaluates alternatives based on their desirability and non-desirability by measuring their distance from an average solution, aiding in the selection of machine learning algorithms for diabetes detection (Sharma et al., 2021). Each of these methods offers unique advantages in ranking vaccine candidates by leveraging the multi-attribute nature of protein structures, although they may have limitations in fully capturing the interactions between attributes, but through use of multiple individual methods it will allow

for a more holistic evaluation by capturing diverse perspectives and compensating for the limitations of individual methods, ultimately enhancing the robustness and reliability of the vaccine candidate rankings.

Aggregation models also need to be examined to determine the relevance for this study. Borda’s aggregation model works by assigning points to each candidate based on their rank position across various individual models. The candidate with the highest total points is ranked first. This method is appealing due to its simplicity, ensuring that all criteria contribute equally to the final decision. In the context of vaccine candidate ranking, Borda’s method offers a straightforward way to balance multiple protein attributes, which is critical when the significance of each attribute might not be immediately clear (Boehmer et al., 2023). The method has proven effective in situations where reconciling diverse ranking sources is necessary, such as recommender systems (Tang et al., 2016). However, Borda’s method may not account for the varying importance of protein attributes in Neosporosis vaccine candidate ranking, as it treats all criteria equally. Despite this, its simplicity and computational efficiency make it a strong candidate for this project, where ease of implementation and balanced aggregation are prioritised (Burkovski, 2014). Copeland’s method is another effective aggregation model that uses pairwise comparisons. Each candidate is compared against every other candidate, and points are awarded for each "win." The candidate with the most points, based on these pairwise wins, ranks highest (Al-Sharrah et al., 2010). Copeland’s method is particularly useful when you want to ensure a candidate performs consistently well across various attributes, which is essential in identifying a robust Neosporosis vaccine candidate (Lestari et al., 2018). This method allows the final ranking to reflect a consensus that prioritises candidates excelling across multiple dimensions, even when individual attributes may conflict. While Copeland’s method may have issues with ties or may struggle to highlight the impact of particularly critical attributes, its ability to account for all pairwise comparisons makes it well-suited for selecting cattle vaccination candidates, where consistency across protein attributes is important (Burkovski et al., 2014). Its computational feasibility makes it a reasonable choice for the project. MC4 is a more sophisticated aggregation method that uses Markov Chains to simulate probabilistic transitions between ranks based on candidates’ performance in individual models. Over time, these transitions stabilise, yielding a final, consensus ranking (Fang et al., 2011). The strength of MC4 lies in its ability to capture long-term patterns and interactions between candidates, making it ideal for complex datasets where protein attributes may influence each other in subtle ways. This makes MC4 highly relevant to vaccine candidate ranking, where the relationships between different protein features can impact overall candidate suitability (Tian et al., 2018). While MC4’s complexity requires significant computational power, its ability to provide a nuanced and stable consensus ranking is valuable in ensuring that the best Neosporosis vaccine candidates are identified with a high degree of confidence (Okwuashi et al.,2021).

Despite the increased computational cost, MC4’s benefits in handling complex interactions and generating stable rankings justify its use in this project, where accuracy and robustness are key. The Kemeny-Young method, although known for producing optimal consensus rankings, is computationally infeasible for large datasets like those used in vaccine ranking. This method minimises the disagreement between individual rankings, producing a ranking that is as close as possible to each input ranking (Srivastava et al., 2023). However, its NP-hard complexity makes it unsuitable for projects involving thousands of data points, as is the case with Neosporosis vaccine candidate selection (Hamm et al. 2021). Given the computational limitations and the need for an efficient method that can handle large datasets in a reasonable time frame, Kemeny-Young is not feasible for this project (Andrieu et al, 2023).

## 2.1 Gap in the Research

In the field of vaccine eligibility ranking, common practice has relied on supervised learning methods, with Conformal Prediction being a primary validation model. However, sole reliance on these models may not be able to fully capture the robustness of individual and aggregated rankings, such as Neosporosis vaccine candidate prioritisation. Moreover, there are limitations in possible validation frameworks that could address uncertainty and variability in unsupervised ranking models.

Parvandeh et al. (2020) introduced consensus nested cross-validation (cnCV) to improve feature stability in supervised learning tasks by integrating differential privacy principles into a nested cross-validation (nCV) framework. While traditional nCV mitigates overfitting through inner and outer folds that select features and tune parameters, it often results in the inclusion of irrelevant features and high computational costs. In contrast, cnCV emphasises feature stability by identifying the most reliable features across folds, rather than focusing solely on classifier accuracy. Although cnCV was developed specifically for feature selection in supervised tasks, its focus on consistency across folds offers a useful foundation for evaluating stability in unsupervised ranking models. Applying cnCV’s cross-fold consensus approach in our context could allow us to assess whether an aggregation model maintains stable rankings when presented with varied data subsets, ensuring robustness without a direct dependence on classifier accuracy. This adaptation would involve holding out or adjusting ranking inputs across folds and observing consistency in the aggregated outputs, aligning with cnCV’s principle of achieving stability through cross-fold consensus.

Kemeny’s distance, another potential metric in ranking validation, focuses on measuring the level of disagreement between rankings by quantifying pairwise discrepancies. According to Lederer (2024), Kemeny’s distance aims to identify the distance between rankings by counting the number of pairwise disagreements. This metric is particularly effective in contexts where achieving consensus is important, as it provides a quantitative assessment of how well an

aggregated ranking aligns with individual input rankings. In the context of vaccine candidate ranking, Kemeny’s distance serves as an ideal measure for validating the alignment between aggregated and individual rankings, ensuring that the chosen aggregation reflects collective preferences accurately.

In a different approach, Kumar et al. (2024) utilised bootstrapping to assess the stability of rankings generated by the TOPSIS (Technique for Order of Preference by Similarity to Ideal Solution) method when applied to GPU performance metrics. By generating multiple bootstrap samples, they examined the consistency of TOPSIS rankings across resamples, providing confidence intervals for each rank. This approach allowed Kumar et al. to quantify the variability and robustness of rankings for different GPU compute instances, assessing stability by calculating the standard deviation of each rank. Bootstrapping’s capacity to evaluate stability without relying on traditional validation methods makes it particularly useful for measuring rank consistency in unsupervised tasks, such as prioritising vaccine candidates.

Conformal prediction methods have also been adapted for validation purposes, with tools like MAPIE (Model Agnostic Prediction Interval Estimator) and PUNCC (Prediction Uncertainty via Non-Conformal Confidence) offering robust mechanisms for estimating prediction intervals. Cordier et al. (2023) implemented MAPIE with a RandomForestRegressor, providing confidence intervals that help evaluate the range within which true ranks are likely to fall. This approach was shown to provide interpretable uncertainty estimates, enhancing model reliability by quantifying variability in predictions. Similarly, Mendil et al. (2023) explored PUNCC for generating prediction intervals with non-conformal confidence scores, enabling a rigorous assessment of uncertainty across multiple folds. Both methods are particularly effective in supervised contexts, and while their direct application to unsupervised ranking may be challenging, they provide essential insights into uncertainty calibration and rank prediction.

This study aims to bridge this gap by developing and validating unsupervised ranking models for Neosporosis vaccine candidates. By introducing advanced validation techniques - including toy data noise introduction, sensitivity analysis, consensus-based cross-validation, bootstrapping, correlation - this research will offer frameworks for assessing ranking models without the need for full supervision. This approach meets the need for effective, uncertainty-calibrated ranking methods, providing a more adaptable solution for vaccine candidate prioritisation in veterinary healthcare.

# 3. Methodology

This methodology outlines an ML-based approach for ranking vaccine candidates, starting with data collection and preparation to ensure quality, followed by feature selection to identify biologically relevant attributes and reduce redundancy. Individual ranking models assess candidates across attributes, while aggregation models consolidate these into a consensus ranking. Validation methods, including toy dataset testing, correlation analysis, bootstrapping, and conformal prediction (MAPIE and PUNCC), establish model reliability and provide confidence intervals. This multi-step approach ensures robust, consistent rankings, adding a quantified layer of uncertainty that enhances the interpretability and reliability of candidate rankings.

## 3.1. Data Collection

The data used in this research is sourced from Goodswen et al. (2023) on vaccine discovery methodologies for protozoan parasites, with a focus on Toxoplasma gondii. The dataset provides an analysis of potential vaccine candidates, which is crucial for ranking Neosporosis vaccine candidates based on specific protein attributes relevant to cattle health.

## 3.2. Data Exploration

The dataset, consisting of 8,147 entries (rows) and 13 columns, pertains to the ranking and prioritisation of potential protein candidates for Neosporosis vaccine development in cattle. It is noted that there are no duplicate data points. It includes features such as MWt (Molecular Weight), Net_C (Net Charge), GRAVY (Grand Average of Hydropathicity), Cys-N (Cysteine count normalised by molecular weight), Instab (Instability Index), Sol (Solubility Score), Aliphat (Aliphatic Index), Tz_RNA, Bz_RNA, Sp_RNA, RNA_Mean (mean RNA score), T-cell Score (a measure relevant to T-cell immune response), Exposed (level of protein exposure), Linear_B (Linear B-cell epitopes score), Conf_B (confidence score for B-cell epitopes), and UnderPS (under predicted secretome classification). These attributes are essential for capturing biologically relevant properties of the proteins, informing their ranking based on various criteria relevant to vaccine efficacy.

The exploration confirms a complete dataset with no missing values, ensuring high data integrity. There also seems to be a relatively low variability and distribution across attributes, however this isn’t a concern considering the context of the study is to prioritise proteins rather than predict a target variable. Normalisation needs for consistent scaling and will need to further determine the redundancy between the features during the selection process.

*Figure 1. Statistics Extracted from KNIME*

## 3.3. Data Preparation

The initial dataset underwent a thorough cleaning process to ensure the integrity of the data. No missing values or duplicate entries were found in the dataset.

To ensure comparability across proteins of different sizes, normalisation was performed. Cysteine counts (Cys-N) were normalised by the molecular weight (MWt), making the values independent of protein length. Min-Max scaling was applied to standardise the feature set, ensuring that ALL features were scaled between 0 and 1, enhancing the performance and convergence of machine learning models.

## 3.4. Feature Selection

Guided by biologist John Ellis, the feature selection process aimed to include attributes that are biologically significant while minimising redundancy due to multicollinearity. The final set of features selected for the ranking model includes:

- Conf_B

- Tz_RNA

- Linear_B

- Bz_RNA

- T-cell Score

- Exposed

- Sol

To ensure the robustness of the model, a correlation analysis was conducted using a correlation matrix to identify highly correlated features that could potentially affect model performance. The correlation matrix was computed by calculating the Pearson correlation coefficients between each pair of features in the dataset, excluding the non-numeric 'Protein' identifier column. The following pairs of features exhibited high correlation coefficients (absolute value greater than 0.8):

*Table 1. Correlation Between Features*

Feature 1 Feature 2 Correlation Coefficient

MWt T-cell Score 0.84

Tz_RNA RNA_Mean 0.93

RNA_Mean Tz_RNA 0.93

T-cell Score MWt 0.84

Given the high correlation between these pairs, one feature from each pair was removed to reduce multicollinearity: MWt was removed due to its high correlation with T-cell Score. RNA_Mean was removed due to its high correlation with Tz_RNA. This decision was informed by both statistical analysis and biological relevance. John Ellis advised that features like GRAVY (hydrophobicity) and Net_C (net charge) contribute to Sol (solubility), and including them might introduce redundancy. Therefore, GRAVY and Net_C were excluded in favour of Sol. By removing these highly correlated features, the model focuses on attributes that provide unique information, enhancing the interpretability and predictive power of the ranking model. The final selection ensures that each feature contributes distinct biological insights relevant to vaccine candidate prioritisation against Neosporosis in cattle.

## 3.4. Individual Ranking Models

This section describes the reason for choosing the various individual ranking models and how they were applied to evaluate vaccine candidates based on selected features. These methods include Ranky’s Rank, VIKOR, EDAS, SPOTIS, and Ranky’s Score.

### 3.5.1. Ranky’s Rank

Ranky’s Rank was selected for its efficiency in ranking candidates across multiple criteria without complex transformations. It replaces raw values with ranks, assigning higher values to lower ranks. In the implementation, ranks were calculated for each feature using the Ranky library's ordinal method, and then summed to create a total rank for each candidate. The final rank was determined based on this total, with higher sums receiving better ranks. The process ensured a clear and effective ranking.

### 3.5.2. VIKOR

The VIKOR method was chosen for its ability to handle decision-making problems with conflicting criteria, focusing on ranking and selecting from a set of alternatives. It is particularly useful in situations where compromise solutions are acceptable, making it suitable for complex biological data where attributes may conflict. By finding a balance among the different criteria, VIKOR aids in identifying candidates that perform reasonably well across all attributes, ensuring a fair evaluation in vaccine prioritisation.

The VIKOR method was implemented using the PyMCDM package. Equal weighting was applied to ensure no single attribute dominated the outcome, promoting a fair and balanced assessment across all features. This approach is useful when there is no prior information to justify different weights for attributes, allowing for an unbiased comparison of candidates. By treating all features equally, the method improves the robustness and application of the rankings, ensuring consistency in evaluating vaccine candidates.

### 3.5.3. EDAS

EDAS was employed for its ability to evaluate candidates relative to an average solution, assessing both positive and negative distances from the average. This dual consideration makes it useful for distinguishing candidates that significantly outperform or underperform compared to the norm. By applying equal weighting, this method ensures that each attribute is fairly represented, which is ideal for maintaining consistency and balance in the ranking process. EDAS effectively highlights candidates that are desirable across multiple criteria, making it valuable for comprehensive vaccine candidate evaluation.

The EDAS method was implemented using the PyMCDM package. The process involved loading the dataset and converting the selected features into a matrix format. Equal weights were assigned to each feature to ensure a balanced evaluation, preventing any single attribute from dominating the rankings. The EDAS method was then applied, generating scores for each candidate based on their performance relative to the average solution. The results were compiled into a DataFrame, with protein identifiers, EDAS scores, and ranks.

### 3.5.4. SPOTIS

SPOTIS was included due to its focus on providing a stable and ordered ranking by considering the distance of each alternative from the ideal solution within predefined bounds. This method is particularly useful when the goal is to minimise the influence of outliers and ensure that all criteria contribute equally to the final decision. The equal weighting approach prevents any criterion from disproportionately influencing the final ranking, ensuring a reliable assessment across multiple features. SPOTIS's emphasis on stable ordering makes it valuable for consistent vaccine candidate prioritisation.

For the SPOTIS method, implemented using the PyMCDM package, the dataset was loaded, and selected features were converted into a matrix format. Equal weights were applied to ensure a balanced comparison across all criteria, with the bounds (min and max values) of each feature defined to determine the best and worst cases. The SPOTIS method was then applied to calculate scores, with lower scores indicating better candidates. The results were compiled into a DataFrame, including protein identifiers, SPOTIS scores, and ranks, and saved to an Excel file for further analysis.

### 3.5.5. Ranky’s Score

Ranky’s Score was utilised for its ability to convert multiple attribute values into a single composite score, simplifying the comparison of overall performance among candidates. This method effectively captures the candidates' rankings through its attribute scoring, making it a valuable model before aggregation. By providing a straightforward scoring mechanism, Ranky's Score facilitates quick identification of top-performing vaccine candidates based on the collective influence of all selected features.

For the Ranky's Score method, implemented using the Ranky library, scores were calculated for each candidate based on their overall performance across the selected features. These scores were added to the original DataFrame, and the data was sorted in descending order of the scores, with higher scores indicating better candidates. Final ranks were assigned based on this sorted list, and the results were saved to an Excel file for further analysis.

## 3.6. Aggregation Models

This section outlines the reasons for choosing various aggregation models and how they were applied to combine rankings from individual models to evaluate vaccine candidates. The aggregation methods used include Borda’s Count, Copeland’s Method, and MC4 & MCT. Each method was selected based on its ability to aggregate multiple ranking models effectively, ensuring balanced and reliable final rankings that prioritise candidates consistently performing well across criteria.

### 3.6.1. Borda’s Count

Borda’s Count was chosen for vaccine candidate ranking because it aggregates rankings from multiple models by assigning points based on positions in each ranking. This method ensures that candidates who consistently perform well across different criteria are prioritised, as it captures the overall consensus among individual models. Borda's Count is particularly useful for balancing diverse insights from various individual models to create a fair and reliable final ranking.

For the Borda’s Count aggregation, implementation started by loading the individual rankings from five models: VIKOR, EDAS, SPOTIS, Ranky’s Score, and Ranky’s Rank. The rankings were merged into a single DataFrame, ensuring that the protein identifiers were aligned across all models. The Borda count was applied to the ranking matrix using the Borda’s function from the Ranky library, which summed the ranks across the models. The final Borda ranks were calculated and added to the DataFrame.

### 3.6.2. Copeland’s Method

Copeland’s Method was selected for its capability of comparing candidates pairwise across all models, awarding points based on how often one candidate ranks higher than another. This method ensures that consistently strong candidates are prioritised by considering their relative performance in every possible pairing. Copeland's Method is useful for identifying candidates with broad consensus support, making it effective for reliable vaccine candidate prioritisation.

For the Copeland aggregation, individual rankings from the five models were loaded and merged. Pairwise comparisons were conducted across these models, and scores were computed using the Copeland method in the Ranky library by counting how often one candidate outranked another. Final Copeland ranks were assigned based on these scores.

### 3.6.3. MC4 & MCT

MC4 and MCT methods were chosen due to their ability to handle complex ranking data through probabilistic transitions, using Markov Chain models to simulate the ranking process. These methods are ideal for aggregating diverse ranking models in vaccine candidate prioritisation, as they can capture the dynamic interactions between rankings and converge towards a stable consensus. MC4 and MCT are particularly useful when dealing with large datasets and complex criteria, providing a nuanced aggregation that accounts for the probabilistic nature of ranking preferences.

For the MC4 and MCT methods, individual rankings from the same five models were merged. The mc4_aggregator function from the MC4 library was applied to aggregate the ranks using probabilistic transitions, simulating the process of moving between ranks over iterations. MC4

was implemented using a probabilistic approach to capture stable long-term patterns, with the input matrix structured to include all rankings. MCT involved setting more flexible parameters such as precision, iterations, and ergodic number for finer control over the transitions between ranks.

## 3.7. Validation Methods

The validation process is critical to ensuring the robustness, stability, and reliability of the ranking models applied to vaccine candidate selection. Various validation methods were used to assess the effectiveness of both individual and aggregated models, measuring their performance, consistency, and capacity to provide reliable outputs.

### 3.7.1. Toy Dataset Testing

To validate the individual models, a toy dataset of student grades in Math, English, and Science was created and used. Initial rankings were based on the average score per student. The validation involved removing the average score, shuffling the rankings, and applying the models to reorder students based on raw subject scores. The model would reshuffle based on its algorithm which would be compared to the “Golden Standard”. If the models correctly reconstructed the original rankings, it indicated they effectively prioritised relevant attributes, validating their reliability for use in more complex datasets, like Neosporosis vaccine candidate selection.

For aggregation, the toy dataset was ranked across five lists, four from the individual models and one substituted by a shuffled ranking. This process was repeated with different shuffled datasets to evaluate how well the aggregation models prioritised consistent rankings while filtering out the noise from inconsistent ones. This test of robustness ensured that the aggregation models were capable of handling randomness and noise while still producing reliable consensus rankings for the vaccine candidate dataset, combining the strengths of the individual models.

This method was chosen to essentially test the reliability of each model before applying them to a more complex dataset that may have different relationships between its attributes.

*Figure 2. Toy Dataset “Golden Standard”*

### 3.7.2. Correlation

Correlation analysis, specifically Spearman’s and Kendall’s rank correlations, measures the agreement between ranking models. It is crucial for validating both aggregation and individual models by ensuring that different ranking approaches produce consistent results. High correlations between methods indicate robust and reliable rankings, strengthening the validity of the models. The use of this method will also give an insight of whether the individual models are consistently capturing similar key favourable factors in its rankings, a significant reason for its use in this paper.

Using Pandas, SciPy, Seaborn, and Matplotlib, the rankings from MC4, MCT, Copeland, and Borda models were merged. Spearman’s and Kendall’s correlations were calculated, visualised using heatmaps, and printed for analysis.

### 3.7.3. Kemeney’s Distance

In this research, Kemeny’s distance was used due to it being able to quantify how well each aggregation aligns with the individual rankings, ensuring that collective preferences are accurately reflected. The calculations also facilitated the evaluation between different aggregation models, allowing us to determine which model exhibited the most consistent distribution and highest agreement with the individual methods. By counting pairwise

... (truncated due to length)
