# YouTube Comment Sentiment Analysis

This project performs sentiment analysis on YouTube video comments using the Gemini API and YouTube Data API. The goal is to fetch comments from YouTube videos, analyze their sentiment (positive, neutral, or negative), and output the results in CSV format. 

## Requirements

- Python 3.7+
- Required Python libraries:
  - `pandas`
  - `aiohttp`
  - `google.generativeai`
  - `csv`
  - `json`
  - `re`
  - `asyncio`
  - `time`

You can install the necessary libraries with:

```bash
pip install -r requirements.txt
```

## API Keys

This project utilizes the following APIs:
1. **Google YouTube Data API**: Required for fetching video details and comments.
2. **Gemini API (Google's generative AI)**: Required for performing sentiment analysis.

### Setting Up API Keys

To use the project, you'll need to set your own API keys. 

1. **Gemini API Key(s)**: You can use multiple keys to avoid rate limits imposed by Gemini.
2. **YouTube API Key(s)**: Similar to Gemini, you can use multiple keys to avoid hitting the rate limit of YouTube API.

Set your keys in the script:

```python
gemini_api = 'your_gemini_api'
gemini_api_2 = 'optional_gemini_api'
gemini_api_3 = 'optional_gemini_api'
gemini_api_4 = 'optional_gemini_api'
gemini_api_5 = 'optional_gemini_api'
youtube_api = 'your_youtube_api'
youtube_api_2 = 'optional_youtube_api'
youtube_api_3 = 'optional_youtube_api'
```

If you wish to use only one API key, the system will still function, but rate limiting may be encountered if the volume of requests is high.

## Functions

### Fetching YouTube Videos

1. **`fetch_videos(region_code, category_id, max_results, sort_by)`**:
   Fetches popular YouTube videos based on region and category. Allows pagination for large datasets.

2. **`fetch_videos_channel(channel_id, max_results, sort_by)`**:
   Fetches videos from a specific YouTube channel.

3. **`fetch_video_details(video_id)`**:
   Fetches detailed information about a specific video, including statistics and content details.

4. **`fetch_comments_data(video_id, api, max_results, order)`**:
   Fetches comments for a specific video. Supports pagination and API key rotation.

### Sentiment Analysis

The sentiment analysis function classifies YouTube comments as "positive", "neutral", or "negative" based on the video's title, topic, and the tone of the comments.

1. **`analyze_comments(title, comments)`**:
   Analyzes the sentiment of a list of comments. It uses the Gemini API for analysis, rotating between multiple API keys to prevent hitting the rate limit.

2. **`sentiment_analysis_batch(region_code, cat_id, sort_by)`**:
   This function fetches video details and comments for a specific region and category. It then performs sentiment analysis on the fetched comments and saves the results to CSV files.

### Helper Functions

- **`has_multiple_timestamps(comment)`**: Checks if a comment contains multiple timestamps (e.g., for videos with timecodes).
- **`remove_links(comment)`**: Strips URLs from comments to avoid irrelevant content during analysis.

## Running the Script

You can run the sentiment analysis batch script using the following function:

```python
sentiment_analysis_batch(region_code="US", cat_id="10", sort_by="viewCount")
```

### Parameters:
- `region_code`: The region code (e.g., "US", "GB").
- `cat_id`: The YouTube video category ID (e.g., "10" for Music).
- `sort_by`: How to sort the videos (e.g., "viewCount", "relevance").

The function will fetch the most popular videos from the specified region and category, analyze the comments, and output sentiment results in CSV files. The files will be named based on the region, category, and sort order.

### Example:

```bash
python sentiment_analysis.py
```

The script will generate CSV files for each video analyzed, and a combined CSV file containing the sentiment analysis results for all processed videos.

## Error Handling

- If a video has no comments or if there’s an issue fetching the comments, the video will be skipped, and it will be logged in the `failed_videos` list.
- Rate limiting is handled by rotating through multiple API keys and introducing delays when necessary.

## Output

After processing the comments, the results will be saved to CSV files:

- A CSV file for each individual video: `sentiments-{region_code}-{cat_id}-{sort_by}-{video_id}.csv`
- A combined CSV file containing the results for all videos: `combined_sentiments-{region_code}-{cat_id}-{sort_by}.csv`

The CSV files will contain the following columns:

- `ComId`: Comment ID
- `Vid`: Video ID
- `VideoTitle`: Title of the video
- `AuthorName`: Name of the author
- `AuthorCid`: Author's channel ID
- `Comment`: The comment text
- `Sentiment`: The sentiment label ("Positive", "Neutral", or "Negative")
- `LikeCount`: Number of likes on the comment
- `ReplyCount`: Number of replies to the comment
- `PublishedAt`: Date and time the comment was published
- `RegionCode`: The region code where the video is popular
- `CategoryId`: The video category ID

## License

This project is open-source and available under the MIT License.
