1. 运用https://github.com/tensorflow/models/blob/master/research/slim/export_inference_graph.py得到inception_resnet_v2_inf_graph_mura2cls.pb

   ```
   python ../export_inference_graph.py  \
   --model_name=inception_resnet_v2 \
   --output_file=/home-ex/tclhk/chenww/MY_TRAINING/T4/model/mura2Cls/inception_resnet_v2_inf_graph_mura2cls.pb \
   --alsologtostderr \
   --dataset_name=mura2Cls
   ```

2. 输入inception_resnet_v2_inf_graph_mura2cls.pb、ckpt，用freeze_graph得到frozen_inception_resnet_v2_13203.pb 

   ```
   CUDA_VISIBLE_DEVICES=3 /home-ex/tclhk/anaconda3/envs/pycenternet/bin/freeze_graph \
   --input_graph=/home-ex/tclhk/chenww/MY_TRAINING/T4/model/mura2Cls/inception_resnet_v2_inf_graph_mura2cls.pb  \
   --input_checkpoint=/home-ex/tclhk/chenww/MY_TRAINING/T4/model/mura2Cls/1126/model.ckpt-13203   \
   --output_graph=/home-ex/tclhk/chenww/MY_TRAINING/T4/model/mura2Cls/1126/frozen_inception_resnet_v2_13203.pb   \
   --input_binary=true \
   --output_node_names=InceptionResnetV2/Logits/Predictions
   ```

   