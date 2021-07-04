# opencv-video-mac

## Motivation

OpenCV behavior of camera capture related function is often different for each version, especially for Macintosh.  
Need to clear which version is better to use.

## Tested platform information

- macOS: Big Sur 11.4
- Mac: MacBook Air (M1, 2020)
- Python: 3.8.10 from miniconda

## Test results

|opencv-python|Camera(VideoCapture)|Picture(imwrite)|Video(VideoWriter)|
|-------------|--------------------|----------------|------------------|
|4.5.2.54     |Works well[1]       |Freeze[2]       |Works well[3]     |
|4.4.0.46     |Works well[1]       |Works well      |Freeze[2]         |

### [1] If trapped or aborted
- Permit to access Camera for Terminal.app by: System Settings->Security and privacy->Privacy->Camera

### [2] If freezing
Need to kill freezing process.
```
^Z (means pushing control-key + Z-key)
pkill python
```

### [3] VideoWriter on 4.5.2.54
- Need to close with `release()`
  - If using `out=ImageWriter()`, need to close like as: `out.release()` before exit
- Need to match pixels between ImageWriter() size and captured image size
  - Camera image on Mac may be 16:9, then:
    - Initialize VideoWriter with 16:9 pixels, like: `out=cv2.VideoWriter("a.mp4",cv2.VideoWriter_fourcc(*'mp4v'),10,(640,360))`
    - Insert resize between read and write, like: `img = cv2.resize(img,(640,360))`
- Tested video format and codec
  - avi: VideoWriter_fourcc(*'XVID')
    - Can be write, but Macintosh pre-installed Quicktime player can't play this format, then you need additional player like [VLC media player](https://www.videolan.org/vlc/index.html)  
  - mp4: VideoWriter_fourcc(*'mp4v')
    - Can be write, and Quicktime player can play this format
  - mp4: VideoWriter_fourcc(*'avc1')
    - Can be write, and Quicktime player can play this format

## FYI: opencv-python version control
### How to check version
- `pip list | grep opencv-python`
### How to install
- Latest version install: `pip install -U opencv-python`
- Specific version install: `pip install opencv-python==4.4.0.46`

## Reference
- [OpenCV VideoWriter official reference](https://docs.opencv.org/4.5.2/dd/d9e/classcv_1_1VideoWriter.html)
- [OpenCV VideoWriter official tutorial](https://docs.opencv.org/4.5.2/dd/d43/tutorial_py_video_display.html)
- [PyPI opencv-python version history](https://pypi.org/project/opencv-python/#history)
- Resize image: https://stackoverflow.com/questions/43448456/how-to-store-webcam-video-with-opencv-in-python
- ImageCapture aborts: https://stackoverflow.com/questions/52634009/opencv-python-scripts-mac-aborts
- Codec variation on Mac: https://gist.github.com/takuma7/44f9ecb028ff00e2132e
