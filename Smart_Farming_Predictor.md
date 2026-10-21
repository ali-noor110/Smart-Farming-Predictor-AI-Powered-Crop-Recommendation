# AL-2002 Artificial Intelligence (Laboratory) — Project Proposal
**Fall 2026 | FAST School of Computing, Karachi Campus**

## Group Information
| Name | Roll Number | Section | Proposed role |
|---|---|---|---|
| Ali Noor | 24I-2073 | BCY-5A | Leader; coordination and integration |
| Gotam Lal | 24K-2047 | BCY-5A | AI implementation and evaluation |

- **Project title:** Smart Farming Predictor: AI-Powered Crop Recommendation
- **Project idea:** Own idea (approval status to be confirmed)
- **Category:** Part B — Applied / Data
- **GitHub repository URL:** To be added
- **Date of submission:** 21 October 2026

## 1. Project Title and Summary
Smart Farming Predictor is an AI-based application that recommends a suitable crop from soil and environmental conditions. It accepts nitrogen, phosphorus, and potassium levels, temperature, humidity, soil pH, and rainfall, then compares these inputs with crop requirements. The project combines A* search and a genetic algorithm with a Random Forest classifier used as a baseline for comparison. The system is intended to support farmers and agricultural learners in making more informed crop-selection decisions. It will display the recommended crop and comparison results so users can understand how the recommendation was produced.

## 2. Problem Statement and Motivation
Crop selection depends on several interacting conditions, including soil nutrients, pH, temperature, humidity, and rainfall. Farmers may rely on experience or general assumptions when they do not have access to soil analysis or agricultural advice. Choosing a crop that does not fit local conditions can reduce productivity and waste water, fertilizer, time, and money. Making a recommendation manually is difficult because each crop has a different range of suitable conditions and the input features use different scales. The project will use the Kaggle Crop Recommendation Dataset, described in the source proposal as containing 2,200 records, seven input features, and 22 crop classes. After normalizing the numerical features, the system will represent crop choices as a search problem and compare the search-based output with a Random Forest classifier. A genetic algorithm will also be explored for evaluating candidate farming plans against the available data. The project is worthwhile because it demonstrates how AI methods can support more consistent, data-informed crop selection.

## 3. Problem Formulation

### 3.1 PEAS Description
- **Performance measure:** Correctness of recommendations against labelled test data; classifier accuracy; search cost and runtime; genetic-algorithm fitness.
- **Environment:** A local Python application using crop/soil feature records from the locally stored Kaggle dataset; user supplies environmental values.
- **Actuators:** Display recommended crop, comparison predictions, and evaluation metrics.
- **Sensors:** User-entered N, P, K, temperature, humidity, pH, and rainfall values; dataset records during training/testing.

### 3.2 Environment and Agent Type
- **Observability:** Fully observable for each recommendation; all seven input features are provided.
- **Determinism:** A* and Random Forest predictions are deterministic for fixed data/model settings; the genetic algorithm is stochastic.
- **Change over time:** Static during a single recommendation run.
- **State and actions:** Discrete candidate crop choices with continuous-valued input features.
- **Agents:** Single-agent decision-support application.
- **Agent type:** Goal-based / utility-based search with a learning-based baseline.

### 3.3 Problem Representation
- **State representation:** A normalized seven-feature vector representing soil nutrients and environmental conditions, plus candidate crop nodes in the search graph.
- **Actions / moves:** Evaluate a candidate crop, move to a connected candidate in the crop graph, or generate/modify a candidate farming-plan chromosome.
- **Initial state:** The user's seven input values after validation and feature normalization.
- **Goal or terminal test:** Return a crop recommendation when the search reaches the selected best candidate; for GA, stop after 200 generations or the configured stopping condition.
- **Cost, utility, fitness or evaluation:** A* uses normalized Euclidean distance to a crop's representative/ideal feature vector as its heuristic; GA fitness is evaluated against available historical/labelled data; Random Forest predictions and accuracy provide the baseline.

## 4. Proposed AI Approach
| Technique | Role | Implementation |
|---|---|---|
| A* Search | Searches a weighted crop-candidate graph using normalized feature distance to guide the recommendation. | Own code |
| Genetic Algorithm | Searches candidate farming plans using tournament selection, single-point crossover (rate 0.8), and 200 generations. | Own code |
| Random Forest Classifier | Baseline classifier trained on the Crop Recommendation Dataset; compare predictions and test accuracy with the search-based approach. | scikit-learn/library (implementation details to confirm) |

**Baseline:** Random Forest Classifier.

A* search is proposed to rank crop candidates using normalized distances between the user's conditions and crop feature profiles. Min-max normalization is important because nutrient values, temperature, humidity, pH, and rainfall have different numeric ranges. The genetic algorithm provides a second search-based approach: candidate plans are encoded as chromosomes, selected through tournament selection, and combined using single-point crossover at a rate of 0.8 for up to 200 generations. A Random Forest classifier will act as a baseline so the project can compare a conventional supervised-learning prediction with the search-based recommendations. Results will be evaluated on the same held-out examples and with fixed random seeds where applicable. The distance heuristic and fitness definition will be documented and tested; claims of optimality will only be made if the implemented graph costs and heuristic satisfy the required conditions.

