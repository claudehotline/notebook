### yolov3 mobilenet v3模型导出

```shell
python tools/export_model.py -c configs/yolov3/yolov3_mobilenet_v3_large_270e_voc.yml -o weights=https://paddledet.bj.bcebos.com/models/yolov3_mobilenet_v3_large_270e_voc.pdparams TestReader.inputs_def.image_shape=[3,320,320]
```

### yolov3 mobilenet v3 Lite模型转换

```shell
opt --model_file=./output_inference/yolov3_mobilenet_v3_large_270e_voc/model.pdmodel --param_file=./output_inference/yolov3_mobilenet_v3_large_270e_voc/model.pdiparams --optimize_out=./output_inference/yolov3_mobilenet_v3
```

### ppyolo-tiny 模型导出

```shell
python tools/export_model.py -c configs/ppyolo/ppyolo_tiny_650e_coco.yml -o weights=https://paddledet.bj.bcebos.com/models/ppyolo_tiny_650e_coco.pdparams
```

### ppyolo-tiny Lite模型转换

```shell
opt --model_file=./output_inference/ppyolo_tiny_650e_coco/model.pdmodel --param_file=./output_inference/ppyolo_tiny_650e_coco/model.pdiparams --optimize_out=./output_inference/ppyolo_tiny
```

