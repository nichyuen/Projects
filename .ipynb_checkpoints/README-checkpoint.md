# <font color='green'>GreenEye</font>: Computer Vision for Preliminary Detection of Plant Disease
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

The purpose of this project is to utilise Computer Vision to improve productivity with the consideration of the aforementioned constraints under the context of the use of [hydroponic farms](https://psci.princeton.edu/tips/2020/11/9/the-future-of-farming-hydroponics#:~:text=The%20vertical%20integration%20of%20plants,99%25%20while%20also%20increasing%20productivity.). With the use of images captured by the cameras setup in the farms, these images would be feed into the model to classify if the plant is infected with the fungal diseases. The monitoring of the crops aims to have early control of the spread of the disease before it is widely spread across the environment. Each image is tagged based on the camera location and can be use to track the location of the diseased plant.

### Objective:
To conclude as a success classification model, the **minimum accepting metric have to be at least at 90%** which would be deployable for smaller hydroponic farms as the spread would be less devasting as compared to a large hydroponic farm where the **acceptable metric would be at 95%**. The choice of metric would be Accuracy due to the similar quantity of validation data which would provide a reliable measure on the model's overall performance.

---
### Notebooks:
- 01_Cleaning&EDA
- 02_Modeling
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

