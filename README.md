# SyncNet

This repository contains the demo for the audio-to-video synchronisation network (SyncNet). This network can be used for audio-visual synchronisation tasks including: 
1. Removing temporal lags between the audio and visual streams in a video;
2. Determining who is speaking amongst multiple faces in a video. 

Please cite the paper below if you make use of the software. 

## Dependencies
You must install CUDA. On Windows, you can install it with `scoop`. On Linux, investigate what
version is available for your distribution.

You must install the CUDA version of PyTorch. Unfortunately, that requires a specific index URL, so cannot be
installed from requirements.txt. You can get the appropriate command for your system from the 
[PyTorch website](https://pytorch.org/get-started/locally/).
```
# Modify the line below to match the instructions you get from the PyTorch website
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
pip install -r requirements.txt
```

In addition, `ffmpeg` is required. You can install it on Windows with `scoop`. On Linux, use your package manager.

On Windows, you will also need `bash` (the latest version in 2025 is the
`scoop msys` package, but that is over three years old) and `wget`.

## Demo

Before you run the demo you must run `download_model.sh`. It also downloads the
`example.avi` file used in the demo.

SyncNet demo:
```sh
python demo_syncnet.py --videofile data/example.avi --tmp_dir /path/to/temp/directory
```

Check that this script returns something like:
```none
AV offset:      3 
Min dist:       5.353
Confidence:     10.021
```

(On my system, it's slightly different:
```none
AV offset:      3
Min dist:       5.348
Confidence:     10.081
```
)

Full pipeline:
```sh
sh download_model.sh
python run_pipeline.py --videofile /path/to/video.mp4 --reference name_of_video --data_dir /path/to/output
python run_syncnet.py --videofile /path/to/video.mp4 --reference name_of_video --data_dir /path/to/output
python run_visualise.py --videofile /path/to/video.mp4 --reference name_of_video --data_dir /path/to/output
```

Outputs:
```none
$DATA_DIR/pycrop/$REFERENCE/*.avi - cropped face tracks
$DATA_DIR/pywork/$REFERENCE/offsets.txt - audio-video offset values
$DATA_DIR/pyavi/$REFERENCE/video_out.avi - output video (as shown below)
```
<p align="center">
  <img src="img/ex1.jpg" width="45%"/>
  <img src="img/ex2.jpg" width="45%"/>
</p>

## Publications
 
```
@InProceedings{Chung16a,
  author       = "Chung, J.~S. and Zisserman, A.",
  title        = "Out of time: automated lip sync in the wild",
  booktitle    = "Workshop on Multi-view Lip-reading, ACCV",
  year         = "2016",
}
```
