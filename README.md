# youtube-bias-analysis
## Extract and analyse the bias of a a large set of search and recommendation results from YouTube

This project was created in the contaext of a Master's Degree project and is not intended to be maintained. The code and data is available for further development and use, as is.

# Objective
This project aimed to analyze the political bias in YouTube search and recomendation results in the context of the US political discussion. The project aimed to answer the following qustions:


# Methodology

AFter the extraction of more than 460,000 search and recommendation results, the 


Where available, the individual bias of each result was assigned by the channel that published the video, according to www.mediabiasfactcheck.com and/or www.allsides.com. For videos published by channels not classified in the two reference sources, a model was creatd based on classified videos and 

The overall bias was calculated counting the number of videos of each bias in the search results and at multiple levels of recommendations.

Along with the bias, the "echo chamber" effect was analyzed by documenting how selecting videos with a given bias affects the bias of the recommendations that follow. This effect was also studied with alternative classifications using available dictionaries.

The code available in this repository was used to :

* Extract search and recommendation results
* Extract the transcripts for the videos
* Classify videos according to bias and each of the dictionaries
* Organize, count and plot

The project was performed through a collection Python and R notebooks, documented below.

# Video Extraction `./code/extraction/`

The notebook `YouTubeScraper.ipynb` (Phython) performs the main collection of search and recommendation results and dumps it into a file representing the search results tree.

After the creation of a few funcitons, the key is the `build_tree_from_search` which is called as an example in the last cell. The parameters for running it are:
* **query**: one or more search terms separated by comma. Each search term can contain one or more words
* **search_results**: the number of search results to be fetched
* **branching**: the number of recommendation results to be fetched at each recommendation level
* **depth**: the numebr of recommendation levels to be fetched
* **gl**: the location (country) passed of the query
* **language**: the language to filter the videos by
* **recent**: a flag to only query recent videos
* **lookup**: deprecated
* **alltime**: a flag indicating that videos from all dates are allowed
* **top_rated**: a flag indicating that only top_rated videos are allowed

The results are written in files, one per query term, concatenated with a timestamp and stored on the `./results`foilder.

Although the intention was to extract several video properties (channel, likes, etc.) several were dropped as they would not be used in the study, and the received htnl didn't consistently include them.

Execution for a single search term with **search_results=5**, **branching=5** and **depth=5** can take about 12 hours. During that, a couple of queries in the notebook `ValidateData.ipynb` allows to query the files in the `./results` folder to monitor the progress.

# Transcript Extraction

The notebook `extract_full_subtitles_to_file.ipynb` (Python) performs the extraction of transcripts.

After the creation of a few functions, the `filepaths` variable is unsed to indicate the folder to be investigating. This is generally the `./results` folder. All the files that comply with the pattern (`endswith` in the current status will be processed.

The routine in the last cell will keep only the unique videos and for each, retrieve the transcripts and create a new file with the same columns as the video extraction results, plus the transcript. The resulting file is stored in the `./subtitles` folder.

Executing for a the whole dataset can take up to 24 hours. During that, a couple of queries in the notebook `ValidateSubtitles.ipynb` allows to query the files in the `./subtitles` folder to monitor the progress.

# Channel Titles and Bias

The metadata provided by the vidoe fetching libraries includes the channel Id, not the title. It was also the case that the channels would not be available for all videos, or for difference appearances of the same video in the search results.

Routines in the `channelTitles.ipynb` (Python) notebook allow to:
* Get the titles for the fetched channels
* Assign channel to the instances of videos that did not retrieve a channel, where there was another instance that did
* Recreate the results files, now with channel title and bias (the channel-bias file had been created manually)

The need for the above routines will depend on the quality of the originally fetched results.

The `channel`folder incldues the files that list the retreived channels and the match between channels and bias.

# Classification `/code/classification`

The notebook `Classification.ipynb` (R) in the `Quanteda` folder uses the Quanteda library to perform quantitative analysis using 4 dictionaries, resulting in the classification of each video with the following:
* **positive**: the number of positive words
* **negative**: the number of negative words
* **sentiment**: the larger of positive vs. negative above
* **lgpp**: the Policy Position, according to the Laver & Garry Dictionary of Policy Position dictionary
* **nrc_el**: the Emotion, according to the NRC Emotion Lexicon (version 0.92) dictionary
* **topic**: the Topic, according to a topic dictionary available in Quanteda
* **Flesch**: the Flesch readability index
* **ARI**: the Automated Readability Index
* **Flesch.Kincaid**: the Flesch-Kincaid readability index

The resulting files are stored in the `Quanteda/Subtitles/classified` folder with prefix `classified_subtitles`.

The `categorizeSubtitles`functions perform the job. The code on the las cell allos to indicate the pattern for the files to be processed.

# Organize, count and plot `/code/inventory`

The notebook `inventory_R.ipynb` (R) starts loading the files from the original `results` folder and producing several counts that are part of the results of the project. 

The `Classification` loads the files from the `Quanteda/Subtitles/Classified/` folder whioch contains the list of unique videos and their classifications. A few more counts are produced by different groupings and these classified videos are then merged with the full list of results. Counts by different groupings are then produced.

The Sentiment by Ratio and labels for the readability results are added.

After a few more counts, the charting functions are created. Each of these funcitons will arrange the data so the charts can be produced for the different groupings and the charts be produced.

The cells below represent runs of the functions to produce the charts which are written to files in the `/inventory/Entregas/Images` folder. Each set of charts is built by selecting a specific bias from the search results and building a tree from the children of the videos with that bias.

There is a second version of the inventory file, `inventory_R_plotting_fromFiles.ipynb` (R) which produced a set of charts for each search term.
