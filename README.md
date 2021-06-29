# opencv-video-mac

Tested version information on Mac

- macOS: Big Sur 11.4
- Mac: MacBook Air (M1, 2020)
- Python: 3.8.10 from miniconda

|opencv-python|Camera(VideoCapture)|Picture(imwrite)|Video(VideoWriter)|
|-------------|--------------------|----------------|------------------|
|4.5.2.54     |Works well[1]       |Freeze[2]       |Works well[3]     |
|4.4.0.46     |Works well[1]       |Works well      |Freeze[2]         |

## [1] If trapped or aborted
- Permit to access Camera for Terminal.app by: System Settings->Security and privacy->Privacy->Camera

## [2] If freezing
```
^Z (means pushing control-key + Z-key)
pkill python
```

## [3] VideoWriter on 4.5.2.54
- Need to close with `release()`
  - If using `out=ImageWriter()`, need to close like as: `out.release()` before exit
- Need to match pixels between ImageWriter() size and VideoCapture size
  - Camera image on Mac may be 16:9, then:
    - Initialize VideoWriter with 16:9 pixels like: `out=cv2.VideoWriter("a.mp4",cv2.VideoWriter_fourcc(*'mp4v'),10,(640,360))`
    - Insert resize in read-write loop like: `img = cv2.resize(img,(640,360))`
- Tested video format and codec
  - avi: VideoWriter_fourcc(*'XVID')
    - Pre-installed Quicktime player can't play this format, then you need additional player like [VLC player](https://www.videolan.org/vlc/index.html)  
  - mp4: VideoWriter_fourcc(*'mp4v')
  - mp4: VideoWriter_fourcc(*'avc1')

## OpenCV version change
- Version up to latest: `pip install -U opencv-python`
- Version specific install: `pip install opencv-python==4.4.0.46`

## Reference
- [OpenCV VideoWriter official reference](https://docs.opencv.org/4.5.2/dd/d9e/classcv_1_1VideoWriter.html)
- [OpenCV VideoWriter official tutorial](https://docs.opencv.org/4.5.2/dd/d43/tutorial_py_video_display.html)
- [PyPI opencv-python version history](https://pypi.org/project/opencv-python/#history)
- Resize image: https://stackoverflow.com/questions/43448456/how-to-store-webcam-video-with-opencv-in-python
- ImageCapture aborts: https://stackoverflow.com/questions/52634009/opencv-python-scripts-mac-aborts
- Codec variation on Mac: https://gist.github.com/takuma7/44f9ecb028ff00e2132e
