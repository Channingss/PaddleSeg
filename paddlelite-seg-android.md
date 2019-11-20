# 安卓端人像分割

## 1.介绍
基于addleLite v2.0.0实现的人像分割Android示例`模型链接`，基于`数据`训练。以下第二节介绍如何使用demo，后面几章节介绍如何将PaddleSeg的Model部署到安卓等移动设备。
## 2.
### 2.1 要求
- Android Studio 3.4
- Android手机或开发版；

### 2.2 安装
- 打开Android Studio，在"Welcome to Android Studio"窗口点击"Open an existing Android Studio project"，在弹出的路径选择窗口中进入"PaddleLite-android-demo"目录，然后点击右下角的"Open"按钮即可导入工程
- 通过USB连接Android手机或开发板；
- 载入工程后，点击菜单栏的Run->Run 'App'按钮，在弹出的"Select Deployment Target"窗口选择已经连接的Android设备，然后点击"OK"按钮；
- 手机上会出现Demo的主界面，选择"Image Classification"图标，进入基于MobileNetV2的垃圾分类示例程序；
- 在垃圾分类Demo中，默认会载入一张一次性餐盒图像，并会在图像下方给出CPU的预测结果；
- 在垃圾分类Demo中，你还可以通过上方的"Gallery"和"Take Photo"按钮从相册或相机中加载测试图像；
### 2.3 效果展示
## 3.模型导出
[模型导出](https://github.com/PaddlePaddle/PaddleSeg/blob/release/v0.2.0/docs/model_export.md)
## 开发环境准备

## 4.模型转换
为了支持PaddleSeg模型在移动端的部署，首先需要准备PaddleLite使用的开发环境，在该开发环境中编译PaddleLite的预测库和模型转换工具，并对模型进行优化和转换。

### 4.1开发环境准备
PaddleLite目前支持Docker，Linux和Mac OS开发环境，建议使用Docker开发环境，以免存在各种依赖问题，具体环境的准备和编译方法参考[PaddLite源码编译](https://paddlepaddle.github.io/Paddle-Lite/v2.0.0/source_compile/)，同时Paddlelite [release](https://github.com/PaddlePaddle/Paddle-Lite/releases/)也发布了最新预编译版本的预测库和模型转换工具。

### 4.2模型转化

准备好开发环境，以及PaddleSeg导出来的模型和参数文件后，需要使用paddlelite提供的model_optimize_tool对模型进行优化，同时转换成PaddleLite支持的文件格式，这里有两种方式来实现：

注意：请在上一节准备好的开发环境中使用model_optimize_tool

1. 使用预编译版本的model_optimize_tool，最新的预编译文件参考[release](https://github.com/PaddlePaddle/Paddle-Lite/releases/)，此demo使用的版本为[model_optimize_tool v2.0.0](https://github.com/PaddlePaddle/Paddle-Lite/releases/download/v2.0.0/model_optimize_tool) 

2此demo使用的版本为[model_optimize_tool v2.0.0](https://github.com/PaddlePaddle/Paddle-Lite/releases/download/v2.0.0/model_optimize_tool) 

2. 手动编译model_optimize_tool

详细的模型转换方法参考paddlelite提供的官方文档：[模型转化方法](https://paddlepaddle.github.io/Paddle-Lite/v2.0.0/model_optimize_tool/https://paddlepaddle.github.io/Paddle-Lite/v2.0.0/model_optimize_tool/)，从PaddleSeg里面导出来的模型使用如下指令即可导出model.nb和param.nb文件。
```
./model_optimize_tool \
    --model_dir=<model_param_dir> \
    --model_file=<model_path> \
    --param_file=<param_path> \
    --optimize_out_type=naive_buffer \
    --optimize_out=<output_optimize_model_dir> \
    --valid_targets=arm \
    --prefer_int8_kernel=(true|false) \
```
##  5. 部署

