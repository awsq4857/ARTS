# [iOS音频频谱动画](https://juejin.im/post/5c1bbec66fb9a049cb18b64c)

[一](https://juejin.im/post/5c1bbec66fb9a049cb18b64c)、[二](https://juejin.im/post/5c26d44ae51d45619a4b8b1e)

原理：将音频的PCM流，通过傅里叶变换，分解成不同频率的正弦波。这些信号的频率和振幅就是我们需要实现动画的数据。

本文是个学习笔记，记录实现思路，后续用 Objective-C 把代码实现一遍。

## 音频解码

1. 音频播放。创建AVAudioEngine、AVAudioPlayerNode。
2. 音频数据获取。engine.mainMixerNode.installTap，获取数据流buffer

## 傅里叶变换

> 使用苹果的Accelerate框架，其中vDSP部分提供了数字信号处理的函数实现，包含FFT傅里叶变换。

1. 定义一个FFT的权重数组(fftSetup)，它可以在多次FFT中重复使用和提升FFT性能
2. 新建fft函数
3. 频带划分，在人可识别的音频范围，设置起始频率、截止频率、每次变化值。

## 动画绘制

通过 CAGradientLayer，设置colors和locations属性，创建动画。

## 调整优化

1. 动画与音乐节奏匹配度不高。A计权
2. 锯齿消除。使用加权平均
3. 闪动优化。加权平均
