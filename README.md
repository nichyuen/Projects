# <font color='green'>GreenEye</font>: Computer Vision for Preliminary Detection of Plant Disease

![Header](https://git.generalassemb.ly/nichyuen/capstone/blob/master/README%20images/hydroponic%20img.jpg)

---
## Introduction
---

### Background:
Singapore have been relying on imports to support for local food consumption needs with imports for more than 90% from more than 170 countries and regions. As of current state, the risk of food security in Singapore is not a major concern, in fact Singapore was ranked 1st on the [Global Food Security Index in 2019](https://www.corteva.sg/resources/singapore-media/global-food-security-index-2019-highlights-the-growing-threat-of-climate-change.html) and have been one of the top ranking countries as well.

However, the heavy reliance on food imports might pose risk in the future with the shifts in global trends. [Climate change have been influential in threatening global food security](https://www.ipcc.ch/srccl/chapter/chapter-5/#:~:text=Observed%20climate%20change,a%20risk.%20%7B5.2.2%7D) with more extreme weather conditions which decreases yield. Addtionally, global rise in population pressures for [higher demand for food](https://abcnews.go.com/International/world-faces-food-insecurity-crisis-global-population-reaches/story?id=93347906). With upcoming trends of lower supply and higher demand of food, the Singapore Food Agency (SFA) lauch an initiative, [30 by 30](https://www.ourfoodfuture.gov.sg/30by30/), to build up Singapore's agri-food industry’s capability and capacity to sustainably produce 30% of its nutritional needs by 2030. 

Existing plans include growing food overseas and locally as well as diversifying import sources where about [1% of Singapore's land](https://www.ourfoodfuture.gov.sg/30by30/#:~:text=With%20just%201%25%20of%20Singapore%E2%80%99s%20land%20set%20aside%20for%20farming%20given%20the%20many%20competing%20land%20needs) is use for domestic agricultural purposes. The sector could also face manpower challenges due to its physically demanding and nonconventional nature of the job. These factors could hinder the achievability of the goal leaving Singapore to future food security risk.

### Problem Statement:
- Increase agriculture production yield with:
    - reduce/maintain low manpower needs
    - reduce use of fungicides
    - limited landspace

The purpose of this project is to utilise Computer Vision to improve productivity with the consideration of the aforementioned constraints under the context of the use of [hydroponic farms](https://psci.princeton.edu/tips/2020/11/9/the-future-of-farming-hydroponics#:~:text=The%20vertical%20integration%20of%20plants,99%25%20while%20also%20increasing%20productivity.). With the use of images captured by the cameras setup in the farms, these images would be feed into the model to classify if the plant is infected with the fungal diseases. The monitoring of the crops aims to have early control before it spread across the farm as the [fungal spores are airborne and thrive in humid conditions](https://ipm.ucanr.edu/agriculture/floriculture-and-ornamental-nurseries/downy-mildew/#:~:text=Downy%20mildew%20spores%20are%20usually%20short%2Dlived%2C%20although%20they%20may%20survive%20several%20days%20under%20cool%2C%20moist%20conditions.%20They%20are%20airborne%2C%20and%20when%20they%20land%20on%20a%20susceptible%20plant%20with%20free%20water%20present%2C%20germination%20and%20infection%20generally%20occur%20within%208%20to%2012%20hours). Thus, the images are ideally captured every 4 hours (in the context of downy mildew) to capture potential symptoms where each image is tagged based on the camera location and can be use to track the location of the diseased plant.

### Objective:
To conclude as a success classification model, the **minimum accepting metric have to be at least at 90%** which would be deployable for smaller hydroponic farms as the spread would be less devasting as compared to a large hydroponic farm where the **acceptable metric would be at 95%**. The choice of metric would be Accuracy due to the similar quantity of validation data which would provide a reliable measure on the model's overall performance.

---
### Notebooks:
- 01_Cleaning&EDA
- 02_Modeling

### Data:
The image data and h5 files of model callbacks are located in google drive which can be access [here](https://drive.google.com/drive/folders/1HnPLc6HtejySnRiP5WkKGoh_Aj-815Kl?usp=sharing).

### Streamlit App:
Access to the streamlit app [here](https://greeneye-by-nicholas-yuen.streamlit.app/)

---
## Approach
---
### Data Acquisition
Images are sourced from an image database dedicated for computer vision projects, [Roboflow](https://universe.roboflow.com/) on pictures of healthy and diseased bok choy. In total of 460 images for training and 100 for validation.

### Preprocessing Steps
- [Histogram Equalization](https://staff.fnwi.uva.nl/r.vandenboomgaard/ComputerVision/LectureNotes/IP/PointOperators/HistogramEqualization.html): To enhance the image’s contrast, it spreads out the most frequent pixel intensity values or stretches out the intensity range of the image. By accomplishing this, histogram equalization allows the image’s areas with lower contrast to gain a higher contrast.

- Amplifying saturation for yellow hues in images
Visual symptoms of downy mildew involves yellow blemishes on the leaves. In attempt to assist the model in detection for such signals in the image, the saturation of yellow hues are amplified. Below illustrates the changes to the sample image.

### Modeling

General approach consist of selection of pre-trained [Convolutional Neural Network](https://www.techtarget.com/searchenterpriseai/definition/convolutional-neural-network#:~:text=A%20CNN%20is%20a%20kind,the%20network%20architecture%20of%20choice.) (CNN) and adjusments to its parameters and to top it off with a standardized self-defined fully connected layer.

How it works:
![CNN infographic](Chttps://git.generalassemb.ly/nichyuen/capstone/blob/master/README%20images/CNN%20infographic%20basic.png)

#### Choice of CNNs
1. [MobileNetV2](https://www.mathworks.com/help/deeplearning/ref/mobilenetv2.html)

- MobileNetV2 is a CNN architecture designed for mobile and embedded devices. It uses [depthwise separable convolutions](https://towardsdatascience.com/a-basic-introduction-to-separable-convolutions-b99ec3102728) to reduce computational complexity with a more efficient method used to convolute images.Thus it is choosen as it maintains accuracy and provide good balance between computational efficiency and performance in image classification tasks. 

- Infographic of this feature as shown below:
![Depthwise seperable info](https://git.generalassemb.ly/nichyuen/capstone/blob/master/README%20images/depthwise%20seperable%20convultion%20infograph.png)

2. [MobileNetV3](https://arxiv.org/pdf/1905.02244v5.pdf)

- MobileNetV3, an improved version from MobileNetV2, with the additional features of [Squeeze & Exicitation](https://towardsdatascience.com/squeeze-and-excitation-networks-9ef5e71eacd7) and [h-swish](https://arxiv.org/pdf/1905.02244v5.pdf) to further improve feature extraction of images and computational efficiency. 
    - Squeeze & Excitation: More detailed feature extraction from squeezing each channel of the image to a 1D vector with channel-wise information and through learning each channel's importance to assign respective weights to be use for rescaling on the features map. 
    - h-swish: Activation function that modifies the swish function, a function that is more robust than the ReLU due to its nonlinearity characteristic. The h-swish reduces computational cost as it avoids costly operations as compared to other activation functions.
    
- Infographic of the Squeeze & Excitation feature:
![Squeeze & Excitation](https://git.generalassemb.ly/nichyuen/capstone/blob/master/README%20images/squeeze_excite.png)

3. [InceptionResNetV2](https://www.mathworks.com/help/deeplearning/ref/inceptionresnetv2.html)

- InceptionResNetV2 architecture is a combination of the Inception-v4 and ResNet architectures, where it uses the Inception-v4's parallel convolutional branches and ResNet's residual blocks. The parallel convolutional branch aims to improve feature extraction and the residual blocks feature aims to balance computational efficiency as Inception's feature is computational heavy.
    - Parallel convolutional branches: Utilises different filter size for feature extraction and concatenate them. This enables extraction of both local details and broader context, leading to a richer representation of the input data.
    - Residual block: Shortcut connections that bypass layers in the network to create shorter gradient paths for faster training and better learning process as the information of the input are passed through these residual blocks along the training phase.
    
- Infographic of the parallel convolutional branch:
![Parallel Convolutional Branch](https://git.generalassemb.ly/nichyuen/capstone/blob/master/README%20images/inception.png)

- Infographic of residual block:
![Residual Block](https://git.generalassemb.ly/nichyuen/capstone/blob/master/README%20images/resblock.png)

### Model Performance Summary

|Approach|Description of Parameters|Performance|Conclusion|
|--|--|--|--|
|1|MobileNetV2 model <ul><li>CNN architecture:</li><ul><li>Average Pooling</li></ul><li>Fully Connected Layer:</li><ul><li>Loss function: Adam</li><li>Batch size: 32</li><li>Epochs:25</li></ul>|<li>Accuracy:</li><ul><li>Train: 0.96</li><li>Test: 0.97</li></ul><li>Loss:</li><ul><li>Train: 0.1257</li><li>Test: 0.1256</li></ul><li>Avg train time: 8secs</li>|**Best score** without overfitting on regular images but performed poorly on images with presence of water droplets (**27%** correctly classified as healthy and **numerous close to zero** prediction probabilities)|
|2|MobileNetV3 model <ul><li>CNN architecture:</li><li>Average Pooling</li><li>Fully Connected Layer:</li><ul><li>Loss function: Adam</li><li>Batch size: 32</li><li>Epochs:25</li></ul>|<li>Accuracy:</li><ul><li>Train: 0.94</li><li>Test: 0.94</li></ul><li>Loss:</li><ul><li>Train: 0.1796</li><li>Test: 0.1791</li></ul><li>Avg train time: 8secs</li>|**Acceptable score** without overfitting on regular images but performed poorly on images with presence of water droplets (**29%** correctly classified as healthy and **lesser close to zero** prediction probabilities)|
|3|MobileNetV3 model with Ridge regularization <ul><li>CNN architecture:</li><ul><li>Average Pooling</li><li>Ridge regularization alpha=0.01 </li></ul><li>Fully Connected Layer:</li><ul><li>Loss function: Adam</li><li>Batch size: 32</li><li>Epochs:30</li></ul>|<li>Accuracy:</li><ul><li>Train: 0.96</li><li>Test: 0.95</li></ul><li>Loss:</li><ul><li>Train: 0.1453</li><li>Test: 0.1462</li></ul><li>Avg train time: 8secs</li>|**Acceptable score** without overfitting on regular images but performed the worse on images with presence of water droplets (**2%** correctly classified as healthy and **lesser close to zero** prediction probabilities)|
|4|InceptionResNetV2 <ul><li>CNN architecture:</li><ul><li>Average Pooling</li></ul><li>Fully Connected Layer:</li><ul><li>Loss function: Adam</li><li>Batch size: 32</li><li>Epochs:20</li></ul>|<li>Accuracy:</li><ul><li>Train: 0.91</li><li>Test: 0.94</li></ul><li>Loss:</li><ul><li>Train: 0.2131</li><li>Test: 0.2085</li></ul><li>Avg train time: 38secs</li>|**Acceptable score** without overfitting on regular images but performed poorly as well on images with presence of water droplets (**15%** correctly classified as healthy and **numerous close to zero** prediction probabilities)|

---
## Conclusion/Future works
---
### Conclusion    
Overall, the various models have acheived the objective of classifying with an accuracy of at least 90% on regular images but have shown limited success on images with the presence of water droplets where the models have tendencies to classify it as a diseased plant.

MobileNetV2 have the best results in classifying regular images (with 97% accuracy) but is considered to be one of the worst performing models on images with water droplets present as most classification on water droplet images are confidently incorrect.

Nonetheless, the selected model/approach is the **MobileNetV2** as the CNN architecture used in **approach 1**. It is selected due to its best performance on regular image classifications and its short training time needed. 

Despite the model being poorer than other models in classifying images with water droplets being present, the problem holds less relevance in the context of hydroponic farms:
- As the roots of the plants are watered instead of watering from a top-down approach thus it is less likely to have water droplets being present on the leaves.
- Likely for the farms to be under shelter as the vegetable is [prone to stress for over excessive sunglight and sub-optimal conditions when the pH values are not in the range of 6.0 to 7.0](https://luv2garden.com/how-to-grow-hydroponic-bok-choy/). Thus, the vegetables are protected from rainfall.
    
### Limitations
    
|Limitations|Prospective resolutions|
|--|--|
|**Limited applicability on outdoor farms with use of soil**<ul><li>Performance of classifciation of images with disturbances such as water droplets/debris on leaves have been poor due to niche data available for training.|Gather relevant higher quality image data collected from farmers for model training and validation purposes<ul>Relevant quality images include:<ul><li> images that fit the context (i.e water droplets presence)</li><li>images with appropriate distance of the object</li><li>quality of images so as to better capture distinct features</li></ul></ul>|
|**Questionable classification of low light (night) images performance**<ul><br><li>As mentioned that the downy mildew spores takes 8 to 12 hours for germination and infection to occur, the need for round the clock monitoring is necessary which includes night imaging.</li><br><li>The images used in training are taken under considerably sufficient light(images also under went histogram equalization in attempt to improve low contrast images) but have yet to be tested on images with the presence of low light which can hinders the ability to extract features of the image and thus its classification capability.</li><br><li>Hardware solutions such as night vision imaging(eg. infrared imaging) might not be useful to provide quality images for training</li></ul>|Use of promising low-light preprocessing model, [Learning to See in the Dark](https://arxiv.org/pdf/1805.01934v1.pdf), to enhance low light images.|
|**Potential loss of accuracy with group images of vegetables** <ul><br><li>The images are analyse as a collective of neighbouring vegetables, there could be instances where only a single plant display symptoms and the rest are healthy. Such cases might not raise a flag and early control will not be timely implemented|Use of Object Detection to isolate individual plants from the image and interpret individually. Since the object in concern is relatively niche as compared to the common objects used in existing pre-trained models, collection of interested vegetable images and self-annotation is needed to train the Object Detection model. |
    
### Future works
    
- There are many other existing CNN architectures that extracts the image features differently that have yet to be tested to solve this problem which might produce better results. Therefore, it can be explore with the use of [Neural Architecture Search](https://towardsdatascience.com/what-is-neural-architecture-search-and-why-should-you-care-1e22393de461) to "gridsearch" over the best CNN architectures and even its parameters.
- The model in current state can detect decently on visual symptoms of diseased plants but not able to classify the type of disease based on specific/different symptoms which would be useful as different disease requires different course of actions to protect the crops. Additionally, similar works can be done on other common local produce and their common diseases.
- Image classification on the presence of common and harmful pests such as the [Diamondback Moth](https://www.sfa.gov.sg/food-for-thought/article/detail/new-method-to-control-dbm#:~:text=The%20DBM%20is%20one%20of%20the%20most%20destructive%20insect%20pests%20of%20brassica%20crops) which thrives in local conditions and pose a threat on the yield of vegetables.
- Scale the [current deployed app](https://greeneye-by-nicholas-yuen.streamlit.app/) to accept live feed of images to monitor crops as its current state only accept single images being passed manually which can be roll out to farmers for their self use.    