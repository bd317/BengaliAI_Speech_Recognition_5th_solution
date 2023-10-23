# bengali_5th_place_solution
Solution of the 5th of the Kaggle Bengali.AI speech recognition challenge

# Download the following datasets:

## CTC model
Competition training data
https://www.kaggle.com/competitions/bengaliai-speech/data
Competition meta data for training data
https://www.kaggle.com/datasets/imtiazprio/bengaliai-speech-train-nisqa

!unzip bengali-speech.zip
!mv bengali-speech/* data/

## Language model
IndicCorp v2:
License: MIT License (https://github.com/AI4Bharat/IndicBERT/blob/main/LICENSE)
https://objectstore.e2enetworks.net/ai4b-public-nlu-nlg/indic-corp-frozen-for-the-paper-oct-2022/bn.txt

!mv bn.txt language_model/base_files/

IndicCorp processed & tokenized (https://github.com/Open-Speech-EkStep/vakyansh-models#punctuation-models):
License: MIT License (same as above) (https://github.com/Open-Speech-EkStep/vakyansh-models/blob/main/LICENSE)
https://storage.googleapis.com/vakyansh-open-models/language_model_text/bengali.zip

!unzip bengali.zip
!mv bengali/* language_model/base_files/

OpenSLR 53:
License: Apache License 2.0 (https://github.com/danpovey/openslr/blob/master/LICENSE)
https://us.openslr.org/resources/53/utt_spk_text.tsv

!mv utt.spk_text.tsv language_model/base_files/

DL Sprint competition data:
https://www.kaggle.com/competitions/dlsprint/data

!unzip dl-sprint.zip
mv dl-sprint/train.csv dl-sprint/train_dl_sprint.csv

!mv dl-sprint/train_dl_sprint.csv language_model/base_files/


# CTC model training:

## Stage 1 training

Run preprocessing\filtering_v1_mos.ipynb

This notebook will filter the training data based on the mos scores calculated by the competition hosts and create train_21.csv and val_21.csv in the folder xy.

After run experiments\train_w2w_baseline_v7_v5_v3_v2.ipynb

This notebook will do stage 1 training. This model will be used to pseudo label the data and calculate a wer score in the next step

## Stage 2 training

Now run filtering_v2_wer.ipynb

It will filter calculate wer scores based on the previous model and filter the dataset with wer < 0.5. This enhances the quality of the training data.

Now we can train the final single models:

IndicWav2Vec backbone:
train_w2w_baseline_v35.ipynb
This model will be trained for 130k steps.

1b backbone:
train_w2w_baseline_v32.ipynb
This model will be trained for 210k steps.

## Ensemble training

Now the ensemble model can be trained:
train_w2w_baseline_v34_ensemble.ipynb

Use the 6k training step checkpoint

# Language model training:
Run language_model/language_model_current_v12.ipynb
Copy the unigram from the lms/new_model_arpa to lms/new_model_bin_mixed after creating the binary file.


# Inference:
Inference notebook is found here:
https://www.kaggle.com/code/benbla/5th-place-solution
