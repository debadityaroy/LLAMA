# SkyEye dataset
<h2>Dataset for analyzing lane-less traffic behavior at intersections </h2>

<img src="images/m2smart.png" width="200"> <img src="images/nihon.jpg" width="100"> <img src="images/iith.png" width="100">



The SkyEye dataset is the first aerial dataset for monitoring intersections with mixed traffic and lane-less behavior. Around 1 hour of video each from 4 intersections, namely, Paldi (P), Nehru bridge - Ashram road (N), Swami Vivekananda bridge - Ashram road (V), and APMC market (A) in the city of Ahmedabad, India.

**Paldi (P)**         | **Nehru Bridge Ashram Road (N)** 
----------------|--------------
![](images/paldi.png) |![](images/nehru.png)
4-way signalized intersection | 4-way signalized intersection
**Swami Vivekananda bridge - Ashram road (V)** | **APMC market (A)**
![](images/vivek.png) |![](images/apmc.png)
7-way signalized intersection | 3-way unsignalized intersection

These intersections were considered because of the diverse traffic conditions they present. 

The videos were captured using a DJI Phantom 4 Pro drone at 50 frames per second in 4K resolution (4096x2160). 

<h2> Annotation </h2>
There are 50,000 frames in total with 4,021 distinct road user tracks are annotated. A detailed breakdown is below:

**Number of unique road users**

Intersection | car | bus | motorbike | auto-rickshaw | truck | van | pedestrains
-|-|-|-|-|-|-|-
P | 175 | 54 | 881 | 494 | 45 | 16 | 226
V | 132 | 9 | 627 | 195 | 7 | 0 | 9 | 9
N | 41 | 8 | 275 | 99 | 12 | 6 | 33
A | 73 | 6 | 402 | 135 | 43 | 0 | 81
**Total** | **421** | **77** | **2185** | **971** | **107** | **22** | **349**

<h2> Downloads </h2>

The Skyeye dataset is available as images with bounding box annotations for _road user localization and type detection_ or videos with tracks extracted from every road user for _road-user tracking_. Additionally, we also provide labeled collision prone tracks 

<h3> Road user localization and type detection </h3>

