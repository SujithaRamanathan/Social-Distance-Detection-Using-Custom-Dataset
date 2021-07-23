# Social-Distance-Detection-Using-Custom-Dataset
A digital system which can minimise the work of officials who are involved in the work of enforcing social distancing norms among public.It will detect and provide warning if pedestrians do not maintain the minimum social distance.


![output1](https://user-images.githubusercontent.com/69512604/126737563-8d9b7ec3-d8c0-40e8-9378-392fedfa881a.gif)  

![output](https://user-images.githubusercontent.com/69512604/126742440-30811b54-f51a-4539-886a-c3575f847cf4.gif)


Social distancing in Real-Time using live video stream/IP camera in OpenCV.

Simple Theory
Object detection:

We will be using YOLOv3, trained on COCO dataset for object detection.
In general, single-stage detectors like YOLO tend to be less accurate than two-stage detectors (R-CNN) but are significantly faster.
YOLO treats object detection as a regression problem, taking a given input image and simultaneously learning bounding box coordinates and corresponding class label probabilities.
It is used to return the person prediction probability, bounding box coordinates for the detection, and the centroid of the person.

Distance calculation:

NMS (Non-maxima suppression) is also used to reduce overlapping bounding boxes to only a single bounding box, thus representing the true detection of the object. Having overlapping boxes is not exactly practical and ideal, especially if we need to count the number of objects in an image.
Euclidean distance is then computed between all pairs of the returned centroids. Simply, a centroid is the center of a bounding box.
Based on these pairwise distances, we check to see if any two people are less than/close to 'N' pixels apart.

Features
The following are examples of the added features. Note: You can easily on/off them in the config. options (mylib/config.py):

1. Real-Time alert:

If selected, we send an email alert in real-time. Use case: If the total number of violations (say 10 or 30) exceeded in a store/building, we simply alert the staff.
You can set the max. violations limit in config (Threshold = 15).
This is pretty useful considering the COVID-19 scenario.
Note: To setup the sender email, please refer the instructions inside 'mylib/mailer.py'. Setup receiver email in the config.

2. Threading:

Multi-Threading is implemented in 'mylib/thread.py'. If you ever see a lag/delay in your real-time stream, consider using it.
Threading removes OpenCV's internal buffer (which basically stores the new frames yet to be processed until your system processes the old frames) and thus reduces the lag/increases fps.
If your system is not capable of simultaneously processing and outputting the result, you might see a delay in the stream. This is where threading comes into action.
It is most suitable for solid performance on complex real-time applications. To use threading:
set Thread = True in the config.

3. People counter:

If enabled, we simply count the total number of people: set People_Counter = True in the config.
4. Desired violations limits:

You can also set your desired minimum and maximum violations limits. For example, MAX_DISTANCE = 80 implies the maximum distance 2 people can be closer together is 80 pixels. If they fell under 80, we treat it as an 'abnormal' violation (yellow).
Similarly MIN_DISTANCE = 50 implies the minimum distance between 2 people. If they fell under 50 px (which is closer than 80), we treat it as a more 'serious' violation (red).
Anything above 80 px is considered as a safe distance and thus, 'no' violation (green).

Main:

YOLOv3 paper: https://arxiv.org/pdf/1804.02767.pdf
YOLO original paper: https://arxiv.org/abs/1506.02640
YOLO TensorFlow implementation (darkflow): https://github.com/thtrieu/darkflow

