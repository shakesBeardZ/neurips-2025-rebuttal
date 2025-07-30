# Experiments that we run for the rebuttal

## Augmentations ablation experiments

We will run an ablation study to evaluate the impact of different augmentation strategies on ReefNet's performance. The experiments will include:

1. Full augmentations(1 experiment):
    - All augmentations (RandAugment, Color Jitter, Horizontal Flipping, Mixup, CutMix, Random Erasing)

2. No augmentations (1 experiment):
    - No augmentations (baseline)

3. Only one augmentation (6 experiments):
    - Only Mixup
    - Only CutMix
    - Only RandAugment
    - Only Random Erasing
    - Only Horizontal Flipping
    - Only Color Jitter
4. Leave-one-out (remove one augmentation at a time)(6 experiments):
    - No Mixup
    - No CutMix
    - No RandAugment
    - No Random Erasing
    - No Horizontal Flipping
    - No Color Jitter

**commands**:

Only One Augmentation at a Time
```bash
./submit_single.sh only-mixup v100 4 -> Done
./submit_single.sh only-cutmix a100 4 -> Done
./submit_single.sh only-randerase a100 4 -> Done
./submit_single.sh only-randaug v100 4 -> Done
./submit_single.sh only-hflip a100 3 -> Done
./submit_single.sh only-colorjitter a100 4 -> Done
```
Leave-One-Out Ablations
```bash
./submit_single.sh no-mixup v100 4 -> Done
./submit_single.sh no-cutmix a100 3
./submit_single.sh no-randerase a100 4
./submit_single.sh no-randaug a100 4 -> Done
./submit_single.sh no-hflip a100 4 -> Done
./submit_single.sh no-colorjitter a100 4
```


# OSR experiments

OSR Vit done


## Todo:

1. test OSR
2. test OSR with CLIP
3. test OSR