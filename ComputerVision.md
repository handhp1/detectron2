## Dataset Folders Hierarchy
1. CocoData
./tools/datasets/coco/train2017/
./tools/datasets/coco/test2017/

2. Annotations
- Original COCO Data (NumClasses : 80)
./tools/datasets/coco/annotations/instances_train2017.json
./tools/datasets/coco/annotations/instances_val2017.json

- Refined COCO Data (NumClasses : 10) ['bicycle','car','motorcycle','airplane','bus','train','truck','boat','traffic light', 'bench']
./tools/datasets/coco/annotations/test_train2017.json
./tools/datasets/coco/annotations/test_val2017.json

- Seperated COCO Data (NumClasses : 10) ['bicycle','car','motorcycle','airplane','bus','train','truck','boat','traffic light', 'bench']
./tools/datasets/coco/annotations/test_occlude_val2017.json

3. GradCam Folders for test_train2017.json, test_val2017.json, test_occlude_val2017.json
This data is in \\mango.postech.ac.kr\common\Share\Meeting\ResearchMeetingMemo\2023\이재경\gradcam
./tools/datasets/coco/test_train2017_gradcam
./tools/datasets/coco/test_val2017_gradcam

## Model Config File
1. faster_rcnn_R_50_C4
./configs/COCO-Detection/faster_rcnn_R_50_C4_3x_class10_gradcam_coco.yaml
./configs/COCO-Detection/faster_rcnn_R_50_C4_3x_class10_gradcam_seperatecoco.yaml
./configs/COCO-Detection/faster_rcnn_R_50_C4_3x_class10_ori_coco.yaml
./configs/COCO-Detection/faster_rcnn_R_50_C4_3x_class10_ori_seperatecoco.yaml

2. faster_rcnn_R_50_FPN
./configs/COCO-Detection/faster_rcnn_R_50_FPN_3x_class10_gradcam_coco.yaml
./configs/COCO-Detection/faster_rcnn_R_50_FPN_3x_class10_gradcam_seperatecoco.yaml
./configs/COCO-Detection/faster_rcnn_R_50_FPN_3x_class10_ori_coco.yaml
./configs/COCO-Detection/faster_rcnn_R_50_FPN_3x_class10_ori_seperatecoco.yaml


## Train or Test Example
cd tools/
1. Train
./train_net.py --config-file ../configs/COCO-Detection/faster_rcnn_R_50_C4_3x_class10_gradcam_coco.yaml (training with gradcam)
./train_net.py --config-file ../configs/COCO-Detection/faster_rcnn_R_50_C4_3x_class10_ori_coco.yaml     (training without gradcam)

2. Evaluation
./train_net.py --config-file ../configs/COCO-Detection/faster_rcnn_R_50_C4_3x_class10_gradcam_coco.yaml  --eval-only MODEL.WEIGHTS ./output_c4_input_gradcam/         (evaluation with gradcam)
./train_net.py --config-file ../configs/COCO-Detection/faster_rcnn_R_50_C4_3x_class10_gradcam_seperatecoco.yaml  --eval-only MODEL.WEIGHTS ./output_c4_input_gradcam/ (evaluation with gradcam)
=> write.csv will be created. You can check mAP for evalaution models.

## Very very important Hardcording!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! ##
Modify below codes in ./detectron2/modeling/meta_arch/rcnn.py 
- If you use gradcam config file, you must use these codes
for i in range(len(images)):       
    images[i] = gradcam[i]*images[i]
- If you use original config file, you must comment like this.
#for i in range(len(images)):       
#    images[i] = gradcam[i]*images[i]