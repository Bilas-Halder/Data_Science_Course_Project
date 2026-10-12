# Project Progress Report: Building a Fiction vs. Non-Fiction Classifier

## Introduction and Goal

The objective of this project is to build a binary classification model capable of distinguishing between Fiction and Non-Fiction books based on their text descriptions and metadata. Since the end goal is a robust machine learning pipeline, I am prioritizing data quality, methodical feature engineering, and a deep understanding of the dataset over rushing to train a model.

For my dataset, I chose to scrape `books.toscrape.com`, a sandbox website containing 1,000 books spread across 50 granular sub-categories.

## Phase 1: Data Collection and Resiliency

I wrote a custom web scraper using Python and BeautifulSoup to extract the title, description, category, and cover image for all 1,000 books. Instead of using complex image URLs, I opted for a simple, sequential naming convention (1 to 1000) using a loop counter as the unique key (`Book_ID`).

**Where things went wrong:** During the scraping process, around page 35 (book 687), the scraper encountered an error and crashed.
**The fix:** Instead of panicking and re-downloading hundreds of images, I built a fault-tolerance check (`os.path.exists`) into the script. This allowed me to restart the scraper and blaze through the first 687 books, extracting only the text while intelligently skipping the image downloads until it picked up where it left off.

## Phase 2: Initial Cleaning and Feature Engineering

Once the data was collected, I moved into pandas. The sandbox site includes a lot of e-commerce filler—UPC codes, tax amounts, and stock availability. I dropped these irrelevant columns immediately to reduce noise.

Before even looking at the text descriptions, I hypothesized that Fiction and Non-Fiction books are marketed differently. I engineered a few features directly from the titles to test this during Exploratory Data Analysis (EDA):

* **Title Character & Word Count:** Assuming one genre might prefer punchier titles.
* **Subtitle Colon Presence (`:`):** I hypothesized that Non-Fiction books lean heavily on explanatory subtitles (e.g., "Title: How to Do Something"), making the colon a potentially strong predictive signal.

## Phase 3: The Target Variable Misjudgment

This is where I hit my biggest roadblock and learning moment.

To train the classifier, I needed to collapse the 50 specific sub-categories into a binary `Is_Fiction` target column. Initially, I created a list of obvious Non-Fiction categories (Business, History, Biography) and assumed the rest defaulted to Fiction. This resulted in an initial class imbalance of 73% Fiction to 27% Non-Fiction.

However, when I grouped the dataset to inspect my work, something felt off.

1. **Blind-spot errors:** I had accidentally left categories like "Science", "Crime" (True Crime), and "Historical" in the Fiction bucket.
2. **The "Default" trap:** The dataset contained a massive category literally called "Default" (152 books) and another called "Add a comment" (67 books).

My initial judgment based purely on the category labels was wrong. I realized I couldn't just assume the contents based on the label. I had to actually go through a few books from every ambiguous category to judge whether they truly belonged in Fiction or Non-Fiction.

By manually inspecting the data, I discovered that the "Default" category was a complete mixed bag of both genres—a deliberate quirk of the sandbox site.

## Phase 4: Course Correction

Feeding a machine learning model mixed or false labels will completely destroy its ability to find patterns. I made the hard data science decision to drop the "Default" and "Add a comment" categories entirely. While losing over 200 rows of data hurt, having ~780 cleanly labeled rows is infinitely more valuable than 1,000 poorly labeled ones.

I updated the mapping logic to correctly capture the missed Non-Fiction categories and applied the changes.