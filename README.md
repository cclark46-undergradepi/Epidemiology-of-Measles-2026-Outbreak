# Epidemiology-of-Measles-2026-Outbreak
Epidemiology of Measles 2026 Outbreak
Disease Background Information
Measles is a highly contagious infectious disease with an R0 of 12-18, while the incidence remains low due to a widely available and effective vaccine. It has seen a resurgence in 2026. I wanted to understand how coverage predicts measles case odds while adjusting for background rates of disease.

Background
This data was formed by merging the Kindergarten Vaccine Coverage Dataset with the Measles Outbreak list from 2026 so far. Both datasets were publicly available from the CDC. Then, for epidemiologic methods and weighting, 2025 census population information was added. The final data set includes Geography (excluding South Dakota and Montana due to them no longer reporting vaccine information), the median coverage MMR estimates from the 2023-2024 and 2024-2025 school years were calculated and inputted as the median of both years under the name median_coverage, the 2026 Measles case count, and the 2025 state-level population census.

Methodology
This project was exclusively conducted in RStudio. For this project, merging the data proved to be challenging. I created a mockup Excel file, attached to the appendix of this report, of the clean data structure template and used an LLM to optimize the proper merging of the data. I independently added prevalence to the dataset to assist with weighting the regression. I independently stratified them by US region for a deeper understanding of regional background disease rates.

Analysis
I calculated vaccine effectiveness by employing the use of a binomial regression. This was conducted because the attack rate alone would not effectively weigh background disease rates. First, a 2-column success-failure matrix was created for the binomial model, where the matrix included [case count, population-case count]. The first model had some issues, most notably a very large F-statistic, F=3955, indicating overdispersion within the model. I concluded that the next logical step would be to attempt a quasibinomial model to correct this. This corrected the overdispersion issue but still introduced some variance issues. The next logical step was to address the low variance issue with median coverage, as the range of median coverage was [79.05,98.30], n=49, coefficient of variation was calculated as CV=.08903. This indicated that it was very tight and needed to be addressed. The median coverage was rescaled with Z-scores. This was successful and showed that prevalence was a dominant predictor of case odds, β=.306. The scaled coverage estimate remained flat, β=.003, meaning that 90% vaccine coverage vs 93% does not affect case odds. Demonstrating that these regional areas likely already achieved herd immunity. This is a statistical phenomenon due to the scaled coverage variable remaining flat and negative when, in fact, the coverage should be protective to case odds. Demonstrating real-world biologic effect covariate suppression. This led to the creation of the final quasibinomial model, which added the US region as a factor. Thus, the final model was Yi= β(Ymatrix)+ β(scaledcoverage)+ β(prevalence)+ β(region). Where scaled coverage was β= -.0444, and β(region south)=.586, β(region west) =.290. This model removed the covariate suppression and correctly showed that increased coverage was protective against case odds.   It also indicated that case odds increased when living in the US South. Ultimately, this project highlights the importance of continuing the goal of 95% herd immunity.

Formulas:
Coefficient of Variation = σ/μ x 100
Logistic Regression=  ln⁡(p/(1-p))=β_0+β_1 x_1+β_2 x_2+⋯+β_k x_k
Quasibinomial model with dispersion= Var(y) = φ ⋅ μ(1 – μ)







Appendix and Sources:
https://www.sciencedirect.com/science/chapter/monograph/abs/pii/B9780124071971000041
https://pmc.ncbi.nlm.nih.gov/articles/PMC3575184/
https://d.docs.live.net/4c31f61db9732ec1/Documents/data%20structure.xlsx
https://www.epirhandbook.com/en/
https://www.r-bloggers.com/2016/01/s-shaped-data-smoothing-with-quasibinomial-distribution/#google_vignette
https://pubmed.ncbi.nlm.nih.gov/28757186/
https://pmc.ncbi.nlm.nih.gov/articles/PMC2254615/
https://gemini.google.com/app
https://www.sciencedirect.com/topics/engineering/coefficient-of-variation
