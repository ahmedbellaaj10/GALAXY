# GALAXY
This repository contains code and data for the **AAAI'2022** paper "**GALAXY: A Generative Pre-trained Model for Task-Oriented Dialog with Semi-Supervised Learning and Explicit Policy Injection**".

Full version with Appendix: [[PDF]](https://arxiv.org/abs/2111.14592)

## Abstract
Pre-trained models have proved to be powerful in enhancing task-oriented dialog systems. However, current pre-training methods mainly focus on enhancing dialog understanding and generation tasks while neglecting the exploitation of dialog policy. In this paper, we propose GALAXY, a novel pre-trained dialog model that explicitly learns dialog policy from limited labeled dialogs and large-scale unlabeled dialog corpora via semi-supervised learning. Specifically, we introduce a dialog act prediction task for policy optimization during pre-training and employ a consistency regularization term to refine the learned representation with the help of unlabeled dialogs. We also implement a gating mechanism to weigh suitable unlabeled dialog samples. Empirical results show that GALAXY substantially improves the performance of task-oriented dialog systems, and achieves new state-of-the-art results on benchmark datasets: In-Car, MultiWOZ2.0 and MultiWOZ2.1, improving their end-to-end combined scores by 2.5, 5.3 and 5.5 points, respectively. We also show that GALAXY has a stronger few-shot ability than existing models under various low-resource settings.

## Main Results
GALAXY perform end-to-end dialog modeling and achieve new state-of-the-art results on four TOD benchmark datasets: MultiWOZ2.0, MultiWOZ2.1, In-Car Assistant and CamRest.

| End-to-End Modeling | Inform | Success |  BLEU | Combined Score |
|:-------------------:|:------:|:-------:|:-----:|:--------------:|
|     MultiWOZ2.0     |  94.40 |  85.30  | 20.50 |     110.35     |
|     MultiWOZ2.1     |  95.30 |  86.20  | 20.01 |     110.76     |

| End-to-End Modeling | Match | SuccF1 |  BLEU | Combined Score |
|:-------------------:|:-----:|:------:|:-----:|:--------------:|
|   In-Car Assistant  | 85.26 |  83.60 | 23.03 |     107.46     |
|       CamRest       | 98.50 |  87.73 | 24.15 |     117.26     |

## Requirements
```
- torch == 1.8.0+cu111
- scikit-learn == 0.23.1
- numpy == 1.18.5
- nltk == 3.5
- spacy == 2.3.5
- scipy == 1.5.0
- regex == 2020.6.8
- tqdm == 4.60.0
```
We use the tokenization tool in SpaCy and you can directly install python packages by commands: `pip install -r requirements.txt` and `python -m spacy download en_core_web_sm`.

## Pre-training
### Pre-training Corpora
- [UniDA](https://drive.google.com/file/d/1t7YaaZ0niVcypFIi-3P8s9zKCh7Zs3aN/view?usp=sharing): a new labeled dialog dataset consisting of 975,780 utterances, which are annotated with 20 frequently-used DAs, according to our proposed comprehensive unified DA taxonomy for task-oriented dialog.
- [UnDial](https://drive.google.com/file/d/1t7YaaZ0niVcypFIi-3P8s9zKCh7Zs3aN/view?usp=sharing): a large-scale unlabeled dialog dataset consisting of 35M utterances with careful processing, ranging from online forum chatting logs to customer service conversations.

### Pre-trained Checkpoint
- [GALAXY](https://drive.google.com/file/d/16WolpMhg5bRIETuqQpENBBGndCYelaxA/view?usp=sharing): an uncased model with DA classification head (12-layers, 768-hidden, 12-heads, 109M parameters)

You need to unzip the downloaded model file `model.zip`, then put the unzipped directory `model` into the project directory `GALAXY` for the futhuer fine-tuning.

## Fine-tuning
### Data Preparation
Download data from this [link](https://drive.google.com/file/d/1Spb48PwH1vIyRIR1gCkcJ3f-aIsIsuXx/view?usp=sharing). 

The downloaded zip file `data.zip` contains four TOD benchmark datasets: MultiWOZ2.0, MultiWOZ2.1, In-Car Assistant and CamRest, which have already been processed. You need to put the unzipped directory `data` into the project directory `GALAXY` for the subsequent training.

### Fine-tuned Checkpoints
Download checkpoints from this [link](https://drive.google.com/file/d/1158aGRryHNX7YdH_HV-YAEEksatoxNUj/view?usp=sharing). 

The downloaded zip file `outputs.zip` contains our best fine-tuned checkpoints on different datasets: 
- the **7-th** epoch on MultiWOZ2.0 (**60** training epochs in total)
- the **5-th** epoch on MultiWOZ2.1 (**60** training epochs in total)
- the **89-th** epoch on In-Car Assistant (**100** training epochs in total)
- the **18-th** epoch on CamRest (**60** training epochs in total)

If you want to reproduce our reported results, you should put the unzipped directory `outputs` into the directory `${SAVE_ROOT}` (set in scripts). 
Then you can directly run the inference scripts of different datasets for the reproduction, which will be introduced later.

### Training
We fine-tune the GALAXY on the four TOD datasets and focus on the end-to-end dialog modeling (**E2E**) task.
You can fine-tune GALAXY from scratch by running the following training scripts:

```sh
# Training on MultiWOZ2.0
sh scripts/multiwoz2.0/train.sh

# Training on MultiWOZ2.1
sh scripts/multiwoz2.1/train.sh

# Training on In-Car Assistant
sh scripts/kvret/train.sh

# Training on CamRest
sh scripts/camrest/train.sh
```
> **NOTE**: For MultiWOZ2.0 and MultiWOZ2.1, we also maintain the DA prediction task to alleviate the model discrepancy between pre-training and fine-tuning. On the other hand, we discard this task on the In-Car Assistant and CamRest due to the lack of useful DAs in these two datasets.

### Inference
After collecting some fine-tuned checkpoints (by directly using ours or fine-tuning GALAXY from scratch by yourself), you can do the inference on the test sets of these datasets by running the following inference scripts:

```sh
# Inference on MultiWOZ2.0
sh scripts/multiwoz2.0/infer.sh

# Inference on MultiWOZ2.1
sh scripts/multiwoz2.1/infer.sh

# Inference on In-Car Assistant
sh scripts/kvret/infer.sh

# Inference on CamRest
sh scripts/camrest/infer.sh
```
> **NOTE**: For reproduction, all the best hyper-parameters have already been set in corresponding scripts and you can follow them to run.

## Citation
If you use our code or find GALAXY useful in your work, please cite our paper as:

```bib
@inproceedings{He2022GALAXY,
  title={GALAXY: A Generative Pre-trained Model for Task-Oriented Dialog with Semi-Supervised Learning and Explicit Policy Injection},
  author={Wanwei He and Yinpei Dai and Yinhe Zheng and Yuchuan Wu and Zhen Cao and Dermot Liu and Peng Jiang and Min Yang and Feiling Huang and Luo Si and Jian Sun and Yongbin Li},
  year={2022}
}
```
