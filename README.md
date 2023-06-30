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
- pytorch
- scikit-learn

In addition we used a [custom wrapper for huggingface embeddings](https://github.com/UBI-AGML-NLP/Embeddings).


## Dataset

The paper investigates the quality of the [BIOS dataset](https://github.com/microsoft/biosbias). The dataset is not published due to copyright and data protection reasons. However, a crawler to download the dataset can be found [here](https://github.com/microsoft/biosbias). The results from our review can be found in ```data/BIOS_LABELS.csv```. It contains no sensitive information such as the biographies themselves or names, but only the original ids, labels and paths as well as our review results to allow people to merge it with the BIOS dataset.

The following columns from the original dataset were kept:
- path
- gender
- start\_pos
- URI
- auto\_raw\_title (renamed from raw\_title)
- auto\_title (renamed from title)

These columns refer to our review:
- review (1 if the samples was reviewed, otherwise 0)
- raw\_titles (a list of raw\_titles determined by the annotator)
- titles (a list of titles derived from the raw titles and the ```title_lookup.json```, including only occupations used in the paper)
- valid (1 if the annotator decided the sample was a valid biography (see paper)) otherwise 0 (or -1 for samples not reviewed))
- style\_valid (1 if the annotator decided the text style matched a biography, in addition to being valid, otherwise 0 (or -1 if not reviewed/ invalid))


## Reproducing the paper

#### Finding candidate samples for review
Use ```filter_dataset.ipynb``` to obtain a selection of approx. 20.000 potentially problematic samples we used for the review.

#### Add review labels to dataset
If you downloaded the BIOS dataset yourself, you may use ```add_changes_to_dataset.ipynb``` to add our labels to the dataset. Move the downloaded ```BIOS.pkl``` to the ```data/``` directory or changes the path in the notebook.

#### Experiments from the paper
To reproduce our experiments, you first need to download the dataset yourself and merge our labels (see above). Then you can use ```data_review.ipynb``` to run our experiments. This includes training 30 BERT models, so it may take a while.
