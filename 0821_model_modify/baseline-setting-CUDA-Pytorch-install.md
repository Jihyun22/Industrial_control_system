# CUDA, Pytorch 설치

## CUDA 설치

- 설치 [링크](https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=10&target_type=exelocal)
- 설치 사양(window 기준)

![설치사양](https://user-images.githubusercontent.com/43198705/90589799-f030a100-e219-11ea-92be-f7725db6a12a.png)



- 설치 확인

  ``` 
  nvcc --version
  ```

  ![image-20200819131524033](C:\Users\jih02\AppData\Roaming\Typora\typora-user-images\image-20200819131524033.png)



## pytorch 설치

```
conda install pytorch torchvision cudatoolkit=9.0 pytorch
```





