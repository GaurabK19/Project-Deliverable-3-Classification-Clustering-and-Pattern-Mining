## 1. Overview  
This project analyzes a synthetic healthcare dataset and applies three major data mining techniques:

1. **Classification Models**  
   Predict whether a patient has diabetes based on demographic, medical, and visit-related attributes.

2. **Clustering**  
   Discover natural patient groups using K-Means and interpret the characteristics of each cluster.

3. **Association Rule Mining**  
   Use the Apriori algorithm to uncover patterns between admission type, medications, conditions, test results, and insurance providers.

The analysis demonstrates how data mining techniques can support decision-making in healthcare operations, patient stratification, and clinical planning.

---

## 2. Classification Models  

### Target Variable  
A binary label **`is_Diabetes`** was created (1 = Diabetes, 0 = Non-Diabetes).

### Features  
Included:
- Age  
- Gender  
- Blood Type  
- Admission Type  
- Insurance Provider  
- Medication  
- Test Results  
- Billing Amount  
- Length of Stay

### Models Implemented  
1. **Decision Tree Classifier**  
2. **k-Nearest Neighbors (k-NN)**  
   - Baseline model  
   - **Tuned model using GridSearchCV**

### Hyperparameter Tuning  
k-NN was tuned using:
- `n_neighbors`: 3, 5, 7, 9, 11  
- `weights`: uniform, distance  
- `metric`: euclidean, manhattan  

**Scoring metric:** F1 score (due to potential class imbalance).

### Evaluation Metrics  
All models were evaluated using:
- Accuracy  
- F1 Score  
- Confusion Matrix  
- ROC Curve + AUC  

### Key Findings  
- The **tuned k-NN** improved over the baseline and performed competitively with the Decision Tree.
- Hyperparameter tuning significantly improved F1 and AUC, showing that k-NN is sensitive to distance metrics and neighbor count.
- Decision Tree showed strong interpretability but was slightly more prone to overfitting.

---

## 3. Clustering (K-Means)

### Method  
The dataset was scaled and encoded using the same preprocessing pipeline as classification.

The **Elbow Method** was used to select k = 3 clusters.

### PCA Visualization  
Clusters were projected into 2D using PCA for interpretability.

### Cluster Characteristics  

- **Cluster 0:**  
  This cluster has the highest counts across all medical conditions. It may represent the “general” patient population.

- **Cluster 1:**  
  Shows consistently lower counts for each condition compared to Cluster 0. This cluster may represent a smaller or less medically complex patient segment.

- **Cluster 2:**  
  Has medical condition counts similar to Cluster 1, but it shows the **highest Diabetes count** among all clusters.  

### Practical Value  
- Helps classify patient severity levels  
- Aids hospital staffing and bed allocation  
- Identifies high-resource vs low-resource groups

---

## 4. Association Rule Mining (Apriori)

### Preparation  
Categorical attributes were converted into one-hot encoded transactions:

- Gender  
- Blood Type  
- Medical Condition  
- Admission Type  
- Medication  
- Test Results  
- Insurance Provider  

### Frequent Itemsets  
Apriori was run with:

- `min_support = 0.01`  
  (Lowered to avoid sparsity issues and generate more meaningful rules.)

### Rules  
Association rules were generated using:

- Metric: lift  
- `min_threshold = 1.0`  

Strong rules were filtered using:

- Support ≥ 0.02  
- Confidence ≥ 0.5  
- Lift ≥ 1.05  

### Practical Value  
These rules can support:

- Inventory planning for medications  
- Identifying high-risk patient profiles  
- Improving quality of care pathways  
- Validating clinical protocols  

---

## 5. Practical Relevance

The combination of classification, clustering, and association rule mining provides a layered understanding of patient data:

- **Classification** assists with risk prediction and automated flagging of chronic conditions (e.g., diabetes).  
- **Clustering** identifies meaningful patient segments for resource optimization.  
- **Association Rules** provide interpretable insights into how medical variables interact in real patient scenarios.

---

## 6. Challenges & Solutions

Large numbers of medication, provider, and hospital categories caused sparsity in Apriori.  
**Solution:** Reduced categorical columns and lowered minimum support.
 
Initial filters eliminated all rules.  
**Solution:** Relaxed support, confidence, and lift thresholds; used fallback plotting.

Diabetes was not the majority class.  
**Solution:** F1 and AUC used instead of accuracy.

Clusters are unlabeled.  
**Solution:** Used summary statistics and crosstabs to interpret groups.

Mixed categorical/numerical features required careful handling.  
**Solution:** Unified ColumnTransformer + Pipeline.

---