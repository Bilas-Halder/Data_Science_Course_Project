# Project Progress Report : Model Optimization and Image Feature Integration

## Introduction and Goal

Today is focused on hyperparameter tuning to break my 90% accuracy barrier and integrating book cover images as a secondary asset for a multimodal approach.

## Phase 1: Model Optimization & Grid Search

* **The Decision:** I used GridSearchCV to tune hyperparameters for Logistic Regression, Random Forest, and LinearSVC.
* **The Outcome:** LinearSVC performed the best, locking in a strong **89% test accuracy** on our text and numerical features. Which looked very good to me for now.

## Phase 2: The CNN Journey and Roadblocks

* **The Main Intention:** I wanted to use a pre-trained ResNet-50 CNN to extract deep visual embeddings from book covers, capturing artistic styles and layout patterns to push past 90%.
* **The Difficulty:** I hit persistent roadblocks. I encountered `TypeIs` import errors with `typing_extensions`, followed by silent M1 MacBook Jupyter kernel crashes caused by low-level OpenMP library collisions.
* **The Pivot:** To bypass PyTorch entirely, I shifted to a simpler, scikit-learn-native approach using **HOG (Histogram of Oriented Gradients)** to extract shape and edge layouts directly.
* **The Outcome:** While HOG ran smoothly on the M1 Mac, the multimodal model's accuracy dropped to **78.98%**. The raw visual noise from the cover backgrounds overpowered our clean text features.

## Current Status and Next Steps

I closed Day 3 with a robust text model at 89% and a clear understanding of why raw image features introduced noise.

**Next Steps for Day 4:** I will apply PCA to compress the image features and strip away noise, and try tackling CNN feature extraction again with a stable setup to make the final push past 90%.

Have a great rest of your day!