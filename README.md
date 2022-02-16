# youtube-bias-analysis
## Extract and analyse the bias of a a large set of search and recommendation results from YouTube

This project was created in the contaext of a Master's Degree project and is not intended to be maintained. The code and data is available for further development and use, as is.

# Objective
This project aimed to analyze the political bias in YouTube search and recomendation results in the context of the US political discussion. The project aimed to provide an updated scrutiny of the YouTube search and recommendation algorithms, and to answer the following qustions:

1. Are YouTube users being exposed to a balanced mix of politically biased content?
2. Does YouTube promote the echo chamber effect?
3. Doe YouTube promote Mainstream Media over Independent Media content?

# Methodology

After the extraction of more than 460,000 search and recommendation results, the publishing channels were identified anc classified according to www.mediabiasfactcheck.com and/or www.allsides.com. For the videos published from channels not classified, few models and training datasets were created to select the bet performing one and use it to classify the remaining videos.

After the classification, the full results set was analyzed.
The code available in this repository was used to :

The names of the files indicate the order in which they were executed, as well as their functions. Along the executions, datafiles are created to allow for the continuation of the tasks without having to repeat previous steps.

All the code executed in found in the `./code` folder and datafiles at the different stages can be found (compressed) in the `./code/results`, `./code/model`, `./code/processed` and `./code/counts` folders.

All the coding was done and is published in Jupyuuter Notebooks format, some in Python and some in R.

### Video Extraction

1. YouTubeScraper.ipynb
2. completeVideoMetadata
3. extractVideoTranscript

### Transcript Classification

4. classifyTranscripts_R

### Training Videos Extraction and PReparation

6. channelStats
7. extractTrainingVideoTranscript
8. classifyTrainingTranscripts_R

### Model Creation and Results Classification

10. modelCreation_1-fromresults-Dataset1
10. modelCreation_1-fromresults-Dataset2
10. modelCreation_1-fromresults-Dataset3
10. modelCreation_channel_proportional_Dataset4-top15
10. modelCreation_channel_proportional_Dataset5-top15
10. modelCreation_1-fromresults-Dataset6

### Inventory and plotting

20. inventory_R
25. createPlottingDatasets
25. createPlottingDatasets-Copy1
26. inventory_R_plotting_fromFiles
