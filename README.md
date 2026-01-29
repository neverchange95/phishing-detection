# blacklist

### 1. blacklist-phishing-detection.ipynb
* Contains blacklist evaluation code and results

### 2. blacklist-server.py
    python blacklist-server.py --gsb-key <YOUR_API_KEY>
* Opens a HTTP-Server with one route `/ingest-urls` to receive phishing and begin urls
* Checks urls against Googe Safe Browsing API v4 Blacklist
* Creates a CSV-File which shows the result for requested urls by their label and some metadata

### 3. push-legitimate-urls-to-blacklist-server.py
    python push-legitimate-urls-to-blacklist-server.py --csv-file <YOUR_CSV_FILE_WITH_URLS>
* Helper Script which is used to send legitimate urls to backlist-server.
* Pushes a list of urls from a CSV-File (column "url") to the blacklist-server which ist pushing these urls against the Google Safe Browsing API
* Script can also be used for phishing urls

### 4. test.ipynb
* This notebook was used to find potential conflicts between the URLs in evaluation-features.csv and blacklist-evaluation-results.csv, 
ensuring that all procedures are safely evaluated on the same datasets.
---
# chatgpt
### 1. chatgpt-evaluation-input.csv
* Contains phishing and legitimate url list, used as evaluation input for chatgpt

### 2. chatgpt-phishing-detection.ipynb
* Contains chatgpt evaluation code and results

### 3. prompt
* Contains the prompt which is used for chatgpt evaluation on url based dataset and its answer
---
# data
### evaluation
* Includes legitimate and phishing URL dataset which is used for evaluation (raw-data)
* Includes extracted features from phishing and legitimate urls (evaluation-features.csv)
* Includes the blacklist and chatgpt evaluation results (blacklist-evaluation-results.csv/chatgpt-evaluation-results.csv)
* Includes a python script to filter duplicate and unique data between evaluation-features.csv and blacklist/chatgpt-evaluation-results.csv (filter-duplicate-and-unique-data.py)
* Includes a python script to extract only urls as csv file from evaluation-features.csv to use as chatgpt input (get-urls-as-csv.py)

### training
* Includes the legitimate and phishing URL dataset as well as the features generated from the URLs, which were used to train the models.
---
# data-collection-and-transformation

### 1. feature-extractor.py
    python feature-extractor.py --isPhishing <0/1> --file <YOUR_BENGIN/PHISHING_URL_CSV_FILE>
* Extracts 27 Features from given urls and safe them into a feature.csv file
* This feature list can then be used for machine learning algos as input
* `--isPhishing 0` --> CSV-File contains Phishing-URLs
* `--isPhishing 1` --> CSV-File contains Legitimate-URLs
* Your CSV-File must contain a column named "url" where urls to analyse are stored

### 2. httparchive-big-query.sql
* Used Google Cloud httparchive BigQuery to get legitimate urls for training and evaluation
  * 50/50 Root/Non-Root URLs
  * 30% with scheme http 
  * Per origin max. 5 root/non-root urls and max. 2 urls with scheme http

### 3. PhiUSIIL_Phishing_URL_Dataset.csv
* Used Dateset to get phishing urls for training ml/dl algos

### 4. pull-openphish-feed.py
    python pull-openphish-feed.py --repo-url https://<YOUR_GITHUB_USER>:<TOKEN>@github.com/openphish/academic

##### Is used to create a realtime pipeline on phishing urls for blacklist checking and write that urls with some metadata to a csv-file which can be used to evaluate machine learning and deep learning algorithms.
* Pulls OpenPhish Academic Use Program realtime phishing url feed within a specified time window
  * Time Window can be changed with `--pull-interval`. By default, set to 5 minutes.
* Writes only the most recent entries to a csv file that occurred within the specified time window
* Sends url and some metadata of recent entries to a listening blacklist-server for evaluation of these blacklist

### 5. split-by-label.py
    python split-by-label.py <YOUR_CSV_FILE_WITH_URLS_AND_LABEL> --out0 <OUT0.CSV> --out1 <OUT1.CSV>
* Helper Script which is used to split a CSV-File with both phishing and legitimate labled urls into two seperated files
* Input file must contain columns: "url" and "label"
* Label must be 0/1
    * 0 --> Phishing
    * 1 --> Bengin
* `--out0`: Output File for Phishing URLs
* `--out1`: Output File for Bengin URLs
---

# ml-dl

### 1. ml-phishing-detection.ipynb
* Contains machine learning algos evaluation code and results

### 1. dl-phishing-detection.ipynb
* Contains deep learning algos evaluation code and results
