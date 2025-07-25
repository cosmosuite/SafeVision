
# SafeVision
<img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/44434b2b-e7c9-4458-b878-83053860e8d2" />

 ![GitHub Temp](https://i.ibb.co/d4LqhX4/Safe-Vision-2.png)

SafeVision is a sophisticated Python script designed to detect and blur nudity in both images and videos. By harnessing advanced computer vision and deep learning techniques, SafeVision ensures that potentially sensitive or inappropriate content is effectively obscured, promoting safer and more appropriate media sharing and consumption. This README file provides comprehensive instructions on setting up and utilizing SafeVision effectively.
 ## Features
- **Nudity Detection:** Detects various types of nudity in images and videos.
- **Blur Functionality:** Blurs detected nudity to ensure safe content sharing.
- **Exception Rules:** Configurable rules to control which parts should be blurred.
- **Logging:** Generates logs with detailed detection information.

## Installation

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/im-syn/safevision.git
   cd safevision
   ```

2. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Download the Model:**
   - Ensure the `best.onnx` model is placed in the `Models` directory.

## Usage
 ![Demo Image](https://i.postimg.cc/J07dYYLH/1-Output.jpg)
### Command-line Arguments
- `-i` or `--input`: Path to the input image or video (required).
- `-o` or `--output`: Path to save the censored image or video. If not provided, a default path will be used.
- `-b` or `--blur`: Apply blur to NSFW regions instead of drawing boxes.
- `-e` or `--exception`: Path to the blur exception rules file.
- `-fbr` or `--full_blur_rule`: Number of exposed boxes to trigger full image blur.

### Example Commands
1. **Detect nudity and draw boxes:**
   ```bash
   python main.py -i path/to/media.jpg
   ```

2. **Detect nudity and blur detected regions:**
   ```bash
   python main.py -i path/to/media.jpg -b
   ```

3. **Specify output path:**
   ```bash
   python main.py -i path/to/media.jpg -o path/to/output.jpg
   ```

4. **Use exception rules file:**
   ```bash
   python main.py -i path/to/media.jpg -e path/to/BlurException.rule
   ```

5. **Set full blur rule and use exception rules file:**
   ```bash
   python main.py -i path/to/media.jpg -e path/to/BlurException.rule -fbr 25
   ```

### Blur Exception Rules File (`BlurException.rule`)
```plaintext
FACE_MALE = false
FACE_FEMALE = false
ARMPITS_COVERED = false
FEET_COVERED = false
BUTTOCKS_COVERED = false
BELLY_COVERED = false
FEMALE_GENITALIA_COVERED = false
FEMALE_BREAST_COVERED = false
ANUS_COVERED = false
MALE_GENITALIA_EXPOSED = true
MALE_BREAST_EXPOSED = true
ANUS_EXPOSED = true
FEET_EXPOSED = false
ARMPITS_EXPOSED = true
FEMALE_GENITALIA_EXPOSED = true
FEMALE_BREAST_EXPOSED = true
BUTTOCKS_EXPOSED = true
BELLY_EXPOSED = false
```

In the `BlurException.rule` file, `false` indicates that the corresponding body part will not be blurred, while `true` indicates that it will be blurred.

### Video Processing

#### Additional Command-line Arguments for Video
- `-vo` or `--video_output`: Path to the video output folder. Default is 'video_output'.
- `-r` or `--rule`: Blur rule in the format 'percentage/count'.

#### Example Commands for Video Processing
1. **Process video and draw boxes:**
   ```bash
   python video.py -i path/to/video.mp4 -t video
   ```

2. **Process video and blur detected regions:**
   ```bash
   python video.py -i path/to/video.mp4 -t video -b
   ```

3. **Specify video output folder:**
   ```bash
   python video.py -i path/to/video.mp4 -t video -vo path/to/video_output
   ```

4. **Use blur rule:**
   ```bash
   python video.py -i path/to/video.mp4 -t video -r 50/10
   ```



## 🎥 Video Processing (Advanced)

The `video.py` script in SafeVision provides powerful and configurable video censorship capabilities, supporting multiple blurring strategies, audio handling, codec options, and more.

### ✅ Command-Line Arguments

* `-i`, `--input` – Path to the input video (**required**)
* `-o`, `--output` – Output video path (optional)
* `-t`, `--task` – Processing type: `frames` or `video` (default: `video`)
* `-vo`, `--video_output` – Folder to store the final rendered video (default: `video_output`)
* `-r`, `--rule` – Rule in the format `percentage/count` to trigger full blur
* `-b`, `--boxes` – Generate video with detection boxes only
* `--blur` – Blur detected regions instead of just drawing boxes (requires `--boxes`)
* `-a`, `--with-audio` – Include original audio in the output video
* `-c`, `--codec` – Output codec: `mp4v`, `avc1`, `xvid`, or `mjpg` (default: `mp4v`)
* `--ffmpeg-path` – Path to `ffmpeg` binary if needed (for merging audio)
* `-df`, `--delete-frames` – Delete frames after rendering the video
* `--enhanced-blur` – Apply stronger blur for total censorship
* `-fbr`, `--full-blur-rule` – Format: `labels/frames` to trigger full video blur
* `--color` – Use solid color mask instead of blur
* `--mask-color` – BGR color used for masking (e.g., `0,0,255` for red)

###  Example Commands
```bash
python video.py -i path/to/video.mp4 -b
python video.py -i path/to/video.mp4 -b --blur
python video.py -i path/to/video.mp4 -b --blur -a -vo ./output_folder
python video.py -i path/to/video.mp4 -b --blur -r 60/20
python video.py -i path/to/video.mp4 -b --blur -fbr 8/3
python video.py -i path/to/video.mp4 -b --color --mask-color 0,0,255
python video.py -i path/to/video.mp4 -b --blur -df
```

---

##  SafeVision GUI App

The SafeVisionGUI.py provides a modern desktop interface for interacting with the CLI tools.

###  Features
- Drag & Drop media loading (MP4, AVI, JPG, PNG)
- Process modes: Blur, Mask, Boxes
- FFmpeg integration and audio handling
- Enhanced blur and solid color masking
- Codec selection
- Log panel and preview tab
- Dark and Light themes
- Real-time feedback

###  How to Launch

```bash
python SafeVisionGUI.py
```

###  Interface

- Input Section for file selection
- Preview tab for before/after comparison
- Log panel to view execution details
- Settings tab for output folders and codecs



### How Video Processing Works
The video processing follows a similar structure to image processing, with additional steps for handling frames and creating videos.

## How It Works

### Overview
The script processes an input image or video to detect and blur nudity based on a trained ONNX model. It includes the following main components:

1. **Image/Video Preprocessing:**
   - Reads and preprocesses the image or video frames, resizing and normalizing them for the model.

2. **Model Inference:**
   - The `NudeDetector` class uses ONNX Runtime to load the model and perform inference to detect nudity.

3. **Postprocessing:**
   - Processes the model output to extract detected bounding boxes and labels.

4. **Blurring and Drawing Boxes:**
   - Applies blurring to detected nudity or draws bounding boxes based on user configuration.

### Detailed Code Explanation

#### Image/Video Preprocessing
- Reads the image or video frames using OpenCV.
- Resizes and pads the image or video frames to maintain aspect ratio.
- Normalizes the image or video frame data for model input.

#### Model Inference
- Initializes the ONNX model for inference.
- Loads exception rules for blurring.

#### Postprocessing
- Transposes and processes model output to extract bounding boxes and class labels.

#### Blurring and Drawing Boxes
- Detects nudity in the image or video frames.
- Applies blurring or draws bounding boxes based on user options.
- Saves the processed image or video frames to the specified output path.

### Creating Directories
The script ensures necessary directories exist for saving output images and videos.

## Output
- **Processed Images/Videos:**
  - **Blur Folder:** Contains all the output images with nudity blurred based on the applied rules.
  - **Output Folder:** Stores debug images with detected boxes and blurring based on the rules. 
  - **Prosses Folder:** Contains images with only the detected boxes, without any blurring. 

### Conclusion
SafeVision provides a robust solution for detecting and blurring nudity in images and videos, making it a valuable tool for content moderation and safe media sharing. Follow the instructions in this README to set up and use SafeVision effectively. If you have any questions or issues, please refer to the documentation or contact me.
