
# YouTube Comments Sentiment Analysis

This project analyzes YouTube comments to determine the sentiment of viewers using sentiment analysis techniques. Additionally, it explores various aspects of trending videos such as correlations between views, likes, dislikes, and comment sentiments. The analysis includes checking for missing values, cleaning the data, and conducting sentiment analysis using the `TextBlob` library.

## Table of Contents

1. [Project Overview](#project-overview)
2. [Data Sources](#data-sources)
3. [Installation](#installation)
4. [Usage](#usage)
5. [Code Structure](#code-structure)
6. [Results](#results)
7. [Contributing](#contributing)
8. [License](#license)

## Project Overview

This project aims to analyze YouTube comments to determine the sentiment polarity of each comment. It involves the following steps:

1. Loading and cleaning the data.
2. Performing sentiment analysis on the comments.
3. Visualizing the results.
4. Exploring additional relationships within the data such as correlation between views, likes, dislikes, and the presence of punctuation in video titles.

## Data Sources

The primary data source used in this project is a CSV file containing YouTube comments. The data includes various fields such as comment text, likes, and the number of replies to each comment. Full data source at https://drive.google.com/file/d/1acUMXAGhctb1d_rRUUBqxDJsav_khTSW/view?usp=drive_link

## Installation

To run this project, you need to have the following Python libraries installed:

- pandas
- numpy
- seaborn
- matplotlib
- textblob
- plotly

You can install these libraries using pip:

```sh
pip install pandas numpy seaborn matplotlib textblob plotly
```

## Usage

To use this project, follow these steps:

1. Clone the repository.
2. Place the CSV file containing YouTube comments in the appropriate directory.
3. Open the Jupyter notebook and run the cells sequentially.

## Code Structure

The code in the Jupyter notebook is structured as follows:

1. **Importing Libraries**:
    ```python
    import pandas as pd
    import numpy as np
    import seaborn as sns
    import matplotlib.pyplot as plt
    ```

2. **Loading Data**:
    ```python
    comments = pd.read_csv('path_to_csv_file', on_bad_lines='skip')
    ```

3. **Data Cleaning**:
    - Checking for missing values:
        ```python
        comments.isnull().sum()
        ```
    - Dropping missing values:
        ```python
        comments.dropna(inplace=True)
        ```

4. **Sentiment Analysis**:
    - Installing and importing TextBlob:
        ```python
        !pip install textblob
        from textblob import TextBlob
        ```
    - Calculating polarity for each comment:
        ```python
        polarity = []
        for comment in comments['comment_text']:
            try:
                polarity.append(TextBlob(comment).sentiment.polarity)
            except:
                polarity.append(0)
        comments['polarity'] = polarity
        ```

5. **Data Visualization**:
    - Correlation heatmap:
        ```python
        sns.heatmap(comments[['views', 'likes', 'dislikes']].corr())
        ```
    - Bar plot of channels with the largest number of trending videos:
        ```python
        import plotly.express as px
        cdf = comments['channel_title'].value_counts().reset_index()
        cdf.columns = ['channel_title', 'total_videos']
        px.bar(data_frame=cdf.head(20), x='channel_title', y='total_videos')
        ```

6. **Exploratory Analysis**:
    - Analyzing the relationship between punctuation in titles and video metrics:
        ```python
        def punc_count(text):
            return len([char for char in text if char in string.punctuation])
        sample = comments.head(10000)
        sample['count_punc'] = sample['title'].apply(punc_count)
        sns.boxplot(x='count_punc', y='views', data=sample)
        ```

## Results

The analysis provides insights into the sentiment polarity of YouTube comments and explores relationships between various metrics such as views, likes, and dislikes. Visualizations such as heatmaps and bar plots help in understanding these relationships better.

