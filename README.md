# About The Project: RTLDepthNet

This project addresses the challenge of predicting combinational logic depth in RTL modules to identify timing violations early in semiconductor design. Traditional timing analysis occurs post-synthesis, making it a very time consuming process. Our solution trains an AI model that employs a data-driven, feature-based machine learning approach to estimate the number of logic gates required to generate critical signals (e.g., flip-flop inputs) directly from behavioral RTL code, bypassing full synthesis. 

First, a dataset was created by synthesizing diverse RTL modules collected from a dataset using the Yosys Synthesis Suite to extract structural synthesis details. For this, we used a dataset consisting of Verilog RTL Codes and used Yosys to generate structural details/features. 

To build a meaningful dataset, we engineered features such as the number of modules, ports, and operations to capture design complexity from the RTL Code itself. Feature engineering was critical: structural metrics (e.g., num_wires, num_memories) were combined with RTL-derived features like module hierarchy, input/output port counts, operator diversity (AND/OR/NOT density), and assignment/always block frequency. These features capture both design complexity and synthesis optimizations. Using Random Forest and XGBoost, we framed this as a regression problem, predicting the number of cells as a proxy for logical depth. An XGBoost regressor was chosen for its balance of accuracy and speed, trained on 80% of the dataset, with hyperparameters (e.g., max_depth, learning_rate) tuned via grid search to minimize mean absolute error (MAE).

# Analysis Results

In my analysis, the dataset was split into training and testing sets (80-20 split), and the model’s performance was evaluated using metrics such as Mean Absolute Error (MAE) and R² Score. Initially, our models had high errors (MAE ~416), but feature engineering and hyperparameter tuning improved performance, with XGBoost achieving MAE = 289 and R² = 0.70 on the test set, demonstrating a strong correlation between predictions and actual values. Additionally, cross-validation ensured robustness across different data splits, and predictions for unseen RTL modules aligned closely with synthesis results, proving the model’s generalizability.

However, further improvements plateaued, indicating the need for deeper feature extraction or alternative approaches such as graph-based representations of RTL structures or advanced synthesis-based metrics to enhance accuracy.

# Getting Started

Following libraries have been used: subprocess, re, tqdm, joblib, collections, pandas, scikit-learn.

Yosys can be downloaded from here: https://yosyshq.net/yosys/download.html
