### 编译

```shell
cmake . -G "Visual Studio 16 2019" -A x64 -T host=x64 -DWITH_GPU=ON -DWITH_MKL=ON -DCMAKE_BUILD_TYPE=Release -DCUDA_LIB="C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.2\lib\x64" -DCUDNN_LIB="C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.2\lib\x64" -DPADDLE_DIR="E:\fluid_inference_install_dir" -DOPENCV_DIR="G:\opencv"
```



```shell
cmake . -G "Visual Studio 16 2019" -A x64 -T host=x64 -DWITH_GPU=ON -DWITH_MKL=ON -DCMAKE_BUILD_TYPE=Release -DCUDA_LIB="C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.2\lib\x64" -DCUDNN_LIB="C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.2\lib\x64" -DPADDLE_DIR="E:\paddle_inference_install_dir" -DPADDLE_LIB_NAME="paddle_inference" -DOPENCV_DIR="G:\opencv"
```

```shell
cmake . -G "Visual Studio 16 2019" -A x64 -T host=x64 -DWITH_GPU=ON -DWITH_MKL=ON -DCMAKE_BUILD_TYPE=Release -DCUDA_LIB="C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.2\lib\x64" -DCUDNN_LIB="C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.2\lib\x64" -DPADDLE_DIR="E:\paddle_detect_project\paddle_inference" -DPADDLE_LIB_NAME=paddle_inference -DOPENCV_DIR="E:\paddle_detect_project\opencv"
```

