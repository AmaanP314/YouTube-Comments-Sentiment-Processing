# YouTube Comment Augmentation and Sentiment Analysis

## Overview

This project provides a system for augmenting and analyzing YouTube comments using Google's Gemini API. The primary objective of this pipeline is to collect over **1 million YouTube comments** along with their **sentiment labels** for the purpose of **fine-tuning a large language model (LLM)** that is primarily trained on **Twitter tweets data**. By fine-tuning the LLM with YouTube comment data, the goal is to enhance its efficiency and accuracy in sentiment analysis, specifically for content and user interactions found on YouTube.

The project involves two main code files:
- **`comment_extraction_labeling.ipynb`**: Responsible for collecting YouTube comments and labeling them with sentiment.
- **`Comment_Augmentation.ipynb`**: Handles augmenting the comments using Google's Gemini API to create diverse variations while preserving sentiment.

## Prerequisites

Before running the code, ensure that the following dependencies are installed:

```bash
pip install google-generativeai pandas
```

Additionally, you'll need access to the Google Gemini API. Make sure to replace the `gemini_api` variables in the code with your own API keys.

## Directory Structure

The project contains two main files:
1. **`comment_extraction_labeling.ipynb`**: Sentiment analysis and comment fetching.
2. **`Comment_Augmentation.ipynb`**: Comment augmentation using Gemini API.

These files collectively implement a pipeline that:
- **Fetches YouTube comments** from specific videos.
- **Performs sentiment analysis** (positive, neutral, or negative) on the fetched comments.
- **Augments the comments** by generating alternative versions while preserving their original sentiment and meaning.

The output of the pipeline includes several CSV files, including:
- **`df_neu_tail.csv`**: Contains the augmented comments.
- **`sentiments-<region_code>-<cat_id>-<sort_by>-<video_id>.csv`**: Contains sentiment analysis results for individual videos.
- **`combined_sentiments-<region_code>-<cat_id>-<sort_by>.csv`**: A combined CSV file of sentiment analysis results for all videos.

## Main Objective: Collecting Data for Fine-Tuning an LLM

The main reason for creating these pipelines and scripts is to collect a large set of **YouTube comments** (targeting **1 million comments**) along with their corresponding **sentiment labels**. This dataset will be used to **fine-tune a large language model (LLM)** for sentiment analysis.

### Why Fine-Tuning an LLM with YouTube Data?
The fine-tuning process aims to:
- Improve the performance of the LLM on sentiment analysis tasks for platforms like YouTube.
- Enable the model to better understand the nuances of YouTube comments, which may differ from the short and often informal nature of Twitter tweets.
- Enhance the model's ability to classify the sentiment of user-generated content across different platforms.

By leveraging the collected YouTube comments and their sentiment labels, this fine-tuning process will help create a sentiment analysis model that is more tailored and efficient for handling the unique context of YouTube comments.

## How it Works

### `comment_extraction_labeling.ipynb`: Sentiment Analysis and Comment Fetching

1. **Fetching Comments**: 
   - The `sentiment_analysis_batch` function fetches YouTube comments for a given list of videos. It retrieves comments from multiple sources (up to 200 per video) and processes them by cleaning and filtering out any unnecessary timestamps or irrelevant data.
   
2. **Sentiment Analysis**: 
   - Sentiment labels (positive, neutral, negative) are generated for each comment based on the context provided by the video title. The sentiment analysis is performed using the Gemini API. 

3. **Output**: 
   - The sentiment analysis results are written to CSV files (`sentiments-<region_code>-<cat_id>-<sort_by>-<video_id>.csv` for each video, and `combined_sentiments-<region_code>-<cat_id>-<sort_by>.csv` for all videos).

### `Comment_Augmentation.ipynb`: Comment Augmentation Using Gemini API

1. **Comment Augmentation**:
   - The `augment_comments` function generates augmented versions of each comment, rephrasing them while retaining the original sentiment and meaning.
   
2. **Handling Multiple API Keys**:
   - The system rotates between multiple Gemini API keys (`gemini_api`, `gemini_api_2`, `gemini_api_3`, etc.) to avoid rate-limiting issues, with a rate limit of 10 requests per minute.

3. **Batch Processing**:
   - The `augmentation_comment_batch` function processes comments in batches and augments them in parallel. Once augmented, these comments are saved to a new CSV file (`df_neu_tail.csv`).

## Key Functions

### `comment_extraction_labeling.ipynb`: Sentiment Analysis and Comment Fetching
- **`sentiment_analysis_batch(region_code, cat_id, sort_by)`**: Fetches YouTube comments for videos and performs sentiment analysis on each comment.
- **`remove_links(comment)`**: Cleans comments by removing any URLs.
- **`has_multiple_timestamps(comment)`**: Checks whether a comment contains multiple timestamps.
- **`analyze_comments(video_title, comment_texts)`**: Analyzes the sentiment of a list of comments based on the video title.

### `Comment_Augmentation.ipynb`: Comment Augmentation Using Gemini API
- **`augment_comments(comments)`**: Takes a list of YouTube comments and generates rephrased versions with the same sentiment.
- **`augmentation_comment_batch(df)`**: Augments comments in batches and saves the results in a CSV file.
- **`robust_json_loads(raw_output)`**: Safely parses JSON data from the Gemini API response.
- **`escape_inner_double_quotes(s)`**: Escapes any internal double quotes in the comments to ensure proper formatting.

### Rate Limiting and API Key Rotation
The system rotates between multiple API keys to manage rate limits. The `toggle` variable ensures the system switches between keys after each request, and the system will sleep for a short period when the rate limit is reached.

## Example Usage

### Augmenting Comments
To augment comments using a batch, call the `augmentation_comment_batch(df)` function, where `df` is a DataFrame containing the original comments. The augmented comments will be saved to a new CSV file.

### Performing Sentiment Analysis
To perform sentiment analysis, the `sentiment_analysis_batch(region_code, cat_id, sort_by)` function can be used to fetch and analyze the comments for a list of YouTube videos.

### Output Files

After processing, the following files are generated:
1. **`df_neu_tail.csv`**: A CSV file containing the augmented comments.
2. **`sentiments-<region_code>-<cat_id>-<sort_by>-<video_id>.csv`**: A CSV file containing the sentiment analysis results for each video.
3. **`combined_sentiments-<region_code>-<cat_id>-<sort_by>.csv`**: A combined CSV file containing the sentiment analysis results for all videos.

## Conclusion

This system offers a robust solution for augmenting and analyzing YouTube comments using Google's Gemini API. It handles large batches of comments efficiently and ensures that both augmentation and sentiment analysis are performed at scale, with rate limiting and key rotation in place to ensure smooth operations. The ultimate goal of this project is to collect a large dataset of sentiment-labeled YouTube comments to fine-tune a large language model for improved sentiment analysis capabilities.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
