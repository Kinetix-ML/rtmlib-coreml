# rtmlib

rtmlib is a super lightweight library to conduct pose estimation based on [RTMPose](https://github.com/open-mmlab/mmpose/tree/dev-1.x/projects/rtmpose) **WITHOUT** any dependencies like mmcv, mmpose, mmdet, etc. 

## Installation

```shell
git clone https://github.com/Tau-J/rtmlib.git

pip install -e .
```

## Demo

Here is a simple demo to show how to use rtmlib to conduct pose estimation on a simgle image.

```python
import time

import cv2

from rtmlib import YOLOX, RTMPose

device = 'cpu'
img = cv2.imread('./demo.jpg')

det_model = YOLOX('./yolox_l.onnx',
                  model_input_size=(640, 640),
                  device=device)
pose_model = RTMPose('./rtmpose.onnx',
                     model_input_size=(192, 256),
                     device=device)

bboxes = det_model(img)
res = pose_model(img, bboxes=bboxes)

# visualize
for each in res:
    p = each['keypoints']
    K = p.shape[1]
    for i in range(K):
        img_show = cv2.circle(img_show,
                                (int(p[0, i, 0]), int(p[0, i, 1])),
                                2,
                                (0, 0, 255),
                                2)

cv2.imshow('img', img_show)
cv2.waitKey()
```

Here is also a demo to show how to use rtmlib to conduct pose estimation on a video.

```shell
python demo.py
```