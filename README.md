# NYC SAT Results: Funding Allocation via Unsupervised Learning

## 📌 Project Overview
This repository contains an unsupervised machine learning project completed as part of an MSc module. Acting as a Data Analyst for the New York City Department of Education, the objective was to analyze the 2012 SAT results of public high schools to establish an evidence-based framework for resource and funding allocation.

Instead of relying on simplistic raw averages, this project implements a rigorous data pipeline using **K-Means Clustering**, **DBSCAN**, and **Principal Component Analysis (PCA)** to uncover hidden structures in school profiles, separating academic performance dimensions from overall school size.

---

## 💼 Business Context & Problem Statement
Educational departments face a continuous operational challenge: how to allocate funding and strategic opportunities equitably across large public school systems. 
* **The Pitfall of Merit-Only Funding:** Directly rewarding only high-performing schools creates resource gaps, depriving struggling institutions of critical operational budgets.
* **The Pitfall of Flat Allocations:** Uniform distribution fails to acknowledge that some schools suffer from localized academic hurdles, while others face scaling bottlenecks due to massive student bodies.

This project builds a data-driven school categorization blueprint to transition the city away from arbitrary budgeting and toward tailored equity funding.

---

## 📊 Dataset & Preprocessing Profile
The project utilizes the official **2012 New York City SAT Results** dataset.
* **Initial Shape:** 478 public high schools.
* **Data Cleaning:** Removed 57 rows containing missing data flags ("s"), which are masked by the state for privacy when fewer than 5 students sit for an exam. 
* **Final Analysis Shape:** 421 schools.
* **Feature Space:** Number of SAT test takers (ranging from 6 to 1,277) and average school scores across Critical Reading, Math, and Writing.

### Feature Behavior Insights
* **The Multicollinearity of Performance:** The three SAT scores are extremely heavily correlated, indicating that academic performance scales uniformly across subjects within a single school.
* **The Independence of Scale:** School size (number of test takers) possesses a very weak relationship with actual test scores, demonstrating that operational size operates independently of academic achievement.

---

## 🛠️ Unsupervised Machine Learning Pipeline

### 1. Feature Standardization
Because distance metrics (K-Means) and orthogonal projections (PCA) are highly sensitive to variable scales, the data was scaled using `StandardScaler`. This prevented the large numeric range of student counts from completely overwhelming the bounded SAT score distributions.

### 2. Identifying Optimal Cluster Count (k)
To systematically determine how many school categories existed, four internal validation indices were evaluated over a range of clusters:
* **Elbow Method (Inertia):** Indicated a clear bend/elbow point at k=4.
* **Silhouette, Calinski-Harabasz, and Davies-Bouldin Indices:** While a mathematical split of k=2 achieved a higher silhouette baseline, it failed to provide granular utility for structural city funding. A split of **k=4** was selected to provide highly actionable policy partitions.

### 3. Dimensionality Reduction (PCA)
Applying Principal Component Analysis condensed the 4-dimensional feature space into a 2D plane while retaining **95.9% of the total variance**.
* **PC1 (78.2% Variance):** Weighted equally across reading, writing, and math scores, capturing overall **Academic Performance**.
* **PC2 (17.7% Variance):** Strongly dominated by the number of test takers, capturing **School Size/Scale**.

---

## 📈 Clustering Results & Policy Recommendations

The final K-Means architecture partitioned the New York City public school system into four distinct structural categories, supported by silhouette profiling (average index of 0.47):

| School Category | School Count | Academic Profile (SAT Avg) | Size Profile (Takers) | Strategic Funding Allocation |
| :--- | :---: | :---: | :---: | :--- |
| **Low Performing** | 276 schools | Lowest ~370 | Small to Moderate | **Equity Support Funding:** Targeted directly at fundamental resources, peer mentoring circles, robust tutoring tables, and community workshops. |
| **Below Average** | 32 schools | Average ~430 | Exceptionally Large | **Structural Interventions:** Focus on reducing class sizes, improving staff-to-student ratios, and administrative reorganization rather than strict curriculum changes. |
| **Above Average** | 92 schools | Moderate ~440 | Moderate | **Curriculum Optimization:** Allocation towards advanced teacher training and curriculum updates to push these schools into top-tier status. |
| **High Performing** | 21 schools | Top-Tier ~590 | Varied | **Innovation Grants:** Rewards for success to fund specialized STEM labs, robotics equipment, and experimental enrichment programs. |

> 🔍 **Density-Based Outlier Detection (DBSCAN):** As a secondary evaluation, DBSCAN (eps=0.6, min_samples=5) was implemented to identify extreme anomalies. It captured a primary dense core of 395 schools and explicitly flagged **26 outlier schools**. These represent hyper-isolated anomalies (both elite exam schools and critically failing academies) requiring independent, case-by-case evaluation by the city.
