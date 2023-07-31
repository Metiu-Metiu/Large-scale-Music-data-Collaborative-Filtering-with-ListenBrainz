# Collaborative Filtering with ListenBrainz
## Introduction

In this task we will build a model which can identify similar musical artists based on user listening history from ListenBrainz. 

We provide a data directory containing the whole listening history for ListenBrainz for 2022. This contains almost 50 million separate events (times when a person listened to a song), and occupies over 30gb of data. However, don't worry. we don’t need to use all of the data at once.

The first part of this task involves processing the ListenBrainz data files to extract only the information that we need. You can perform this computation locally if you have enough disk space, otherwise you can use google colab.

Find the necessary data files at https://drive.google.com/drive/folders/13rfNpcbYnyNWcuTmgPUHIW7xtCpQw_Nr 

# Data
The ListenBrainz data is distributed in 12 files, one per month of 2022. The data is in a format called json lines. This format has one json document per line, each representing a single instance of a person listening to a song.

Data in ListenBrainz is referenced by an identifier called a “MessyBrainz ID” (recording_msid). This ID is random, and the same song could have multiple different MessyBrainz IDs depending on how it was submitted to ListenBrainz. In order to be able to compare items with each other we want to convert it to data that is available in the MusicBrainz database.

To do this we provide three additional pieces of data.
The ListenBrainz Mapping, which provides a link between MessyBrainz IDs and a MusicBrainz Recording ID (listenbrainz_msid_mapping.csv)
The ListenBrainz Canonical Metadata database, which provides additional MusicBrainz data for a given Recording ID. This includes an artist credit, which is a list of MusicBrainz Artist Ids. Note that some songs could be performed by multiple artists, and so MusicBrainz includes IDs of all artists which are involved in the performance (canonical_musicbrainz_data.csv in the metadata-dump folder). 
Artist MBID to name mapping, which gives the name of all artists in the MusicBrainz database. (musicbrainz_artist_mbid_name.csv)

# Data pre-processing
We need to convert the ListenBrainz data dump to a matrix containing user ids, artist ids, and play counts (how many times someone listened to a given artist in 2022). Write a script to do the following:

Read the jsonlines data
For each item, extract user information and the MessyBrainz id
Look up a recording MusicBrainz id in the mapping. Note that not all items in the dataset have a mapping available, so you will need to skip these items.
Look up an artist for the recording. In the case that a recording has multiple artists you can choose what to do
Take only the first artist in the list
Add multiple entries, one for each artists in the list
Treat the artist credit as a single entity (in this case you can use the “artist credit name” from the metadata file as your mapping in the next step)
Build a mapping of artist id to a textual name so that you can use the name to show results

You may wish to process only a part of this dataset during your initial development and then process the full dataset once you are ready to evaluate it.

# Collaborative filtering models
The second part of this task involves using the Implicit library to compute a collaborative filtering model using the Alternating Least Squares method. We will be following the tutorial from the Implicit website: https://benfred.github.io/implicit/tutorial_lastfm.html

This tutorial uses a very similar dataset from the MTG, based on listening history from last.fm. You will need to ensure that you generate data in a format adequate for the rest of the tutorial. You can download the sample last.fm dataset to see its format, and modify the code in the implicit.datasets.lastfm package to read your data into a sparse matrix. 

Use the MusicBrainz site to search for some artists that you are familiar with and determine their MusicBrainz IDs. If these artists are in the model that you just developed, use the model to identify similar artists. Comment on if in your opinion these artists are similar to each other. Use another site, such as last.fm or spotify to find which artists that site says are similar. Comment on any differences that you see between the similarities from your model and from the site.

Investigate the data model that you have built. Determine how many users are used to build this model. Look at the documentation for the Lastfm 360k dataset to determine how many users are in it.
