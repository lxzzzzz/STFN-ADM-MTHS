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
