
# Road Extraction from Sattelite Imagery
A computer vision approach to extraction of road surfaces from high resolution sattelite imagery.

## Dependencies & Installation
- torch
- fastai
- Pillow

pip install -r requirements.txt

## Approach

Data exploration:
The data was first explored to find the following issues:
File count mismatch - There was a mismatch in the no of sattelite images and the ground truth provided, filtering those resulted in only 1100 common images.

Cutout imagery - About 30% of the data included cutout imagery, meaning atleast a section of the image was cutout and filled with white pixels. This was computed programmatically (Data Rectification.ipynb)

Less samples - In all, there were only about 1100 images suitable for training and validation.

Data Rectification:
To rectify the cutout images, a technique of superimposition was devised that allowed pasting of a suitable (non-cutout) image on top of the cutout image and a corresponding change in the road mask as well. This resulted in a final dataset of 1107 images without any cutouts in them.

Data Augmentation:
The data created from the above step was then augmented using only flip and rotate, as elastic distortions or zoom etc are rarely present in sattelite imagery sources that are orthorectified.

Training:
A Unet was chosen as the framework to train the segmentation problem since the upsampling-downsampling nature of the Unet is appropriate here.

The choice of the encoder backbone is a matter of experimentation and a Resnet101 model was chosen randomly. Additionally, attention gates were added to the Unet with the intuition that it might supress redundant activations over the cross connections.
The model was trained in two steps, where initially the frozen encoder model was trained with a large learning rate range and then the model layers were unfrozen and fine tuned.

Results:
| epoch | train_loss | valid_loss | iou      | time  |
|-------|------------|------------|----------|-------|
| 0     | 0.114710   | 0.112988   | 0.333184 | 32:30 |
| 1     | 0.113935   | 0.106455   | 0.356482 | 32:00 |
| 2     | 0.103538   | 0.211881   | 0.337066 | 32:00 |
| 3     | 0.095885   | 0.095804   | 0.416688 | 32:00 |
| 4     | 0.090760   | 0.111679   | 0.428327 | 31:53 |
| 5     | 0.093692   | 0.099806   | 0.349834 | 31:49 |
| 6     | 0.081051   | 0.090341   | 0.458630 | 31:42 |
| 7     | 0.096698   | 0.096900   | 0.417639 | 31:41 |
| 8     | 0.084360   | 0.093338   | 0.451955 | 31:38 |
| 9     | 0.081784   | 0.088551   | 0.479631 | 31:43 |
 

The notebooks below can be run using an appropriate kernel with the above dependencies present.

Results:
| Pixel Acc% | iou%  | f1Score |
|------------|-------|---------|
| 96.97      | 56.34 | 71.74   |

