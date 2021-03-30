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



