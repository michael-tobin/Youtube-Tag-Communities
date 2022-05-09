# Creating Co-Occurence Network of YouTube Video Tags

This project was undertaken as a part of GEOG 780, *Knowledge Analytics*,  at San Diego State University.
The goal was to build a co-occurence network of tags from YouTube videos across multiple channels and categories, then visualize that network. 

The plan was to scrape the tags from every video from each of the top 100 channels in each of the 18 categories. Not all videos use tags, and some videos use many tags. There is no limit to the number of tags (separated by a ,), but there is a 500 character limit per video.
The 18 categories are as follows:

1. Autos & Vehicles
2. Film & Animation
3. Music
4. Pets & Animals
5. Sports
6. Travel & Events
7. Gaming
8. People & Blogs
9. Comedy
10. Entertainment
11. News & Politics
12. How to & Style
13. Education
14. Science & Technology
15. Movies
16. Shows
17. Trailers
18. Nonprofits & Activism

## Pre-Code Preparation
The list of channels for each category was sourced from [YouTubers.me](https://us.youtubers.me "https://us.youtubers.me") by scraping the table with [Google Sheets](https://docs.google.com/spreadsheets/d/1mn0JNn-Yt2smJJ0WbkSb15-m3khWMINSS6l46Fw0DWU/edit?usp=sharing "Channel ID Spreadsheet"). I then manually followed the links on the site to obtain the individual YouTuber's channel link. With that link I was able to get their channel ID. The channel ID is everything that follows after "/channel/". 

With the channel ID's aggregated, I was able to begin obtaining the channel information along with the videos and tags.

## Jupyter Notebooks
This is the bulk of the project, layed out in the following files:

* 01_get_youtube_data.ipynb
* 02_translate.ipynb
* 03_cooccurence_graph.ipynb
* 04_Community_Graphs.ipynb

### 01_get_youtube_data

This is notebook utilizes the channel ID's collected in the preparation section. If you are planning on continuing this project, you will need to create a YoutTube API key with your Google developer account and update the "get_channel_data" function with your API key. Once updated, you can just run all cells and it will continue to poll the API for data, and update the local data until the quota limit is reached and an error is returned. It will then autosave the data; you should not need to interact with it further than that.

Due to YouTube API quota restrictions, I was not able got complete the data collection for multiple categories. This project, as it stands, represents the top ~50 channels in the automotive space at the time of upload. 

### 02_translate
Many of the channels in the top 100 are non-english speaking and thus their tags are in other languages. This notebook iterates through the data and translates the tags. I wound up not using this portion because it is very time intensive. I'm sure that there is a better way to go about this, maybe you can improve the efficiency of this one. 


### 03_cooccurence_graph
This notebook takes the dataframe created in the prevous notebooks and extracts a co-occurence network of the tags. In this instance, if two tags occur on the same video, then they are considered co-occuring; that is the only criteria. 

There are two outputs of this notebook: 
1. edges.pkl - For use in the next notebook.
2. graph.nwb - For use in Sci2.

### 04_Community_Graphs
This notebook utilizes networkx to build a graph, and extract communities from it. Greedy Modularity Community recognition algorithm was used. The larger of these communities are then graphed using PyVis which allows visually appealing and interactive network graphs (See example below)

![PyVis Community Graph](https://github.com/michael-tobin/Youtube-Tag-Communities/blob/main/Images/Community_16.png)

### Sci2
Windows users: Install Sci2 [here](https://github.com/CIShell/sci2/releases/tag/v1.3.0 "Sci2 GitHub").

Mac Users cannot install Sci2, you will need to do one of the following:
1. Load a windows/linux VM and install Sci2
2. Install windows with bootcamp and install Sci2
3. Install Docker and run the container [here](https://github.com/CIShell/sci2-docker-vnc "Sci2 Docker Container").

Once you have Sci2 up and running, you will want to load the graph.nwb file. From there, you can perform whatever network analysis and manipulations you would like. I have included a file, "graph.gephi", that includes the co-occurence network as of this upload (representing ~50% of the top 100 automotive channels) if you would like to skip the Sci2 portion. 

This gephi file has two network graphs inside of it:
1. Raw from Sci2 (contains nodes that should be merged like 'car' and 'cars')

![Full Gephi Graph](https://github.com/michael-tobin/Youtube-Tag-Communities/blob/main/Images/Full_Gephi.png)

2. A smaller graph that has had nodes manually merged and a few nodes removed (isolate communities and the largest node 'video')

![Merged Gephi Graph](https://github.com/michael-tobin/Youtube-Tag-Communities/blob/main/Images/Merged_Gephi.png)

## Wrap up
I hope to continue updating this project as time permits. Pleasefeel free to reach out with any comments, questions, or suggestions. 
