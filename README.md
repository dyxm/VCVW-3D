# VCVW-3D

[Paper](https://www.sciencedirect.com/science/article/pii/S0952197624001222) | [Citation](#citation) | [Dataset](#download)
## Introduction
This repository is the **V**irtual **C**onstruction **V**ehicles and **W**orkers dataset with 3D annotations (VCVW-3D), which covers 15 construction scenes and involves ten categories of construction workers and heavy vehicles. Each construction scene contributes 20,000 and 5,000 images of 1920x1080 for the Trainval and test sets, annotated with **2D/3D bounding boxes**, **2D semantic/instance segmentation**, and **depth maps**. The automatic data synthesis programs were developed based on the Unity 3D platform and the [Perception](https://github.com/Unity-Technologies/com.unity.perception) packages.

- **Prefabricated assets of ten categories.**
![figure_1](https://github.com/dyxm/VCVW-3D/assets/17799440/ba8784e5-b92e-4351-a6c5-0d0c75bf7c3b)

- **The considered construction scenes.**
![figure_2](https://github.com/dyxm/VCVW-3D/assets/17799440/0bf45c5c-07fd-4538-ac22-6e0174898ce7)

- **Sample images of each construction scene with various annotations (3D/2D bounding box and semantic segmentation).**
![figure_3](https://github.com/dyxm/VCVW-3D/assets/17799440/61a832d6-f664-4b7c-a435-4e5eefd25079)

- **Binocular stereo vision and depth map**
![figure_4](https://github.com/dyxm/VCVW-3D/assets/17799440/73b06b44-f6a8-4c58-ab7e-63ae90143dad)


## Data annotation
Each captured image has an annotation record, including the record ID, sensor information, image save path, and specific annotation information for different CV tasks. See [this](https://github.com/Unity-Technologies/com.unity.perception/blob/main/com.unity.perception/Documentation~/Schema/PerceptionSchema.md) for more detailed information.
```
"captures": [
{
    "id":              <str>  --  Unique record ID.
    "seq_id":          <str>  --  Sequence ID for binocular vision annotation, indicating which two item records were captured at the same timestamp.
    "sensor":          <obj>  --  Details of the sensor, refer to Table 4.
    "filename":        <str>  --  Filename of the captured image (e.g., 001.png).
    "annotations":     [<obj>, ...]  --  Annotations of this capture, refer to Table 5.
},
{…}
]
```

The main fields in the "sensor" item include the camera's translation, rotation, and intrinsic parameters.
```
"sensor ": {
    "sensor_id":             <str>  --  Sensor ID.
    "translation":           <float, float, float>  --   Sensor position(x, y, z) in meters, with respect to the global coordinate system. 
    "rotation":              <float, float, float, float>  --  Quaternion (w, x, y, z) of sensor orientation, with respect to the global coordinate system.
    "camera_intrinsic":      <3x3 float matrix>  --  Intrinsic camera calibration.
}
```

The labeled information was different for different CV tasks. For the 3D object detection task, the label ID, label name, translation, 3D size, and rotation of objects are annotated. Note that the position and rotation of the camera were based on the global coordinate system, while the annotated 3D bounding boxes are based on the camera coordinate system. In this dataset, the global coordinate system was set as the translation of (0, 0, 0) and rotation of (1, 0, 0, 0). 
- 3D bounding box annotation
```
{
    "id":                         <str>  --  ID, indicating current task (e.g., 3D bounding box).
    "values": [
        {
            "label_id":           <int>  --  Integer ID of the label.
            "label_name":         <str>  --  String label name.
            "instance_id":        <int>  --  Unique ID for each object instance.
            "translation":        <float, float, float>  -- Center location of the 3D bounding box in meters, with respect to the sensor's coordinate system.
            "size":               <float, float, float>  -- Width, height, and length of the 3D bounding box in meters.
            "rotation":           <float, float, float, float>  -- Orientation (w, x, y, z) of the 3D bounding box, with respect to the sensor's coordinate system.
            "visible_pixels":     <float>  -- The sum of visible pixels of the instance in the captured 2D image.
        },
      {…}
    ]
}
```
- Depth map
```
{
    "id":              <str>  --  ID, indicating current task (e.g., Depth map).
    "filename":        <str>  --  Filename of the 16-bit depth map (e.g., depth_map.raw).
}

```
- 2D bounding box
```
{
    "id":                          <str>  --  ID, indicateing current task (e.g., 2D bounding box).
    "values": [
        {
            "label_id":            <int>  --  Integer ID of the label.
            "label_name":          <str>  --  String label name.
            "instance_id":         <int>  --  Unique ID for each object instance.
            "x":                   <float >  -- Upper left corner's x coordinate of the 2D bounding box.
            "y":                   <float>  -- Upper left corner's y coordinate of the 2D bounding box.
            "width":               <float>  -- Width of the 2D bounding box in pixels.
            "height":              <float>  -- Height of the 2D bounding box in pixels.
            "visible_pixels":      <float>  -- The sum of visible pixels of the instance in the captured 2D image.
        },
        {…}
    ]
}

```
- 2D semantic segmentation
```
{
    "id":              <str>  --  ID, indicating current task (e.g., Semantic segmentation).
    "filename":        <str>  --  Filename of the color mask image (e.g., segmentation.png).
}

```
- 2D instance segmentation
```
{
    "id":                            <str>  --  ID, indicating current task (e.g., Instance segmentation).
    "filename":                      <str>  --  Filename of the color mask image (e.g., instance.png).
    "values": [
          {
              "label_id":            <int>  --  Integer ID of the label.
              "label_name":          <str>  --  String label name.
              "instance_id":         <int>  --  Unique ID for each object instance.
              "color":               <float, float, float, float>  -- Value from 0 to 1 for the red, green, blue, and alpha channels to mask the instance.
              "visible_pixels":      <float>  -- The sum of visible pixels of the instance in the captured 2D image.
          },
          {…}
    ]
}

```

## Download
The **VCVW-3D** dataset and the **original Unity project** can be accessed:
- at the [TeraBox download link](https://terabox.com/s/1b_csjQEEybl3Aio1DJukaA). **Recommended!**
- or, at the [Baidu Netdisk download link](https://pan.baidu.com/s/1vg4jbh-RGZXSAvSjkA2T7g) using the code: **j80v**

The original Unity project can be accessed at the links above, you can also customize and generate the data you want.

## Citation
If you find this dataset useful, consider citing it using:
```
@article{DING2024107964,
    title = {A virtual construction vehicles and workers dataset with three-dimensional annotations},
    journal = {Engineering Applications of Artificial Intelligence},
    volume = {133},
    pages = {107964},
    year = {2024},
    issn = {0952-1976},
    doi = {https://doi.org/10.1016/j.engappai.2024.107964},
    author = {Yuexiong Ding and Xiaowei Luo}
}

@article{ding2023vcvw,
  title={VCVW-3D: A Virtual Construction Vehicles and Workers Dataset with 3D Annotations},
  author={Yuexiong Ding and Xiaowei Luo},
  journal={arXiv preprint arXiv:2305.17927},
  year={2023}
}
```
