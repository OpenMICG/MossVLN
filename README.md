# MossVLN: Memory-Observation Synergistic Planning for Continuous Vision-Language Navigation

## Data Preparation

### Scenes: Matterport3D

Instructions copied from [VLN-CE](https://github.com/jacobkrantz/VLN-CE):

Matterport3D (MP3D) scene reconstructions are used. The official Matterport3D download script (`download_mp.py`) can be accessed by following the instructions on their [project webpage](https://niessner.github.io/Matterport/). The scene data can then be downloaded:

```Python
# requires running with python 2.7
python download_mp.py --task habitat -o data/scene_datasets/mp3d/
```

Extract such that it has the form `scene_datasets/mp3d/{scene}/{scene}.glb`. There should be 90 scenes. Place the `scene_datasets` folder in `data/`.

### Data and Trained Weights

- Waypoint Predictor: `data/wp_pred/check_cwp_bestdist*`
  - [ ]  For R2R-CE, `data/wp_pred/check_cwp_bestdist_hfov90` [[link\]](https://drive.google.com/file/d/1goXbgLP2om9LsEQZ5XvB0UpGK4A5SGJC/view?usp=sharing).

- Processed data, pre-trained weight, fine-tuned weight [[link\]](https://drive.google.com/file/d/1MWR_Cf4m9HEl_3z8a5VfZeyUWIUTfIYr/view?usp=share_link) from [ETPNav](https://github.com/MarSaKi/ETPNav).

  ```Python
  unzip etp_ckpt.zip    # file/fold structure has been organized
  ```

overall, files and folds are organized as follows:

```Python
MossVLN
├── data
│   ├── datasets
│   ├── logs
│   ├── scene_datasets
│   └── wp_pred
└── pretrained
    └── ETP
```

## Setup

### Installation

Follow the [Habitat Installation Guide](https://github.com/facebookresearch/habitat-lab#installation) to install [`habitat-lab`](https://github.com/facebookresearch/habitat-lab) and [`habitat-sim`](https://github.com/facebookresearch/habitat-sim). We use version [`v0.1.7`](https://github.com/facebookresearch/habitat-lab/releases/tag/v0.1.7) in our experiments, same as in the VLN-CE, please refer to the [VLN-CE](https://github.com/jacobkrantz/VLN-CE) page for more details. In brief:

1.Create a virtual environment. We develop this project with Python 3.6.

```Python
conda env create -f environment.yaml
```

2.Install `habitat-sim` for a machine with multiple GPUs or without an attached display (i.e. a cluster):

```python
conda install -c aihabitat -c conda-forge habitat-sim=0.1.7 headless
```

3.Clone this repository and install all requirements for `habitat-lab`, VLN-CE and our experiments. Note that we specify `gym==0.21.0` because its latest version is not compatible with `habitat-lab-v0.1.7`.

```Python
git clone git@github.com:OpenMICG/MossVLN.git
cd MossVLN
python -m pip install -r requirements.txt
pip install torch==1.9.1+cu111 torchvision==0.10.1+cu111 -f https://download.pytorch.org/whl/torch_stable.html
```

4.Clone a stable `habitat-lab` version from the github repository and install. The command below will install the core of Habitat Lab as well as the habitat_baselines.

```Python
git clone --branch v0.1.7 git@github.com:facebookresearch/habitat-lab.git
cd habitat-lab
python setup.py develop --all # install habitat and habitat_baselines
```

## Running

Finetuning and Evaluation

Use `main.bash` for `Training/Evaluation/Inference with a single GPU or with multiple GPUs on a single node.` Simply adjust the arguments of the bash scripts:

```Python
# for R2R-CE
CUDA_VISIBLE_DEVICES=0 bash run_r2r/main.bash train 2333  # training
CUDA_VISIBLE_DEVICES=0 bash run_r2r/main.bash eval  2333  # evaluation
CUDA_VISIBLE_DEVICES=0 bash run_r2r/main.bash infer 2333  # inference
```

