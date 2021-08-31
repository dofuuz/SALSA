# SALSA: Spatial Cue-AugmentedLog-Spectrogram Features for Polyphonic Sound Event Localization and Detection

<p align="center">
        <img src="figures/foa_salsa_16s_tight_with_text.png" title="SALSA feature for first-order ambisonics microphone (FOA)" width="48%"> 
        <img src="figures/mic_salsa_16s_tight_with_text.png" title="SALSA feature for microphone array (MIC)" width="48%">
        <em>Visualization of SALSA features of a 16-second audio clip in multi-source scenarios for 
            first-order ambisonics microphone (FOA) (left) and 4-channel microphone array (MIC) (right).</em>
</p>

Official implementation for Spatial Cue-AugmentedLog-Spectrogram Features **SALSA** for polyphonic sound event localization and detection.

<pre>
SALSA: Spatial Cue-AugmentedLog-Spectrogram Features for Polyphonic Sound Event Localization and Detection
Thi Ngoc Tho Nguyen; Karn N. Watcharasupat; Ngoc Khanh Nguyen; Douglas L. Jones; Woon-Seng Gan. 
</pre>

[[**ArXiv paper**]](https://arxiv.org/xxx)

## Introduction to Sound event localization and detection

Sound event localization and detection (SELD) is an emerging research field that unifies the tasks of 
sound event detection (SED) and direction-of-arrival estimation (DOAE) by jointly recognizing the sound classes, 
and estimating the directions of arrival (DOA), the onsets, and the offsets of detected sound events.
While sound event detection mainly relies on time-frequency patterns to distinguish different sound classes,
direction-of-arrival estimation uses amplitude and/or phase differences between microphones to estimate source directions.
As a result, it is often difficult to jointly optimize these two subtasks.

## What is SALSA?

We propose a novel feature called Spatial Cue-Augmented Log-Spectrogram (SALSA) with exact time-frequency mapping 
between the signal power and the source directional cues, which is crucial for resolving overlapping sound sources. 
The SALSA feature consists of multichannel log-linear spectrograms stacked along with the normalized principal 
eigenvector of the spatial covariance matrix at each corresponding time-frequency bin. Depending on the types of microphone array,
the principal eigenvector can be normalized differently to extract amplitude and/or phase differences between the 
microphones. As a result, SALSA features are applicable for different microphone array formats such as first-order 
ambisonics (FOA) and multichannel microphone array (MIC). 

Experimental results on the TAU-NIGENS Spatial Sound Events 2021 dataset with directional interferences showed that SALSA 
features outperformed other state-of-the-art features. Specifically, the use of SALSA features in the FOA format increased 
the F1 score and localization recall by 6% each, compared to the multichannel log-mel spectrograms with intensity vectors. 
For the MIC format, using SALSA features increased F1 score and localization recall by 16% and 7%, respectively, 
compared to using multichannel log-mel spectrograms with generalized cross-correlation spectra. 

![SELD performance](figures/SELD_performances_with_data_augmenation.png)

Our ensemble model trained on SALSA features ranked second in the team category of the SELD task in the 
[2021 DCASE Challenge](http://dcase.community/challenge2021/task-sound-event-localization-and-detection-results).

## Network architecture

todo

## Visualization of SELD output

todo

## Prepare dataset and environment

This code is tested on Ubuntu 18.04 with Python 3.7, CUDA 11.0 and Pytorch 1.7

1. Install the following dependencies by `pip install -r requirements.txt`. Or manually install these modules:
    * numpy
    * scipy
    * pandas
    * scikit-learn
    * h5py
    * librosa
    * tqdm
    * pytorch 1.7
    * pytorch-lightning      
    * tensorboardx
    * pyyaml
    * einops

2. Download TAU-NIGENS Spatial Sound Events 2021 dataset [here](https://zenodo.org/record/4844825). 
This code also works with TAU-NIGENS Spatial Sound Events 2020 dataset [here](https://zenodo.org/record/4064792). 

3. Extract everything into the same folder. 

4. Data file structure should look like this:

```
./
├── feature_extraction.py
├── ...
└── data/
    ├──foa_dev
    │   ├── fold1_room1_mix001.wav
    │   ├── fold1_room1_mix002.wav  
    │   └── ...
    ├──foa_eval
    ├──metadata_dev
    ├──metadata_eval (might not be available yet)
    ├──mic_dev
    └──mic_eval
```

## Feature extraction

Our code support the following features:  

| Name        | Format   | Component     | Number of channels |
| :---        | :----:   | :---          |  :----:            |
| melspeciv   | FOA      | multichannel log-mel spectrograms  + intensity vector    | 7 |
| linspeciv   | FOA      | multichannel log-linear spectrograms  + intensity vector    | 7 |
| melspecgcc  | MIC      | multichannel log-mel spectrograms  + GCC-PHAT    | 10 |
| linspecgcc  | MIC      | multichannel log-linear spectrograms  + GCC-PHAT   | 10 |
| **SALSA**   | FOA      | multichannel log-linear spectrograms  + eigenvector-based intensity vector (EIV)    | 7 |
| **SALSA**   | MIC      | multichannel log-linear spectrograms  + eigenvector-based phase vector (EPV)    | 7 |

Note: the number of channels are calculated based on four-channel inputs.

To extract **SALSA** feature, edit directories for data and feature accordingly in `tnsse_2021_salsa_feature_config.yml` in 
`dataset\configs\` folder. Then run `make salsa`

To extract *linspeciv*, *melspeciv*, *linspecgcc*, *melspecgcc* feature, 
edit directories for data and feature accordingly in `tnsse_2021_feature_config.yml` in 
`dataset\configs\` folder. Then run `make feature`

## Training and inference

To train SELD model with SALSA feature, edit the *feature_root_dir* and *gt_meta_root_dir* in the experiment config 
`experiments\configs\seld.yml`. Then run `make train`. 

To do inference, run `make inference`. To evaluate output, edit the `Makefile` accordingly and run `make evaluate`.

## Citation
Please consider citing our paper if you find this code useful for your research. Thank you!!!
```
@InProceedings{tho2021salsa,
author = {Nguyen, Thi Ngoc Tho and Watcharasupat, Karn N. and Nguyen, Ngoc Khanh and Jones, Douglas L. and Gan, Woon-Seng},
title = {SALSA: Spatial Cue-AugmentedLog-Spectrogram Features for Polyphonic Sound Event Localization and Detection},
booktitle = {xxx},
month = {xxx},
year = {2021}
}
```
