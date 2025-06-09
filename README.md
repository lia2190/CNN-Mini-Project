The goal is to develop a binary image classifier to detect whether metastatic cancer tissue is present in small patches of histopathologic images.

Input:

image patches with .tif format.

Output:
Binary label:
1 → contains tumor tissue
0 → does not contain tumor tissue

Data:

train_labels.csv: CSV with image id and label (0 or 1)
train/ folder: contains .tif images
test/ folder: unlabeled images for submission

Challenge:

Small Medical images 
Class imbalance is possible
Need for high AUC ROC 
Exploratory Data Analysis (EDA):

Class balance: a plot shows slightly imbalanced data(more 0s than 1s)
Visualization of few images 
In the code, a validation is added to be sure that all labels have images

Analysis of the model:
ResNet-18 CNN pre-trained on ImageNet is used
Last layer was replaced with nn.Linear(in_features, 1) → binary output.
BCEWithLogitsLoss (binary classification loss) is used

Data pipeline:
Images resized to 32x32 
Normalization applied based on ImageNet mean/std.

Architecture (Resnet18):
Stage	Output Size	Layers
Conv1	16x16x64	7x7 Conv, stride=2 + MaxPool
Conv2_x	8x8x64	2 x BasicBlock(64)
Conv3_x	4x4x128	2 x BasicBlock(128)
Conv4_x	2x2x256	2 x BasicBlock(256)
Conv5_x	1x1x512	2 x BasicBlock(512)
AvgPool	1x1x512 → 512-dim vector	Global avg pool
FC	1	Linear(512→1) → BCE loss

Training procedure:

Split data into train / validation (80/20 split)

Adam optimizer is used (learning rate=1e-4)

Learning rate scheduler is used (ReduceLROnPlateau on validation AUC)

Train for 20 epochs and save the best model

Conslusion:
Good result because of Pre-trained ResNet-18.
Good training pipeline with scheduler and early stopping

Possible Improvements :
Use stronger models like ResNet-34 and ResNet-50



