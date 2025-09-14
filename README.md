# Marketing Mix Model with Mediation Analysis

## Project Overview
This project implements a well-reasoned two-stage Ridge Regression model to analyze marketing performance, treating Google Spend as a mediator between social media spend (Facebook, TikTok, etc.) and Revenue. We adopted a two-stage approach for the mediator, applied time-series cross-validation to respect the sequential data structure, and delivered actionable insights for the marketing team.

## Repository Structure
marketing_mix_model_mediation/
├── data/               # raw and processed data (ignored in git)
│   ├── raw/            
│   └── processed/      
├── notebooks/          # Jupyter notebooks for EDA and modeling
│   ├── 01_eda.ipynb    
│   └── 02_modeling.ipynb
├── src/                # modular Python code
│   ├── __init__.py     
│   ├── features.py     
│   └── modeling.py     
├── requirements.txt    # dependencies
└── README.md           # project documentation


## Key Steps & Findings

1.  **Data Preparation:** Handled weekly seasonality with `week_of_year` and `month` features. Addressed zero-spend periods by adding a constant +1 and creating binary flags. Applied log transformations to spends and other continuous variables to model diminishing returns and reduce skewness. All features were standardized.

2.  **Modeling Approach:** A two-stage Ridge Regression model was chosen.
    - **Stage 1:** Predicts `log(google_spend)` using social media spends, their activity flags, and time features.
    - **Stage 2:** Predicts `log(revenue)` using the predicted Google spend from Stage 1, along with control variables (price, promotions, email, SMS) and time features.
    - **Why Ridge?** To handle multicollinearity between marketing channels effectively.
    - **Validation:** Time-Series Cross-Validation (5 splits) was used to ensure no look-ahead bias and to evaluate out-of-sample performance.

3.  **Causal Framing:** The model structure explicitly encodes the mediator assumption. Social media drives search intent (proxied by Google Spend), which in turn drives revenue. This helps isolate the indirect effect of social campaigns.

4.  **Diagnostics:**
    - **Out-of-Sample Performance:** Average RMSE of [Your RMSE value] on log-revenue scale.
    - **Residual Analysis:** Residuals appear random over time and reasonably normally distributed, suggesting no major pattern was left unexplained.
    - **Sensitivity:** The model coefficients for `average_price` were negative and significant, indicating price elasticity. Promotion periods showed a positive lift.

5.  **Insights & Recommendations:**
    - **Google Mediation:** A significant portion of Facebook's impact is mediated through Google Search. This suggests Facebook campaigns are effective at driving brand search queries.
    - **ROAS:** The approximate mediated ROAS for Facebook is estimated at $[Your Calculated Value] for every dollar spent.
    - **Recommendation:** Continue investing in social media to build brand awareness and intent. Closely monitor the synergy between social spend and search spend. Consider increasing budget for Facebook during periods of high promotional activity, as intent is likely higher.
    - **Risks:** Collinearity between marketing channels is a known challenge, mitigated by regularization. The mediated effect is an estimate; true causal impact requires controlled experimentation.

## Setup & Reproduction
1.  **Install dependencies:** `pip install -r requirements.txt`
2.  **Run notebooks in order:**
    - `01_eda.ipynb`: Exploratory Data Analysis.
    - `02_modeling.ipynb`: Full analysis, model building, and insight generation.
3.  The results are deterministic due to set random seeds (`random_state=42`).