* Dataset consists of 49,652 images in 4096x2160 [here](https://drive.google.com/drive/folders/109_fudeRF-cpTWWmML_9rkVm-k3g2W1s?usp=sharing) or sliced into 198,485 sliced images in 1920x1080 [here](https://drive.google.com/drive/folders/1-d6gXC_mcKV_7t1JLl0Knen4fi_9dlxp?usp=sharing)
* Annotations for 4096x2160 images (in Pascal VOC XML format) [here](https://drive.google.com/drive/folders/10IP94ibhl4RHXtvHp-pYf2vbOZkxaSv8?usp=sharing)
* Annotations for 1920x1080 images (in CSV format) [here](https://drive.google.com/drive/folders/1-_knE1XlAl1wxEfQjysXyRzL8Rtztzkl?usp=sharing)

<h3> Road-user Tracking </h3>

* Dataset consists of 5 videos that can be downloaded [here](https://drive.google.com/drive/folders/1160Xuo2920urD6btjQjQVVhQj9xENRWd?usp=sharing)
* Annotations (in MOT format) that can be downloaded [here](https://drive.google.com/drive/folders/10bhhdm2QPL6J4naxsYOs-f8t_OyhBbWL?usp=sharing) 

`frame_number, object_id, top_left_x, top_left_y, width, height, road_user_type`


Road User type | Name
-|-
1 | car
2 | bus  
3 | motorbike (includes all two-wheelers)
4 | autorickshaw
5 | truck
6 | van
7 | pedestrian 


<h2> Benchmarks</h2>

<h3>Road user localization and type detection</h3>

The [Retinanet](https://github.com/fizyr/keras-retinanet) architecure is trained for road user localization and type detection.

* The images are sliced into 1920x1080 tiles to train the [Retinanet](https://github.com/fizyr/keras-retinanet) architecture.
Download the sliced images [here](https://drive.google.com/drive/folders/1-d6gXC_mcKV_7t1JLl0Knen4fi_9dlxp?usp=sharing) (68.1 GB)
* The train, test, and validation CSV files for benchmarking are as follows - [train_annotations.csv](https://drive.google.com/file/d/1-iFERIqRHgeZaVD5bXzuPnFaQfqjseRa/view?usp=sharing), [test_annotations.csv](https://drive.google.com/file/d/1-nzAK9dSlxOU7ymMfOfxxp7RgQv3tftg/view?usp=sharing), and [val_annotations.csv](https://drive.google.com/file/d/1-eEcODW3iKYoDrrwxbIQeAiVf2LhYQim/view?usp=sharing).
* The trained weights for our Retinanet model are uploaded [here](https://drive.google.com/file/d/101WFyoSqlKPQm6NB-fxeCzGmpFgJhYXh/view?usp=sharing). We started with the [resnet50_coco_best_v2.1.0.h5](https://github.com/fizyr/keras-retinanet/releases/download/0.5.1/resnet50_coco_best_v2.1.0.h5) model.
* For replicating the results shown here, use the trained model given above with the [evaluate.py](https://github.com/fizyr/keras-retinanet/blob/master/keras_retinanet/bin/evaluate.py) script on the [test_annotations.csv](https://drive.google.com/file/d/1-nzAK9dSlxOU7ymMfOfxxp7RgQv3tftg/view?usp=sharing) file.

The **meanAP**  for the trained model is **0.8175**.

Road user type | Average Precision (AP)
-|-
car | 0.9747
bus | 0.9863
motorbike | 0.6136
autorickshaw | 0.9802
truck | 0.9568
van | 0.9695
pedestrian | 0.2413

<h4> Visualization </h4>

<img src="images/sample_detection.gif" width="1000">


<h3> Road-user Tracking </h3>

For tracking, the [SORT](https://github.com/abewley/sort) algorithm is evaluated as a preliminary benchmark. The user-defined detections were used for tracking. 

Video name | Precision | Recall| False Acceptance Rate (FAR)
-|-|-|-
Paldi 1 | 7.2 | 7.2 | 19.53
Vivek 1 | 15.9 | 15.9 | 17.12
Nehru 1 | 8.8 | 8.8 | 11.53
APMC 1 | 8.6 | 8.6 | 15.65
Paldi 2| 9.7 | 9.8 | 15.72

<h4> Visualization </h4>

<img src="images/sample_tracking.gif" width="1000">

Some more tracking output videos with the [DeepSORT](https://github.com/nwojke/deep_sort) tracker are available [here](https://drive.google.com/drive/folders/11g4EfbYTXBAlVnWjK2hehp_FX1vQPlKi?usp=sharing)


<h2> License </h2>

This dataset is provided for academic and research purposes only.

<h2> People </h2>

* [Debaditya Roy](https://sites.google.com/view/debadityaroy/), Post-doctoral Researcher, Dept. of TSE, Nihon University

* [Tetsuhiro Ishizaka](https://www.researchgate.net/profile/Tetsuhiro_Ishizaka), Associate Professor, Dept. of TSE, Nihon University

* [Atsushi Fukuda](https://www.researchgate.net/profile/Atsushi_Fukuda2), Professor, Dept. of TSE, Nihon University

<h3> Annotators </h3>

* Yusuke Doi (土井　悠輔), Bachelor student, Dept. of TSE, Nihon University
* Sho Matsunoshita (松野下　翔), Bachelor student, Dept. of TSE, Nihon University
* Daichi Tashiro (田代　大智), Bachelor student, Dept. of TSE, Nihon University
* Kaoru Kuga (空閑　香), Bachelor student, Dept. of TSE, Nihon University


<h2> Citation</h2>

If you use this dataset, consider citing one of our papers.

```
@INPROCEEDINGS{roy2020defining, 
author={D. {Roy} and Naveen Kumar {K.} and C. K. {Mohan}}, 
booktitle={2020 IEEE Intelligent Transportation Systems Conference (ITSC)}, 
title={Defining Traffic States based on Spatio-Temporal Traffic Graphs}, 
year={2020}, 
}


@misc{roy2019detection,
    title={Detection of Collision-Prone Vehicle Behavior at Intersections using Siamese Interaction LSTM},
    author={Debaditya Roy and Tetsuhiro Ishizaka and Krishna Mohan C. and Atsushi Fukuda},
    year={2019},
    eprint={1912.04801},
    archivePrefix={arXiv},
    primaryClass={cs.CV}
}
```

<h2> Acknowledgment </h2>

This  work  has  been  conducted  as  the  part  of  SATREPS project [M2Smart “Smart  Cities  development  for  EmergingCountries by Multimodal Transport System based on Sensing, Network  and  Big  Data  Analysis  of  Regional  Transportation”](http://m2smart.org/en/) (JPMJSA1606) funded by JST and JICA
