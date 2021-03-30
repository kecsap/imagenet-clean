# ImageNet Clean

This repository contains Bash scripts to clean up the ImageNet 1k dataset and pretrained Pytorch models in different configurations.

The Bash scripts can be downloaded from https://www.dropbox.com/s/pyzem2svhnx5h6m/imagenet_clean_scripts.tar.gz?dl=0.

Pytorch pretrained models can be downloaded from https://www.dropbox.com/s/lzm60bz90wfl6ys/imagenet_clean_models.tar.gz?dl=0. 

# Requirements

Download ImageNet 1k (https://academictorrents.com/details/943977d8c96892d24237638335e481f3ccd54cfb) and/or ImagenetV2 (http://imagenetv2public.s3-website-us-west-2.amazonaws.com/) datasets.
Bash to run the cleanup scripts.
ImageMagick to apply the categorical fixes.
Pytorch and Pytorch Image Models (https://github.com/rwightman/pytorch-image-models) to use the pretrained models.

# Clean up ImageNet 1k (Validation set)

Download and extract the scripts in a directory. Copy the imagenet_val_\*.sh scripts into the validation set subdirectory of the dataset (val/) and execute the scripts in the following order:

1. Fix image labels based on confident learning:

```
./imagenet_val_1_image_fixes.sh
```

2. Remove the wrong-problematic images based on model consensus and confident learning:

```
./imagenet_val_2_image_removal.sh
```

3. Apply categorical fixes:

```
./imagenet_val_3_categorical_fixes.sh
```

# Clean up ImageNet 1k (Training set)

Download and extract the scripts in a directory. Copy the imagenet_train_\*.sh scripts into the training set subdirectory of the dataset (train/) and execute the scripts in the following order:

1. Fix image labels based on confident learning:

```
./imagenet_train_1_image_fixes.sh
```

2. Remove the wrong-problematic images based on model consensus and confident learning:

```
./imagenet_train_2_image_removal.sh
```

3. Apply categorical fixes:

```
./imagenet_train_3_categorical_fixes.sh
```

Optional steps:

- Removing the wrong images only found by confident learning (a subset of point 2): imagenet_train_2_image_removal1.sh
- Removing the wrong images only found by model consensus (a subset of point 2): imagenet_train_2_image_removal3.sh
- Applying the fixes and removal before category fixes for CAE-EDSR images (https://github.com/hendrycks/imagenet-r/tree/master/DeepAugment) before category fixes: imagenet_train_cae_edsr_1_image_fixes.sh and imagenet_train_cae_edsr_2_image_removal.sh

Note: The CAE and EDSR scripts expect that CAE/EDSR images must be renamed to a new name schema (e.g. n01440764_10042.JPEG -> n01440764_10042_CAE.JPEG)

# Clean up ImageNetV2 Matched Frequency (Validation set)

Download and extract the scripts in a directory. Copy the imagenetv2_\*.sh scripts into the ImageNetV2 subdirectory and execute the scripts in the following order:

1. Fix image labels based on confident learning:

```
./imagenetv2_matched_frequency_format_1_image_fixes.sh
```

2. Remove the wrong-problematic images based on model consensus and confident learning:

```
./imagenetv2_matched_frequency_format_2_image_removal.sh
```

3. Apply categorical fixes:

```
./imagenetv2_matched_frequency_format_3_categorical_fixes.sh
```

Optional steps:

- Removing the wrong images only found by confident learning (a subset of point 2): imagenetv2_matched_frequency_format_2_image_removal1.sh
- Removing the wrong images only found by model consensus (a subset of point 2): imagenetv2_matched_frequency_format_2_image_removal3.sh

# Pretrained Pytorch models

The pretrained models have the following name schema:

model_name-widthxheight-variant.pth.tar

- model_name - efficientnet_b0, shufflenet_v2_x1_5 or squeezenet1_1
- variant - baseline (trained on original ImageNet), clean (trained on ImageNet Clean), clean-imagenet-r (trained on ImageNet Clean with CAE/EDSR images)

Install Pytorch Image Models:

```
pip3 install timm
```

# Pretrained Pytorch models (example validations)

Validate an EfficientNet-B0 model (trained on ImageNet Clean, portrait input 216x384) on ImageNetV2 dataset (top-1/top-5 - 63.21 %/84.18 %):

```
./validate.py --model efficientnet_b0 --checkpoint efficientnet_b0-384x216-clean.pth.tar -b 64 --log-interval 100 --input-size 3 216 384 --num-classes 1000 IMAGENETV2_DIRECTORY
```

Validate a SqueezeNet 1.1 model (trained on ImageNet Clean+CAE/EDSR, landscape input 320x180) on ImageNet validation dataset (top-1/top-5 - 60.89 %/83.15 %):

```
./validate.py --torchvision-model squeezenet1_1 --checkpoint squeezenet1_1-180x320-clean-imagenetr.pth.tar -b 64 --log-interval 100 --input-size 3 320 180 --num-classes 1000 IMAGENET_VALIDATION_DIRECTORY
```

Validate a ShuffleNetV2 (x1_5) model (trained on original ImageNet, standard input 224x224) on cleaned ImageNet validation dataset (top-1/top-5 - 77.93 %/94.57 %):

```
./validate.py --hub-model-github-or-dir kecsap/vision --hub-model shufflenet_v2_x1_5 --checkpoint shufflenet_v2_x1_5-224x224-baseline.pth.tar -b 64 --log-interval 100 --num-classes 1000 IMAGENET_VALIDATION_DIRECTORY
```