### 4.1 Planned Class Design
- **CropDataset:** Attributes: dataset path, feature columns, target column. Methods: load_csv(), clean_data(), split_data().
- **FeatureProcessor:** Attributes: feature names, scaling parameters. Methods: validate_input(), fit_scaler(), transform().
- **AStarCropRecommender:** Attributes: crop graph, heuristic, open list. Methods: build_graph(), heuristic_cost(), recommend().
- **GeneticFarmPlanner:** Attributes: population size, crossover rate, generation limit. Methods: initialize_population(), fitness(), selection(), crossover(), evolve().
- **RandomForestBaseline:** Attributes: classifier, training data. Methods: train(), predict(), evaluate().
- **RecommendationApp:** Attributes: model instances, current user input, results. Methods: get_input(), run_recommendation(), display_results().

## 5. Scope
### In scope
1. **Input and validation for seven soil/environment features:** Prevents invalid or incomplete user input.
2. **Load and preprocess the local Kaggle CSV:** Makes the application runnable from a local dataset.
3. **Implement normalized A* crop recommendation:** Delivers the primary search-based AI technique.
4. **Implement genetic algorithm with documented settings:** Provides a second AI technique for comparison.
5. **Train/evaluate Random Forest baseline and show results:** Provides a reference point for comparing recommendations.

### Out of scope
Live weather feeds, paid APIs, hardware sensors, mobile deployment, yield guarantees, and a production-grade farm-management platform are excluded. The system will provide data-based suggestions, not replace professional agronomic advice. The proposal does not yet specify a verified GitHub repository URL or a confirmed project-idea number; these should be added once available.

## 6. Data or Environment Source
- **Source name and URL:** Kaggle Crop Recommendation Dataset — https://www.kaggle.com/datasets/atharvaingle/crop-recommendation-dataset
- **Type:** Public dataset; stored and read locally as CSV.
- **Access requirements:** No paid source planned; dataset download/account requirements should be checked before submission.
- **Approximate size:** 2,200 records; 7 input features; 22 crop classes (as stated in the source proposal).
- **Licence / terms of use:** Check and follow the dataset's current Kaggle licence/terms before redistribution.
- **Offline fallback:** Keep the downloaded CSV in the project data/ folder. Load it from disk at runtime and show a clear error if the file is missing; no network access should be required for normal execution.

## 7. Evaluation Plan
- **What will be compared:** A* recommendation, Genetic Algorithm output, and Random Forest prediction.
- **Metric(s):** Accuracy on held-out labelled records; confusion matrix; average prediction/search runtime; A* nodes expanded; GA best fitness by generation.
- **Baseline:** Random Forest classifier.
- **Test instances / test set:** Use a fixed train/test split of the 2,200-record dataset; keep the same held-out records for all comparable methods.
- **Number of runs and seed:** At least 5 runs for stochastic GA experiments; record and fix random seeds. Use the same split and seeds for reproducibility.
- **Planned charts (at least three):** (1) Test accuracy by approach; (2) runtime by approach; (3) GA best fitness by generation; optionally (4) confusion matrix.

## 8. Division of Work
| Member | Modules / components owned | Approximate share |
|---|---|---|
| Ali Noor — 24I-2073 | Project coordination, dataset preparation, integration and final evaluation. | 50% |
| Gotam Lal — 24K-2047 | A* search, genetic algorithm, Random Forest baseline, and result visualizations. | 50% |

*Roles and shares are proposed and should be confirmed by the group.*

## 9. Milestones and Timeline
| # | Demonstrable milestone | Target date | Owner |
|---|---|---|---|
| 1 | Dataset loading, input validation, and feature preprocessing demonstrably run. | Week 11 / date to confirm | Ali Noor |
| 2 | A* recommender returns a crop for test inputs and records search metrics. | Week 12 / date to confirm | Gotam Lal |
| 3 | Genetic algorithm and Random Forest baseline run on the same dataset split. | Week 13 / date to confirm | Ali Noor |
| 4 | Integrated application displays predictions and comparison results. | Week 14 / date to confirm | All members |
| 5 | Experiments complete, final integration, documentation and repository cleanup. | Week 15 / date to confirm | All members |

## 10. Expected Outcome and Risks
A working submission will let a user enter seven soil and environmental values and receive a crop recommendation. The application will also show the A* result, genetic-algorithm result where applicable, and Random Forest baseline prediction, along with measured evaluation results. We expect the evaluation to reveal how recommendation quality and runtime differ between the approaches; no accuracy value is assumed before experiments are run. Two main risks are that the crop graph or heuristic may not represent crop suitability correctly, and that the GA may produce unstable results across runs. To reduce these risks, the team will document graph construction, test representative cases, use fixed seeds, run repeated trials, and report limitations honestly.

## References
1. Atkar, A. (2020). *Crop Recommendation Dataset*. Kaggle. https://www.kaggle.com/datasets/atharvaingle/crop-recommendation-dataset
2. Streamlit Documentation. https://docs.streamlit.io
3. Russell, S. & Norvig, P. (2021). *Artificial Intelligence: A Modern Approach* (4th ed.). Pearson. A* Search, Chapter 3; Genetic Algorithms, Chapter 4.
