# Music Popularity Prediction: Coarse-to-Fine Optimization & Stacking Regressor

This project documents the development of a regression system designed to predict song popularity based on technical audio attributes. The workflow covers everything from exploratory data analysis to the implementation of a Stacking Regressor, demonstrating thorough control over overfitting and hyperparameter optimization.

## Methodology and Optimization Strategy

The technical core of this work is built on two pillars:

1. **Pipelines and Data Leakage Prevention**: I used scikit-learn's `Pipeline` class to encapsulate data scaling (`MinMaxScaler`) and training. This ensures that no data leakage occurs and that preprocessing is independently validated in each cross-validation split.
2. **Coarse-to-Fine Optimization**: Instead of relying on simple random searches, I implemented a hierarchical strategy for logarithmic scale parameters (such as Alpha, Gamma, or C). I first explored broad orders of magnitude to identify the region of interest and subsequently conducted dense searches to find the optimal value.



## Evaluated Models

The following algorithms were trained and tuned using `GridSearchCV`:

* Linear Regression and Kernel Ridge.
* K-Neighbors Regressor (KNN).
* Decision Trees and Random Forest.
* Support Vector Regression (SVR).

## Stacking Model Analysis

To achieve maximum predictive power, I implemented a `StackingRegressor` using the previous models as base-learners and a Linear Regression as the meta-model. The analysis of the meta-model coefficients yields the following insights:

### Importance Hierarchy
The meta-model primarily trusts a tandem of two approaches: **Random Forest (weight: 0.63)** and **K-Neighbors (weight: 0.49)**. Together they dominate the decision-making process, combining logical decision rules with distance-based similarity.

### Models Discarded due to Redundancy
The individual Decision Tree received a near-zero weight (**-0.001**). This confirms that with Random Forest present—which is already an ensemble of trees—a solitary tree provides no incremental information. SVR was also significantly ignored (**-0.06**).

### The Role of Negative Coefficients
Kernel Ridge presents a coefficient of **-0.20**. In such an ensemble, negative values act as correction mechanisms: if other models tend to overestimate popularity, the meta-model uses this value to offset and balance the final result.

### Accuracy Improvement
The Stacking implementation proved effective, reducing the **RMSE error from 17.23** (best individual model) to **16.88**, confirming that combining perspectives improves generalization.



## Critical Analysis and Conclusions

* **Nature of the problem**: Results indicate that the relationship between audio and popularity is preeminently **non-linear**. Pure linear models were outperformed by approaches capable of capturing complex patterns, like Random Forest.
* **Overfitting and Generalization**: The model showing the highest variance was the Decision Tree, with a clear tendency to memorize data. Stacking mitigated this effect, becoming the model with the best generalization capacity on the test set.
* **Limitations and Realism**: Predicting a song's success based solely on audio is a limited challenge. Critical external factors such as label influence, marketing investment, artist fame, or social media virality are not present in this dataset, establishing a natural "ceiling" for the model's accuracy.
