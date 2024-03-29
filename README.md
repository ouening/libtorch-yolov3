# libtorch-yolov3
A Libtorch implementation of the YOLO v3 object detection algorithm, written with pure C++, modified from [https://github.com/walktree/libtorch-yolov3](https://github.com/walktree/libtorch-yolov3). 

It can be seen the changes in issue: [https://github.com/walktree/libtorch-yolov3/issues/52#issue-686915754](https://github.com/walktree/libtorch-yolov3/issues/52#issue-686915754) 

I have successfully tested in Win10 using Visual Studio 2017 and Visual Studio 2019.

## Requirements
1. [>=LibTorch v1.6.0 (Tested on 1.6,1.8 and 1.13)](https://pytorch.org/cppdocs/installing.html)

    Download from `https://download.pytorch.org/libtorch/nightly/cpu/libtorch-win-shared-with-deps-latest.zip` （==Make sure that you have the download the windows version)==

    Add `"${LIBTORCH}/lib"` to system path.
2. CUDA(Optional)
3. [OpenCV4.6](https://github.com/opencv/opencv/releases/tag/4.6.0) (Sugest using windows pre-build package)
    
    Add `opencv/build/x64/vc15/bin` to Windows system `PATH` **(Support Visual Studio 2019)**
4. `Git Bash` or `Cmder`

## To compile
1. Cmake 3.15
2. Visual Studio 2017 (VC 15) or Visual Studio 2019 (VC 16)


```
$ mkdir build && cd build
$ cmake -G "Visual Studio 15 2017 Win64" -T host=x64 -DCMAKE_PREFIX_PATH="your libtorch root" -DOpenCV_DIR="your opencv root" ..
```
For VS 2019
```
# $ cmake -G "Visual Studio 16 2019" -T host=x64 .. -DCMAKE_PREFIX_PATH="your libtorch root" -DOpenCV_DIR="your opencv root" ..
```
For example:
```
$ cmake -G "Visual Studio 16 2019" -T host=x64 .. -DCMAKE_PREFIX_PATH="E:\DeepLearning\libtorch" -DOpenCV_DIR="E:\ScienceComputing\opencv\build" ..
```

3. Finally compile, be aware that we should use *`Release`* because we have **used the released version of opencv**
```
$ cmake --build . --config Release -j 3
```
`your libtorch root` is like `E:\python\pytorch\libtorch` and `your opencv root` is like `"E:\ScientificComputing\opencv\build"`(which has `OpenCVConfig.cmake` file). Finally `libtorch1.6-yolov3\build\Release\yolo-app.exe` is generated.

## Running the detector

The first thing you need to do is to get the weights file for `yolov3`:

```
cd models
wget https://pjreddie.com/media/files/yolov3.weights 
```
By default, the program will load yolov3 cfg and weights in `model` directory. It can be changed manually in `main.cpp` in line 32 and line 39 (Don't forget to recompile!).

Copy all `.dll` file from `libtorch/lib` to `libtorch1.6-yolov3/build`, then open `git bash`  or `cmder` and execute:
```
$ cd libtorch1.6-yolov3/build
$ ./Release/yolo-app.exe ../imgs/person.jpg
```
The output result should be like this:
```
loading weight ...
weight loaded ...
start to inference ...
inference taken : 980 ms
3 objects found
Done
```
If you encounted some errors like:
-  `error while loading shared libraries: torch_cpu.dll: cannot open shared object file: No such file or directory`, be sure that you have put the `libtorch/lib` in your system path, or you can copy the dependent file to the same directory as `yolo-app.exe` is in.
-   `error while loading shared libraries: opencv_world460.dll: cannot open shared object file: No such file or directory`, be sure that you have put the `opencv/build/x64/vc15/bin` in your system path, or you can copy`opencv_world460.dll` to the same directory as `yolo-app.exe` is in.

The output image can be found in `libtorch1.6-yolov3/build/out-det.jpg`
![detection-output1](imgs/out-det.jpg)
![detection-output2](imgs/out-det-2.jpg)

## Change logs
- 2022.11.15 Rename `libtorch1.6-yolov3` to `libtorch-yolov3`