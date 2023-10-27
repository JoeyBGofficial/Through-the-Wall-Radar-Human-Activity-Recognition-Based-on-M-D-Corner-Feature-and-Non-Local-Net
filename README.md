# Through-the-Wall-Radar-Human-Activity-Recognition-Based-on-M-D-Corner-Feature-and-Non-Local-Net

## I. INTRODUCTION

**Motivation:** The reason we're willing to put in the effort to do this is because through-the-wall radar (TWR) human activity recognition is indeed a worthwhile research topic, which can provide non-contact, real-time human movement monitoring, with a wide range of potential applications in areas such as urban safety and disaster rescue. Besides, Open-source efforts can foster collaboration and innovation in the field, advancing the technology and enabling our work to benefit the wider research community.

![image](https://github.com/JoeyBGofficial/Through-the-Wall-Radar-Human-Activity-Recognition-Based-on-M-D-Corner-Feature-and-Non-Local-Net/assets/67720072/8f4e46fc-40c3-4781-8737-da50f28e4a10)
Fig. 1. Prospects for the application of through-the-wall radar human monitoring.

**Basic Information:** This repository is the open source code for our latest feasibility work: "Abnormal Human Activity Recognition Method Based on Micro-Doppler Corner Representation and Non-Local Mechanisms for Through-the-Wall Radar", submitted to Journal of Radars;

**Submitted Author:** Weicheng Gao;

**Email:** JoeyBG@126.com;

**Abstract:** Through-the-wall radar is able to penetrate walls and achieve indoor human target detection. Deep learning is commonly used to extract the micro-Doppler signature, which can be used to effectively identify abnormal human activities behind obstacles. However, when different testers are invited to generate the training set and test set, the test accuracy of deep-learning-based recognition method is low with poor generalization ability. Therefore, this paper proposes an abnormal human activity recognition method based on micro-Doppler corner features and Non-Local mechanism. In this method, Harris and Moravec detectors are utilized to extract the corner features on the radar image, and the corner feature dataset is established in this manner. Then, multi-link parallel convolutions and Non-Local mechanism are utilized to construct the global contextual information extraction network to learn the global distribution characteristics of image pixels. The semantic feature maps are generated by repeating the global contextual information extraction network four times. Finally, the predicted probabilities of human activities are obtained by multi-layer perceptron. Numerical simulations and experiments are conducted to verify the effectiveness of the proposed method, showing that the proposed method can identify sudden abnormal human activities, and improve the recognition accuracy, generalization ability and robustness.

**Corresponding Papers:**

[1] 

## II. TWR ECHO MODEL AND PREPROCESSING METHODS

### A. Theory in Simple 

The proposed method first converts the frequency-domain echo received by the radar to the time domain first, then extracts its baseband signal, and then concatenates it along the slow time dimension, and the resulting image is a range-time map (RTM). The Doppler-time map (DTM) is obtained by summing all range bins of the RTM and doing the short time fourier transform (STFT) along the slow time dimension. The target image after clutter and noise suppression is obtained by doing Moving Target Indication Filtering (MTI) and Empirical Modal Decomposition (EMD) on both RTM and DTM, respectively. Finally, the generation of $\mathbf{R^2TM}$ and $\mathbf{D^2TM}$ is realized by vertical axis stretching.

![回波模型](https://github.com/JoeyBGOfficial/Through-the-Wall-Radar-Human-Activity-Recognition-Based-on-M-D-Corner-Feature-and-Non-Local-Net/assets/67720072/2073ad58-2b97-4a75-9c2b-93b5bc7b631a)
Fig. 2. Flowchart of the proposed radar data preprocessing method.

### B. Codes Explanation (Folder: TWR Echo and Preprocessing Tools, Plots)

#### 1. Channel_Concatenation.m:

The function is used to concatenate color mapped $\mathbf{R^2TM}$ and $\mathbf{D^2TM}$ in channel direction.

**Input:** $\mathbf{R^2TM}$ and $\mathbf{D^2TM}$, with the size of $256 \times 256 \times 3$, respectively.

**Output:** Concatenated image with the size of $256\times 256\times 6$.

#### 2. Convert_Gray_to_RGB.m:

This function implements pseudo-color mapping for any input of grayscale image.

**Input:** Gray-scale image $\mathbf{I}$.

**Output:** Color image $\mathbf{I}_\mathrm{RGB}$.

#### 3. DTM_EMD.m:

This function implements the empirical modal decomposition processing on the DTM.

**Input:** $\mathbf{DTM}$ in matrix form.

**Output:** $\mathbf{DTM}_\mathrm{Processed}$.

#### 4. DTM_Generator.m:

This function implements the time-frequency analysis using STFT.

**Input:** $\mathbf{RTM}$ in matrix form, and max_resolution, which adjusts the resolution of frequency domain in the result of STFT.

**Output:** $\mathbf{DTM}$.

#### 5. DTM_MTI.m:

This function implements the moving target indication filter of DTM.

**Input:** $\mathbf{DTM}$ in matrix form.

**Output:** $\mathbf{DTM}_\mathrm{Declutter}$.

#### 6. DTM_SVD.m:

This function implements the sigular value decomposition filter of DTM.

**Input:** $\mathbf{DTM}$ in matrix form.

**Output:** $\mathbf{DTM}_\mathrm{Processed}$.

#### 7. RTM_EMD.m:

This function implements the empirical modal decomposition processing on the RTM.

**Input:** $\mathbf{RTM}$ in matrix form.

**Output:** $\mathbf{RTM}_\mathrm{Processed}$.

#### 8. RTM_MTI.m:

This function implements the moving target indication filter of RTM.

**Input:** $\mathbf{RTM}$ in matrix form.

**Output:** $\mathbf{RTM}_\mathrm{Declutter}$.

#### 9. RTM_SVD.m:

This function implements the sigular value decomposition filter of RTM.

**Input:** $\mathbf{RTM}$ in matrix form.

**Output:** $\mathbf{RTM}_\mathrm{Processed}$.

#### 10. emd.m:

This function is a tool function for the empirical modal decomposition algorithm. Sourced from MATLAB's native toolbox and fine-tuned.

**Input:** $\mathbf{x}$ in sequence signal form.

**Output:** Decomposed IMFs and variables of EMD processing corresponding to input $\mathbf{x}$.

#### 11. ind2rgb_tool.m:

This function is a tool function for the pseudo-color mapping algorithm. Sourced from MATLAB's native toolbox and fine-tuned.

**Input:** $\mathbf{Indexed}$ in 2D integer matrix form, and $\mathrm{Colormap}$ object in MATLAB standard colormap form, which should be an M-By-3 matrix.

**Output:** Mapped image $\mathbf{RGB}$.

#### 12. nextpow2.m:

This function is a implementation of finding the next neighboring power of $2$ of a input number. Sourced from MATLAB's native toolbox and fine-tuned.

**Input:** Number $n$.

**Output:** Neighboring power $p$ of $2$.

#### 13. svd_tool.m:

Main code of the singular value decomposition algorithm. Sourced from MATLAB's native toolbox and fine-tuned.

**Input:** Matrix $\mathbf{A}$.

**Output:** The left chord vectors $\mathbf{U}$, the singular value matrix $\mathbf{S}$, and the right chord vectors $\mathbf{V}$.

#### 14. D2TM_Generator.m:

Code for generating $\mathbf{D^2TM}$ by vertical axis stretching.

**Input:** Matrix $\mathbf{DTM}$.

**Output:** Stretched matrix $\mathbf{D^2TM}$.

#### 15. R2TM_Generator.m:

Code for generating $\mathbf{R^2TM}$ by vertical axis stretching.

**Input:** Matrix $\mathbf{RTM}$.

**Output:** Stretched matrix $\mathbf{R^2TM}$.

### C. Datafiles Explanation (Folder: TWR Echo and Preprocessing Tools, Plots)

#### 1. R2TM_D2TM_Clist.mat:

This data file stores the color maps used to generate $\mathbf{R^2TM}$ and $\mathbf{D^2TM}$, which is a tool that corresponds to the color maps in the paper.

## III. MICRO-DOPPLER CORNER DETECTION METHODS

### A. Theory in Simple 

The proposed method utilizes Harris model based detector to extract corner features on $\mathbf{R^2TM}$ and Moravec model based detector to extract corner features on $\mathbf{D^2TM}.

![微信截图_20231022155040](https://github.com/JoeyBGOfficial/Through-the-Wall-Radar-Human-Activity-Recognition-Based-on-M-D-Corner-Feature-and-Non-Local-Net/assets/67720072/381900b4-9590-497e-a9ce-2bb0c373190b)

Fig. 3. Detecting corner points on two types of radar images using two different detectors, respectively.

### B. Codes Explanation (Folder: MD Corner Detection)

#### 1. D2TM_Corner_Detector.m:

The function is used to achieve Moravec model based corner detection on $\mathbf{D^2TM}$.

**Input:** $\mathbf{D^2TM}$ in matrix form.

**Output:** Corner feature map of $\mathbf{D^2TM}$.

#### 2. R2TM_Corner_Detector.m:

The function is used to achieve Moravec model based corner detection on $\mathbf{R^2TM}$.

**Input:** $\mathbf{R^2TM}$ in matrix form.

**Output:** Corner feature map of $\mathbf{R^2TM}$.

#### 3. corner.m:

The function is the main body of the Harris model based corner detection algorithm. Sourced from MATLAB's native toolbox and fine-tuned.

**Input:** Image for detection.

**Output:** Harris based corner feature map and variables corresponding to the input image.

#### 4. gradient.m:

The function is the tool for calculating gradient on a vector or an image.

**Input:** Signal or image.

**Output:** Gradient map corresponding to the input signal or image.

### C. Datafiles Explanation (Folder: MD Corner Detection)

None.

## IV. CORNER FEATURE MAP RECOGNITION BASED ON NON-LOCAL NETWORK

### A. Theory in Simple 

The Non-Local mechanism is a technique used to capture long-range dependencies in an image or feature map. It allows the network to consider non-local relationships between pixels or regions, which is particularly useful for tasks like corner feature detection. To implement corner feature map recognition using a CNN with Non-Local mechanism, we start by designing a network architecture that incorporates Non-Local blocks. These blocks are inserted into the network's layers to capture global information and enhance the network's ability for sparse feature extraction.

![GC-ResNeXt网络](https://github.com/JoeyBGOfficial/Through-the-Wall-Radar-Human-Activity-Recognition-Based-on-M-D-Corner-Feature-and-Non-Local-Net/assets/67720072/d4eb1212-d16d-4245-89a3-ed3536225d05)

Fig. 4. Details of the proposed Non-Local neural network's structure.

### B. Codes Explanation (Folder: Non-Local Net (Matlab Version), Non-Local Net (Python Improved Version))

#### 1. NonLocal_Net.m:

The script achieves the MATLAB version of whole process of Non-Local Net construction, network data input, network data preprocessing, real-time training and validation.

#### 2. NonLocalNet.py:

The script achieves the Python, Paddlepaddle framework improved version of whole process of Non-Local Net construction, network data input, network data preprocessing, real-time training and validation. The code also supports command-line one-click replacement of network module structure. Part of the codes come from the work of nanting03, and are improved by us. The supporting functions are all placed in the NonLocalNet_Codes folder, each as follows:

TABLE I. FUNCTIONS OF NON-LOCAL NET IN PYTHON VERSION
| Names of the codes | Functions |
| ------------------ | --------- |
| non_local.py | Definition of Non-Local Modules Part. 1 |
| context_block.py | Definition of Non-Local Modules Part. 2 |
|nlnet.py | Definition of the Network |
|radar_har_dataset.py | Definition of the Dataset |
| config.py | Configurations |
| train_val_split.py | Splitting the Training and Validation Sets |
| train.py | Start Model Training |
| eval.py | Start Model Validation |

### C. Datafiles Explanation (Folder: Non-Local Net (Matlab Version), Non-Local Net (Python Improved Version))

#### 1. Hyperparam.mat:

The .mat file defines the hyperparameters and relationships between the various layers of the MATLAB version of the network and contains some of the information needed for training.

## V. NUMERICAL SIMULATIONS AND EXPERIMENTS

### A. Theory in Simple 

In the paper we focus on the visualization, comparison and ablation validation of the performance of the proposed method. The visualization contains feature maps, training process. Comparison contains accuracy, generalization, robustness, etc. The ablation addresses the design necessity of each module.

![实验场景示意图](https://github.com/JoeyBGOfficial/Through-the-Wall-Radar-Human-Activity-Recognition-Based-on-M-D-Corner-Feature-and-Non-Local-Net/assets/67720072/8f7cdc04-34e9-4cf0-9331-79413eb3b0b0)

Fig. 5. Schematic diagram of the simulation and experimental scenes.

### B. Codes Explanation (Folder: Confusion Matrices, SequenceNN, T-SNE Tools)

#### 1. Confusion_Matrix_Generator.m:

The code implements the input of a confusion matrix in matrix form and outputs the result of its visualization, with the option of a slower but aesthetically pleasing generation or a faster but simplified one.

#### 2. SequenceNN.m:

Here we provide a construction scheme that combines ResNet with sequential neural networks as a comparative reference of sorts. The method is not given in the paper, but is still valid.

#### 3. T_SNE.m:

Input multiple sets of images of different categories and output their scatterplots reduced to three dimensions by T-SNE algorithm.

### C. Datafiles Explanation (Folder: Confusion Matrices, SequenceNN, T-SNE Tools)

#### 1. Hyperparam.mat:

The .mat file defines the hyperparameters and relationships between the various layers of the MATLAB version of the sequential network and contains some of the information needed for training.

#### 2. TSNE_Clist.mat:

This data file stores the color maps used to generate T-SNE scatter Plots, which is a tool that corresponds to the color maps in the paper.

## VI. EXTRA TOOLS

In the Extra Preprocessing Tools folder of the project, we give a variety of different auxiliary signal, data processing tools for radar based human activity recognition. These include the classical FFT, HHT, TFD, WT, VMD algorithms, and so on. This part of the code is derived from the TWVD repository and slightly improved for processing reference.

