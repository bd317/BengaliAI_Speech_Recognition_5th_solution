# Bengali.AI Speech Recognition - 5th Place Solution
Solution of the 5th of the Kaggle Bengali.AI speech recognition challenge

# Download the following datasets:

## CTC model
Competition training data<br>
https://www.kaggle.com/competitions/bengaliai-speech/data

```!unzip bengali-speech.zip```<br>
```!mv bengali-speech/* data/```

Competition meta data for training data<br>
https://www.kaggle.com/datasets/imtiazprio/bengaliai-speech-train-nisqa

Download and place it in data/ as well.

## Language model
IndicCorp v2<br>
MIT License (https://github.com/AI4Bharat/IndicBERT/blob/main/LICENSE)<br>
https://objectstore.e2enetworks.net/ai4b-public-nlu-nlg/indic-corp-frozen-for-the-paper-oct-2022/bn.txt

```!mv bn.txt language_model/base_files/```

IndicCorp processed & tokenized (https://github.com/Open-Speech-EkStep/vakyansh-models#punctuation-models):<br>
MIT License (same as above) (https://github.com/Open-Speech-EkStep/vakyansh-models/blob/main/LICENSE)<br>
https://storage.googleapis.com/vakyansh-open-models/language_model_text/bengali.zip

```!unzip bengali.zip```<br>
```!mv bengali/* language_model/base_files/```

OpenSLR 53:<br>
Apache License 2.0 (https://github.com/danpovey/openslr/blob/master/LICENSE)<br>
https://us.openslr.org/resources/53/utt_spk_text.tsv

```!mv utt.spk_text.tsv language_model/base_files/```

DL Sprint competition data:<br>
https://www.kaggle.com/competitions/dlsprint/data

```!unzip dl-sprint.zip```<br>
```!mv dl-sprint/train.csv dl-sprint/train_dl_sprint.csv```<br>
```!mv dl-sprint/train_dl_sprint.csv language_model/base_files/```

# CTC model training:

## Stage 1 training

Run **preprocessing\filtering_v1_mos.ipynb**

This notebook will filter the training data based on the mos scores calculated by the competition hosts and create train_21.csv and val_21.csv in the folder data/.

After run **experiments\train_w2w_baseline_v7_v5_v3_v2.ipynb**

This notebook will do stage 1 training. The model will be used to pseudo label the data and calculate wer scores in the next step.

## Stage 2 training

Now run **filtering_v2_wer.ipynb**

It will calculate wer scores based on the previous model and filter the dataset for lower wer scores. This enhances the quality of the training data.

Now the final models can be trained:

IndicWav2Vec backbone:<br>
**train_w2w_baseline_v35.ipynb**<br>
This model will be trained for 210 steps.

1b backbone:<br>
**train_w2w_baseline_v32.ipynb**<br>
This model will be trained for 130k steps (longer training will give better results).

## Ensemble training

Now the ensemble model can be trained:<br>
**train_w2w_baseline_v34_ensemble.ipynb**

Use the 6k training step checkpoint.

# Language model training:
Run **language_model/language_model_current_v12.ipynb**<br>
Copy the unigram from the lms/new_model_arpa to lms/new_model_bin_mixed after creating the binary file.

# Punctuation model:
Punctuation model checkpoint was taken from:<br>
https://github.com/xashru/punctuation-restoration<br>
The checkpoint can be found here:<br>
https://drive.google.com/file/d/1X2udyT1XYrmCNvWtFpT_6jrWsQejGCBW/view?usp=sharing<br>
The inference function was slightly modified to ensure that the last predicted sign is | or ?. For details see the linked inference notebook below.

# Inference:
Inference notebook is found here:<br>
https://www.kaggle.com/code/benbla/5th-place-solution

### Notes
Training and val loss or WER scores may differ in earlier epochs because seed_everything was not set in the original version. However, the differences are negligible after a few thousand training steps.<br>
In the original version WandB was used to track the experiments. The flag was set to False in this repository. By commenting out the following line, tracking can be reactivated:<br>
os.environ["WANDB_DISABLED"] = "true"
