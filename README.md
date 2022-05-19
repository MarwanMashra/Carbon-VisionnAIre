# GreatAI Team

See Hackathon Website [Here](https://www.hi-paris.fr/2022/03/15/hickathon-2022-awards/).

## Team and project overview

<details open="open">
<summary><h3 style="display: inline-block">Team presentation</h2></summary>
We are "Great AI", a group of 6 students coming from different backgrounds with complementary skillsets. We divided ourselves into 2 teams, the technical team and the business team. <br>
Let us first present you the Technical Team. First, we have Fabien Roger, student in 2nd year in Cycle Ingenieur at Ecole Polytechnique. Working with him was Pierre-Antoine Arsaguet, engineer student in 2nd year at Telecom SudParis. To complete this amazing team, Marwan Mashra, 1st year master student at Paris-Saclay, was also working on the AI model. While Pierre-Antoine focused on the car detection, Fabien experimented with the identification of the car throughout its logo, and Marwan throught image similarity. They all collaborated throughout the whole process using different approaches to enhance the performance while mainting the frugality. <br>
On the other team were true business leaders, starting with Irina Tabacaru, 1st year master student in Managerial and Financial Economics at HEC Paris. She was working alongside with Georgi Ivanov, in the same program as her. Finally, Elsa Bismuth, 2nd year bachelor student at Ecole Polytechnique joined forces with them to develop a strong business plan. While Irina focused on the planning, Georgi on the animation, and Elsa on the branding, they worked together for the value creation and realization of the video.
</details>

<details open="open">
<summary><h3 style="display: inline-block">General strategy</h2></summary>
We developed an AI model using Computer Vision and capable of recognizing the model of a car based on images taken from a parking lot. Recognizing the model of the car, we were able to know its carbon footprint per kilometer. We developed a solution displaying the carbon footprint of the car and the total carbon emission of the ride when entering the parking lot, enabling the consumer to be aware of its carbon footprint while incentivizing him to lower his emissions.
</details>

## Scientific approach

<details open="open">
<summary><h3 style="display: inline-block">Approach description</h2></summary>
The problem was divided in two task : detecting the car, and estimating its CO2 emission. <br>
Since the frugality on our AI solution is a key factor for sustainability, our strategy was to take advatage of Transfer learning as much as possibe.

#### Car detection 

First, we tried to detect the bounding box of the car using a pretrained model of <b>YOLO</b> (state of the art for object detection). We compared the several versions of it, as well as a fine-tuned version, in order to find the best trade-off between performance and frugality to finally choose the YOLOR model. After analyizing the performance, we realized that the erros were due to the noise in some images. Therefore, we decided to implement a denoiosing algorithm based on scikit-image. Although using this algorithm improves the performance, it does increase the CPU inference time. Thus, we decided to not use it in the final pipeline.<br>

#### Estimating CO2 emission   
In the second part, we first attempted unsuccessfully to treat the task as a standard classification problem with classes being models of cars. However, we have 100 different class and the number of images per class varies between 4 and 25, with a median of 13 images per class. Therefore, it was clear that we needed to look at the task as a <b>Few-shot learning</b> probelm. Then, We tried to recognize the logo of the car using OpenCV in hope that it'll represent addititioanl information for our AI model. However, results of logo detection weren't sufficinlty accurate to be used is this task. Then, we switched to image similary approachs using a pretrained model. The idea is to get the embedding of each image of the training dataset from the last layer of a pretrained CNN. Then for a new given image, we get its embedding, compute its difference with the saved embeddings, and find the K closest image. After comparing the performance and the trainig/inference CPU time between several pretrained CNN, we end up chosing ResNet50. Farthermore, by computing the mean of CO2 emission of 5 closest cars, we were able to improve the performance.

Then to improve results even father as well as reduce the training time, we decided to use <a href="https://github.com/Alibaba-MIIL/ML_Decoder"><b>ML-Decoder: Scalable and Versatile Classification Head</b></a>, a pretrained CNN which is currenly considdered the state of the art for car classification. It allowed us to improve results sinificnlty without having to train the model. 

#### Final Pipeline
We use YOLOR to detect the bounding the box, and then ML-Decoder to detemine the model of the car to estimaate its CO2 emission.

</details>

<details open="open">
<summary><h3 style="display: inline-block">Future improvements</h2></summary>
Explain what next steps you envisage.
</details>


## Project usage

Explain the prerequisites, the environment setup and the procedure and guidelines to run the code / model.

In order to use our pipeline, you need to install the following github repository:
* <a href="https://github.com/WongKinYiu/yolor">YOLOR</a>
* <a href="https://github.com/JunnYu/mish-cuda">mish-cuda</a>
* <a href="https://github.com/fbcotter/pytorch_wavelets">pytorch_wavelets</a>
* <a href="https://github.com/alibaba-miil/ml_decoder">ML-Decoder</a>

as well as all the requirements of these repositories.


# III.	CARBON FOOTPRINT LIMITATION
During this project, we were very careful with several decisions taken to increase the frugality of our AI solution. From taking advatage of Transfere learning and pretrained models, to comparing several pretrainded models for performance and CPU run time and find the best trade-off. 


# IV.	CONCLUSION
Using Transfer learning, we think we acheived the goal of this hackathon which is to make the best trade-off between performance and frugality. Other approches, for increasing frugality without losing on the perfomance can implemented, like like knowledge distillation.
