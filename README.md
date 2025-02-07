# YouTube Comments Sentiment Dataset

## Overview
This repository hosts a large-scale dataset of over **1 million YouTube comments** annotated with sentiment labels (Positive, Neutral, Negative). The dataset is designed for research in natural language processing, sentiment analysis, and content moderation. In addition, this repo includes code for the full pipeline—from extracting and labeling comments using the YouTube Data API to augmenting comments to balance the dataset.

## Repository Structure
- **comment_extraction_labeling.ipynb**  
  Notebook containing the pipeline for extracting YouTube comments and labeling them with sentiment.
  
- **comment_Augmentation.ipynb**  
  Notebook demonstrating the augmentation pipeline that generates paraphrased versions of comments (with a focus on preserving programming-related context) to balance the dataset.
  
- **README.md**  
  This file, providing an overview of the dataset, methodology, and usage instructions.
  
- **LICENSE**  
  The license file for the dataset (e.g., CC BY 4.0 or CC0).

## Dataset Description
The dataset includes the following columns:

- **CommentID**: A unique identifier for each comment.
- **VideoID**: The ID of the YouTube video from which the comment was extracted.
- **VideoTitle**: The title of the corresponding video.
- **AuthorName**: The display name of the comment author.
- **AuthorChannelID**: The unique channel identifier for the comment's author.
- **CommentText**: The text content of the comment.
- **Sentiment**: The sentiment label assigned to the comment (Positive, Neutral, Negative).
- **Likes**: The number of likes the comment received.
- **Replies**: The number of replies to the comment.
- **PublishedAt**: The timestamp when the comment was published, in ISO 8601 format (e.g., `2024-04-01T07:37:07Z`).
- **CountryCode**: A standardized region code indicating the geographic origin of the video/comment.
- **CategoryID**: An identifier categorizing the video content.

## Data Collection and Labeling
- **Extraction:**  
  Comments were collected using the YouTube Data API from a diverse range of channels (spanning topics such as programming, news, and politics).
  
- **Labeling:**  
  Sentiment labels were generated using a combination of advanced AI models (such as Gemini) and manual validation to ensure high accuracy.

## Data Augmentation
To address class imbalances—particularly for negative and neutral comments—a comment augmentation pipeline was implemented. This process:
- Paraphrases comments while preserving the original sentiment and meaning.
- Introduces linguistic diversity by varying vocabulary and sentence structure.
- Ensures that the augmented output is valid, consistently formatted, and matches the number of input comments.

## Usage
You can use this dataset to train and evaluate sentiment analysis models or for further research in natural language processing. The notebooks in this repository demonstrate the entire pipeline from data extraction to augmentation.
