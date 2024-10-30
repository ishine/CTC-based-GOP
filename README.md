# CTC-based-GOP
This repo relates to the paper "A Framework for Phoneme-Level Pronunciation Assessment Using CTC" for INTERSPEECH2024 <br />
{xinwei.cao, zijian.fan, torbjorn.svendsen, giampiero.salvi}@ntnu.no

## We fine-tuned a wav2vec2 model using "train-100-clean" from Librispeech and the model files are in the folder [models]

## In the folder:[generate-GOP] there are the scripts for generating the scalar GOPs
An example call: <br />
python gop-ctc-af-SD.py ./metadata/cmu-kids/cmu.ctm ./models/checkpoint-8000/  data/cmu-kids/metadata.csv ./models/processor_config_gop/ NONE ./test-gop-normnew/test

A1  the transcription in the ctm format: cmu.ctm
A2  the wav2vec2 model fine-tuned as phoneme recogniser: checkpoint-6500/
A3  the csv file for finding the speech data: metadata.csv
A4  the path to other affiliations of  the wav2vec2 model processor_config_gop/ (you can extract them after fine-tuning wav2vec2, most importantly config.json,  feature_extractor_config.json,  preprocessor_config.json,  tokenizer_config.json,  vocab.json)
A5  None (for CTC without training for SIL)
A6  the output path for writing the GOPs test-gop-normnew/test

## In the folder [generate-GOP-features], the script for genearting vector GOPs can be found
An example call: <br />
python gop-af-feats.py ./metadata/cmu-kids/cmu.ctm ./models/checkpoint-8000/ data/cmu-kids/metadata.csv ./models/processor_config_gop/ ./out-gops-cmu/cmu-enctc

## In the folder [evaluation], there are the scripts for evaluating the generated GOP/GOP-features on both dataset reported in the paper. 
An example call for evaluating the real errors of CMU-kids:<br />
python analyze_real_auc.py ./gops/cmu.gop ./metadata/cmu-kids/uttid.temp ./metadata/cmu-kids/label_mapped.txt ./cmu.json <br />

An example call for evaluating the simulated errors of CMU-kids: <br />
python sim-ctc-af-SDI.py ./metadata/cmu-kids/cmu.ctm ./models/checkpoint-8000/ ./data/cmu-kids/metadata.csv ./output/out_cmu_all_gv2.txt

And then: <br />
python cal_auc.py --filter ./error_uttid.txt output/out_cmu_all_gv2.txt ./filtered_out_cmu_all_gv2.json

Example calls for evaluting the Speakocean762: <br />
python evaluate_gop_scalar.py ./gops/so762_gv1.gop ./metadata.csv ./utt2dur_train ./utt2dur_test <br />
python evaluate_gop_feats.py ./gops/all_gop.feats ./metadata.csv ./utt2dur_train ./utt2dur_test

## The essential data related files for running the script can be found in the folder [data] and [metadata]

## The ASR-CTC training script is provided in the folder [ctc-ASR-training]
