# Environmental Performance Index Project

<!-- ![.](https://github.com/Marc-Lodge/Environmental_Performance_Index/blob/main/epivsair.png) -->

This project covers several of the data science topics I have learnt, all in one; Pure Python, Exploratory Data Analysis, Supervised Machine Learning Modelling and Model Evaluation.

![World](https://sb.ecobnb.net/app/uploads/sites/3/2016/02/green-world-1170x490.jpg)

## 1. Problem & Goals

### Who Are The Greenest Countries in The World?

There are many different ways to answer this question, for each metric you can factor in you can argue about what weighting or aggregation can be applied to each. To help focus this project on what matters, I'll be focusing on an existing measure of this developed by Yale called the 'Environmental Performance Index' (https://epi.yale.edu/).

> The 2020 Environmental Performance Index (EPI) provides a data-driven summary of the state of sustainability around the world. Using 32 performance indicators across 11 issue categories, the EPI ranks 180 countries on environmental health and ecosystem vitality. These indicators provide a gauge at a national scale of how close countries are to established environmental policy targets. The EPI offers a scorecard that highlights leaders and laggards in environmental performance and provides practical guidance for countries that aspire to move toward a sustainable future.

#### What can be learnt from these Nations, if anything, that can be applied to other nations further behind in the scoring used?

The goal of this project is to:

- Explore all the factors that Yale have deemed the most accurate in making a Nation ‘Green’
- Find correlations between aspects of individual Nations, and their levels of success
- Then use this information to build a better picture to gain any insights

Success Criteria

- Correlations (strongly positive or negative)
- R2 Score

## 2. Dataset & Data Collection

The initial dataset we'll use for this will be the results of each "32 performance indicators across 11 issue categories" included in the EPI (this can be found here https://epi.yale.edu/downloads). I acknowledge this is a small dataset, but we are limited here first by the number of countries there are, and a slightly smaller number than that depending on validity of data meaning some countries will not be included. To get a feel for the data they have included, please see the 11 issue categories as described in the intro, below:

- Air Quality
- Sanitation & Drinking Water
- Heavy Metals
- Waste Management
- Biodiversity & Habitat
- Ecosystem Services
- Fisheries
- Climate Change
- Pollution Emissions
- Agriculture
- Water Resources

The above data is only one side of the coin; being the components that make up the Index and therefore of the target variable, we now need to get data of the world nations that are not environment related that can be candidates for predictor variables. For this, I obtained data from https://data.worldbank.org/. While there is huge scope for a project of this kind to look at as much data as possible, I have selected the below 10 variables to look at initially:

- Country
- Region
- GDP
- Population
- Debt to GDP Ratio
- Area KM2
- Density KM2
- Population Growth Rate %
- World % of Population
- Life Expectancy

## 3. Overall Approach

As the EPI score for 2020 is a continuous target variable, this is a regression project; I’ll use Simple and Multi Linear regression. I Will do analysis on both datasets with an emphasis on non-EPI (as the EPI is derived from the environmental data). Then I'll use data cleaning techniques to improve the quality of the data, then use descriptive stats, visual analysis (correlation heatmaps, plots, and other visualisations).

For the modelling, I set up predictor matrix(-ces) (X) and target arrays (y), find the baseline, apply train/test split and StandardScaler, and use cross-validation.

## 4. Exploratory Data Analysis

#### EPI Components

Looking at the correlation heatmap for the EPI score vs its components, you can get a good idea of the weighting / aggregation they assigned to each of the 11 ‘Issue’ categories without having to delve into their detailed report.

![.](https://github.com/Marc-Lodge/Environmental_Performance_Index/blob/main/EPI_Heatmap.png)

Environmental Health, being the objective and essentially what the EPI is trying to measure, is unsurprisingly the most correlated.

Air Quality and Sanitation & Drinking Water are the 2 categories that carry the most weight in terms of the EPI, so any further look at the individual components of the EPI, if any, will be around these two.

For comparison, Biodiversity appears to be the least correlated. 

#### Non Environmental Variables

![.](https://github.com/Marc-Lodge/Environmental_Performance_Index/blob/main/EPI_NON_Heatmap.png)

The first thing that is clear, is that a country's size, population and population density have nil to a slightly negative correlation to the EPI. 

Further, there is a strong negative correlation between a nation's population growth rate and their EPI score. Developing Nations find Green initiatives most challenging.

The two most positively correlated variables are GDP and life expectancy. GDP is hardly surprising and is something Yale pointed out themselves.

The life-expectancy, is this another variable that is strongly related to wealth? Or is there a correlation between a Nation's social and environmental policies?

![.](https://github.com/Marc-Lodge/Environmental_Performance_Index/blob/main/Hist.png)

Lastly for the EDA, we have used a histogram to check the distribution of the target variable. I notice there is some skew, to combat this I could have used the EPI data which took the change over a 10 year period vs just for one year as it was more evenly distributed. However, I ultimately decided that while that can be looked at, for the initial part of this project at least, I'd aim for the singular year (2020) 'spot' data.

## 5. Modelling

### Simple Linear Regression

![.](https://github.com/Marc-Lodge/Environmental_Performance_Index/blob/main/EPIvGDP.png) 

Despite some wealthy outliers skewing the data, the correlation between GDP and EPI is undeniable.

![.](https://github.com/Marc-Lodge/Environmental_Performance_Index/blob/main/EPIvLE.png) 

An unexpectedly strong correlation between the EPI and life-expectancy can be seen here, marginally higher than with GDP. Further, the Grouping of the regions is more pronounced than with any of the other variables.

![.](https://github.com/Marc-Lodge/Environmental_Performance_Index/blob/main/EPIvGrowth.png) 

The negative correlation on display here, not between the population size but population growth is clear; uncontrolled population growth is more of an issue than large populations that have stable and sustainable growth.

## Multivariate Linear Regression

So far, we have seen only Simple Linear Regression models, however I then went on to run Multi-Linear Regression Models of the entire EPI dataset, the entire non-environmental dataset and lastly, I picked the best performing variables and selected two together (GDP and Life-Expectancy) to see if I could create a model that could be a better predictor than Life Expectancy alone... Spoiler alert, I was not successful...

| Model             | Result                 | Whole EPI Dataset | Whole Non EPI Dataset | GDP & Life-Expectancy |
| ----------------- |:----------------------:| -----------------:| ---------------------:| ---------------------:|
| Linear Regression | Mean CV Training Score |               0.94|                   0.68|                   0.73|
|                   | R2 Training Score      |               1.00|                   0.82|                   0.75|
|                   | R2 Test Score          |               0.95|                   0.82|                   0.80|
| Ridge             | Mean CV Training Score |               0.94|                   0.77|                   0.73|
|                   | R2 Training Score      |               1.00|                   0.85|                   0.75|
|                   | R2 Test Score          |               0.95|                   0.83|                   0.80|
|                   | Cross-Predicted R2     |               0.99|                   0.85|                   0.80|
| Lasso             | Mean CV Training Score |               0.98|                   0.77|                   0.73|
|                   | R2 Training Score      |               0.99|                   0.82|                   0.74|
|                   | R2 Test Score          |               0.98|                   0.82|                   0.80|
| Elastic Net CV    | Best l1-Ratio.         |               0.99|                   0.10|                   0.10|
|                   | Training Score.        |               0.99|                   0.82|                   0.74|
|                   | Test Score.            |               0.98|                   0.81|                   0.80|

## 6. Conclusions

- An established base of wealth allows Countries to fund Green initiatives. “Established” means already wealthy but with a lot of debt does not matter, as the debt-to-GDP ratio has shown having very little correlation to the EPI.

- While Yale themselves noted a strong correlation between a country's wealth and their success on this score (main reason being that they can afford green and environmental initiatives), what was striking was that life-expectancy was a greater predictor of EPI score than GDP. This points to the fact that wealth as a factor being relatively equal, a country's social policies i.e. the healthcare they provide their citizens can have an added and substantial impact. Of course, there is a chicken and egg scenario here; wealthier countries have the means to provide better healthcare. Another chicken and egg aspect, a better environmental profile of a country may allow its citizens longer life spans due to lower risks of death from environmental factors.

- A countries Region also plays a strong part in predicting success, all of the global west were highly concentrated on the top end of this scale

- Population growth was a stronger negative predictor of EPI score vs actual population size. This tells us that already huge populations already have enough wealth / means to combat the worst aspects of environmental ruin, presumably due to growth being more stable and sustainable. High growth populations clearly lack that stability.

### Next Steps

- Look further into more specific variables and particular success stories.
- Build some categorical or classification models. This can be done by looking at either the subsets of the EPI, or as wealth plays such an important role, countries can be grouped by either wealth, region or even a time-series of how each Nation moves between groups over time. This link has a great example https://docs.lib.purdue.edu/fund/4/ 

