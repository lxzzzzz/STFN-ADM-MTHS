## Method

| ![space-1.jpg](asset/overview.png) | 
|:--:| 
| ***Fig 2. Overview of STFN-ADM-MTHS framework. (The tracking process follows a detection and tracking query joint decoding paradigm, the red font shows improvements. The initial input is the image at frame i, tracking results O_tr^i is the final results. First, the image at frame i is processed through ResNet-50 and the Encoder structure to learn 2D features, where the encoder procedure is implemented according to the reference [5] and different colors represent different vehicles. During decoding, q_det^i and q_tr^i vectors are uniformly initialized and will further optimized, initialized according to reference [7], the Last frame newborn with detection query self-attention incorporates the output O_det^(i-1) from frame i−1 to update q_det^i for frame i, combining Repulsion loss produces O_tr^i, the specific details are provided in section A and B. Finally, combining the i-1 frame’s outputs O_tr^(i-1), Spatial-guided cross-temporal feature aggregation captures motion vibration, the specific details are provided in section C. Utilizing the memory of frame i and the vibration learning from the previous step, M_tr^i vectors is initialized and will further optimized, the adaptive decay memory structure updates the frame i+1’s memory and query M_tr^(i+1) and q_tr^(i+1), the specific details are provided in section D.)* |


## Installation

**a. Create a conda virtual environment and activate it.**
```shell
conda create -n STFN python=3.8 -y
conda activate STFN
```

**b. Install PyTorch and torchvision**
```shell
conda install pytorch==1.13.1 torchvision==0.14.1 torchaudio==0.13.1 pytorch-cuda=11.7 -c pytorch -c nvidia
# Recommended torch>=1.9
```

**c. Install Other dependencies.**
```shell
conda install matplotlib pyyaml scipy tqdm tensorboard
pip install opencv-python
```

**d. Compile the Deformable Attention CUDA ops.**
```shell
cd ./models/ops/
sh make.sh
```

## Data

Dataset structure:
```
DATADIR/
  ├── MTHS-SEU/
  │ ├── train/
  │ ├── val/
  │ ├── train_seqmap.txt
  │ ├── val_seqmap.txt
  ├── UA-DETRAC/
  │ ├── train/
  │ ├── val/
  │ ├── test/
  │ ├── train_seqmap.txt
  │ ├── val_seqmap.txt
  │ └── test_seqmap.txt
  ├── KITTI/
  │ ├── train/
  │ ├── val/
  │ ├── test/
  │ ├── train_seqmap.txt
  │ ├── val_seqmap.txt
  │ └── test_seqmap.txt

```

### Training
Train MeMOTR on MTHS-SEU
```shell
python main.py --config-path ./configs/train_mths_seu.yaml --outputs-dir ./outputs/train_mths_seu/ --batch-size 1 --data-root <your data dir path>
```

### Evaluation
You can use this script to evaluate the trained model on the MTHS-SEU val set:
```shell
python main.py --mode eval --data-root <your data dir path> --eval-mode specific --eval-model <filename of the checkpoint> --eval-dir ./outputs/mths_seu/ --eval-threads <your gpus num>
```

## Acknowledgement
- [MeMOTR](https://github.com/MCG-NJU/MeMOTR.git).
- [Deformable DETR](https://github.com/fundamentalvision/Deformable-DETR)
- [DAB DETR](https://github.com/IDEA-Research/DAB-DETR)
- [MOTR](https://github.com/megvii-research/MOTR)
- [TrackEval](https://github.com/JonathonLuiten/TrackEval)
