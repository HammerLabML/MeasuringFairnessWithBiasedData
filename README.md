# MeasuringFairnessWithBiasedData

This repository contains the code and some artifacts to reproduce the paper:

Sarah Schröder, Alexander Schulz, Ivan Tarakanov, Robert Feldhans and Barbara Hammer. Measuring fairness with biased data: A case study on the effects of unsupervised data in fairness evaluation. Accepted at IWANN 2023 and to be published in the LNCS proceedings.


## Requirements

We provide our conda environment in ```env.yml```. The following packages are necessary to run the notebooks:

- numpy
- scipy
- pandas
- matplotlib
- seaborn
- torch
- scikit-learn

In addition we used a [custom wrapper for huggingface embeddings](https://github.com/UBI-AGML-NLP/Embeddings).


## Dataset

The paper investigates the quality of the [BIOS dataset](https://github.com/microsoft/biosbias). The dataset is not published due to copyright and data protection reasons. However, a crawler to download the dataset can be found [here](https://github.com/microsoft/biosbias). The results from our review can be found in ```data/BIOS_LABELS_v1.csv``` and ```data/BIOS_LABELS_v2.csv```. It contains no sensitive information such as the biographies themselves or names, only the original ids, labels and paths as well as our review results that can be merged with the BIOS dataset.


Version differences:
- v1 is the version used in the published paper (10 of the original classes)
- v2 adds the software enginner and model class (12 of the original classes)

Columns from the original dataset:
- path
- gender
- start\_pos
- URI
- auto\_raw\_title (renamed from raw\_title)
- auto\_title (renamed from title)

Columns from our review:
- review (1 if the samples was reviewed, otherwise 0)
- raw\_titles (a list of raw\_titles determined by the annotator)
- titles (a list of titles derived from the raw titles and the ```title_lookup.json```, including only occupations used in the paper)
- valid (1 if the annotator decided the sample was a valid biography (see paper) otherwise 0 (or -1 for samples not reviewed))
- style\_valid (1 if the annotator decided the text style matched a biography, in addition to being valid, otherwise 0 (or -1 if not reviewed/ invalid))


## Reproducing the paper

#### Finding candidate samples for review
Use ```filter_dataset.ipynb``` to obtain a selection of approx. 20.000 candidate samples we used for the review.

#### Patch review labels to dataset
If you downloaded the BIOS dataset yourself, use ```add_changes_to_dataset.ipynb``` to add our labels to the dataset. Move the downloaded ```BIOS.pkl``` to the ```data/``` directory or adapt the path in the notebook.  
If you prefer the [huggingface version](https://huggingface.co/datasets/LabHC/bias_in_bios) of the dataset instead, use ```huggingface_patch.ipynb```.  

#### Experiments from the paper
To reproduce our experiments, you first need to download the dataset yourself and merge our labels (see above). Then you can use ```data_review.ipynb``` to run our experiments. During the experiments we trained 30 BERT models, so it may take a while to run.
