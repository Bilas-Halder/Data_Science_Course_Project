# Project Progress Report: Text Feature Engineering and Baseline Model

## Introduction and Goal

After getting our dataset cleanly labeled on Day 1, Day 2 was all about diving deep into text preprocessing, visualization, feature fusion, and training our very first baseline machine learning model to distinguish between Fiction and Non-Fiction books.

## Phase 1: Text EDA and Visualizations

Before jumping straight into math, I wanted to visually verify if the book descriptions and titles actually looked distinct.

* **The Decision:** I generated side-by-side word clouds for both titles and descriptions using a clean color gradient.
* **The Outcome:** The word clouds instantly validated our approach. Fiction titles and descriptions clearly spotlighted narrative elements like "novel", "vol", and interpersonal words, while Non-Fiction focused heavily on instructional or factual terms like "recipes", "history", and "science".

## Phase 2: Feature Engineering

Next came the technical structuring of our text data.

* **The Difficulty & Solution:** I initially wondered if I should vectorize titles and descriptions separately, or clean out common words like "life" or "new manually". I decided instead to combine titles and descriptions into a single `full_text` field to capture vocabulary from both fields at once. For omnipresent words like "life", I trusted TF-IDF's built-in Inverse Document Frequency (IDF) to mathematically lower their weight automatically rather than over-cleaning manually.
* **Feature Combination:** I combined the text TF-IDF matrix with our custom-engineered numerical features (`title_char_len`, `title_word_count`, and `has_subtitle_colon`) to give the model both semantic text depth and title-specific structure.

## Phase 3: Training and Evaluating the Baseline Model

With the feature matrix ready, I performed a stratified train-test split and trained a baseline **Logistic Regression** model using balanced class weights.

* **The Outcome:** The baseline model achieved an impressive **81.53% accuracy** and a Macro F1-score of **0.80** on its very first run.
* **Coefficient Analysis:** Peeking under the hood of the model revealed an exciting validation: my engineered feature `has_subtitle_colon` emerged as the absolute strongest predictor for Non-Fiction books, proving that my early hypothesis about explanatory subtitles was spot-on.

## Current Status and Next Steps

The baseline model is performing exceptionally well, proving that our data cleanup and feature choices were solid.

**Next Steps for Day 3:** We will experiment with advanced classifiers and Grid Search CV to see if we can push our accuracy past the 90% mark. If text-based features hit a ceiling, we will leverage our secondary asset—the scraped book cover images—to squeeze out even higher performance.