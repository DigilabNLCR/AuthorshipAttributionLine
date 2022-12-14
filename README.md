# AuthorshipAttributionLine
Main DigiLab NLCR initiative

This project is main part of ongoing initiative to establish productive ML environement and test possible approaches upon data stored in National Digital Library.

## Organisational and technical workflow model (BPMN)

![AuthorLineWF rv](https://user-images.githubusercontent.com/25201690/206743941-be281d0e-3dcf-45c9-bf19-ebaf46884151.png)


## Running model(s)

| Version | Specification of literature | Authors | Passage Segmentation | Tokens Total  | Note |
| ---- | -------- | ------ | ----- | ------ | ------ |
| 1. version | Czech Romanticism  | 6  | 50,100,200,500,1000 tokens  | 2 214 878 | hyperparemeters set upon 300 experiments |
| 2. version |  Czech Romanticism | 23 | 50,100,200,500,1000 tokens  | 16 677 327 | stable |



## Environment

| Part | Solution | Detail |
| ---- | -------- | ------ |
| Automatised workflow | CRON + Authorguesser | https://github.com/DigilabNLCR/AuthorGuesser |
| Main ML Library | Scikit-learn | https://scikit-learn.org/ |
| Python version | 3.9 | https://www.python.org/downloads/release/python-390/ |
| Data Integration | National digital library of the Czech Republic | https://ndk.cz/ |
| Preprocessing Integration | UDPipe | https://lindat.mff.cuni.cz/services/udpipe/ |
| Monitoring | Htop, Netdata (+Zabbix)| https://github.com/netdata/netdata |
| Operation System | Centos 7.9 | EOL 1/2 2024, https://www.centos.org/download/ |
| GPU | NVidia Tesla V100S - 32GB | DP 8,2 TFLOPS / SP 16,4 TFLOPS |
| Server |1× SUPERMICRO 1U GPU server | LGA3647, INTEL Xeon Silver 4216 (16-core) 2.1GHZ, 64 GB RAM, 3840 GB SSD |

## **Roles**

### Conception and Research
* **Zdenko Vozár** -  *project lead, solution conception*
* **Jan Holomek** -  *project lead*
* **Martin Holub** -  *statistical and mathematical conception, theory of machine learning*
* **Jan Hajič** - *experiment design, theory of machine learning*
* **František Válek** - *delexicalisation research*
* **Michal Charypar** -  *dataset proposal, literary history background*

### Developement and Operative
* **František Válek** - *principal programmer. operative*
* **Jan Hajič** - *senior programmer, code review, experiment design*
* **Jiří Szromek** -  *supporting programmer, operative*
* **Petra Habetinova** -  *supporting programmer*
* **Inna Skoupá** -  *tester*
* **Miloš Bilwachs** -  *operative support*


## **More**
More information about DigiLab: https://digilab.nkp.cz/

# Using AuthorGuesse
## How to use the guessing script
Gerenal info:
- script: https://github.com/DigilabNLCR/AuthorGuesser/blob/main/authorguesser.py
- You can either use default settings that work with these directories:
    - texts_to_guess
    - models
    - guessed_files
- Or you can run the script with arguments modifying these parameters:
    - -m = models path (default "models")
    - -i = input path (default "text to guess")
    - -o = output path (default "guessed_files")
    - -d = delete input? (default True) set y/n --> if True, input files are deleted after guessing process

1) Place texts you wish to guess into the directory "texts_to_guess".
    - Note that texts must be below 18 000 characters (10 standad pages). (Due to using UDPipe https://lindat.mff.cuni.cz/services/udpipe/ - we do not want to overload it; also, the models are trained on far shorter segments.)
    - Encoding must be UTF-8.
    - Texts can include headers, page numbers, footnotes etc. - these are clened by the script.
    - Texts can have hyphenated words at the end of the lines, the lines are connected by the script.
    - The higher the number of words, the higher probability of a right guess
2) Run authorguesser.py
    - The script has been built in Python 3.9.
    - requirements for python packages: os, requests, joblib, re, datetime, time, json, argparse
    - The script prepares the file (cleaning, connecting lines...) and delexicalize it (using UDPipe). The delexicalization in this case means: part-of-speech tags for autosemantic words (nouns (NOUN), proper nouns (PROPN), adjectives (ADJ), verbs (VERB), adverbs (ADV), and numbers (NUM)), other words are lemmatized.
    - Model is selected according to the segment length (in tokens), and applied.
3) Check for the results
    - You can see the results in the terminal, or:
    - Go to directory "guessed_files", where you can find a xml file that contains:
        - basic info: guessed author, orginal filename, model used, etc.
        - delexicalized text
        - original string (cleaned and connected)
    - Result are also save in "guessed_files" direcotry, JSON file 'results.json'
        - only the basic info is presented in this file.

## Training new models
- In the directory "training" (https://github.com/DigilabNLCR/AuthorGuesser/tree/main/training), you can find a script that facilitates trainning of the models
- In addition to the scrip, you also need a dataset for training
- to run the script, you need to provide following aguments:
    - required arguments:
        - -m = models_path; path to directory where models are to be stored
        - -d = data_path; path to directory where all the delexicalized data are stored.
        - -clf = model_name; model name as in the sklearn package, only some are available in this version.
            - for this, we suggest to use LinearSVC
        - -n = ngram_range; set ngram range, use both values even if they are the same; e.g., "1,2" or "2,2"
            - for this, we suggest to use 1,3
        - -c = model_configuration; write in model configuration just as you would in model settings, but do not use spaces; e.g., "C=1.0,gamma=0.001,kernel='rfb',seed=42"
            - for this, we use empty command, as we use the models in their default settings.
    - optional arguments:
        - -a = author_ids; set list of author ids you wish to train, including the "a", like ["a-01", "a-02"]
            - if not set, all authors are included
        - -b = book_ids; set list of book ids, including the "b", like ["b-01", "b-02"]
            - if not set, all books are included
        - -p = passage_type; set type of passages to train on (all, train, devel, test), train is default.
- example of training command which has been used to train models presented here (except for paths): py -3.9 train_model.py -m "C:\Users\NAME\authorguesser\models" -d "C:\Users\NAME\authorguesser\full_delexicalized_dataset_r-08" -clf LinearSVC -n "1,3" -c ""

# Dedication
National Library of the Czech Republic

_Realized with the support of institutional research of National Library of the Czech Republic funded by Ministry of Culture of the Czech Republic as part of the framework of Longterm conception developement of scientific organization._

Národní knihovna České Republiky

_Realizováno v rámci institucionálního výzkumu Národní knihovny České republiky financovaného Ministerstvem kultury ČR v rámci Dlouhodobého koncepčního rozvoje výzkumné organizace._
