
# ![SafeVision Logo](https://i.ibb.co/d4LqhX4/Safe-Vision-2.png)
![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20macOS-blue)
![Python](https://img.shields.io/badge/python-3.8%2B-yellow)
![License](https://img.shields.io/github/license/im-syn/safevision)
![Status](https://img.shields.io/badge/stability-stable-brightgreen)
  
---
##  Overview

SafeVision is a sophisticated Python script designed to detect and blur nudity in both images and videos. By harnessing advanced computer vision and deep learning techniques, SafeVision ensures that potentially sensitive or inappropriate content is effectively obscured, promoting safer and more appropriate media sharing and consumption. This README file provides comprehensive instructions on setting up and utilizing SafeVision effectively.
---

##  Table of Contents
 
* [ Features](#-features)
* [ Installation](#-installation)
* [ Image Processing](#️-image-processing)
* [ Video Processing](#-video-processing)
* [ Blur Exception Rules](#-blur-exception-rules)
* [ GUI Application](#️-gui-application)
* [ How It Works](#️-how-it-works)
* [ Output Directory Structure](#-output-directory-structure)
* [ NSFW Demo (Spoiler Warning)](#-nsfw-demo-spoiler-warning)
* [ Conclusion](#-conclusion)

---

##  Features

*  Nudity Detection (Image & Video)
*  Selective or Full Blurring
*  Custom Blur Exception Rules
*  Detailed Logs and Reports
*  Advanced CLI and GUI Support
*  Color Masking & Enhanced Blur
*  Auto Folder Structure & Cleanup Options
*  Audio-Preserving Video Output

---

##  Installation

```bash
git clone https://github.com/im-syn/safevision.git
cd safevision
pip install -r requirements.txt
```

>  Place your `best.onnx` model in the `Models/` directory.

---

## Image Processing

### 🔸 Basic Example

```bash
python main.py -i path/to/image.jpg
```

### 🔸 Blur Detected Areas

```bash
python main.py -i path/to/image.jpg -b
```

### 🔸 Full Options

| Flag                       | Description                          |
| -------------------------- | ------------------------------------ |
| `-i`, `--input`            | Input image path (**required**)      |
| `-o`, `--output`           | Output path (optional)               |
| `-b`, `--blur`             | Blur detected regions                |
| `-e`, `--exception`        | Path to BlurException.rule file      |
| `-fbr`, `--full_blur_rule` | Number of boxes to trigger full blur |

---

## 🎥 Video Processing

### 🔸 Basic Detection

```bash
python video.py -i path/to/video.mp4 -t video
```

### 🔸 Blur Video with Audio

```bash
python video.py -i path/to/video.mp4 -b --blur -a
```

### 🔸 Full CLI Options

| Flag                       | Description                              |
| -------------------------- | ---------------------------------------- |
| `-i`, `--input`            | Input video path (**required**)          |
| `-o`, `--output`           | Output video path                        |
| `-t`, `--task`             | Task type: `video` or `frames`           |
| `-vo`, `--video_output`    | Output folder                            |
| `-r`, `--rule`             | Rule in format `percentage/count`        |
| `-b`, `--boxes`            | Draw boxes                               |
| `--blur`                   | Blur detected areas (requires `--boxes`) |
| `-a`, `--with-audio`       | Include original audio                   |
| `-c`, `--codec`            | Codec: `mp4v`, `xvid`, etc.              |
| `--ffmpeg-path`            | Custom path to ffmpeg                    |
| `-df`, `--delete-frames`   | Auto-delete temp frames                  |
| `--enhanced-blur`          | Stronger censorship blur                 |
| `--color`                  | Use solid color masking                  |
| `--mask-color`             | Set color: e.g., `0,0,255` for red       |
| `-fbr`, `--full-blur-rule` | Trigger full blur: `labels/frames`       |

---

##  Blur Exception Rules

Create a file named `BlurException.rule` and define what labels to blur:

```ini
FACE_MALE = false
FEMALE_BREAST_EXPOSED = true
ANUS_EXPOSED = true
...
```

* `true` → Blur this label.
* `false` → Skip blurring for this label.

---

##  GUI Application

A modern desktop GUI is available in `SafeVisionGUI.py`.

###  Features

* Drag & drop images/videos
* Blurring / Masking / Bounding Box mode
* FFmpeg-based audio merging
* Codec & frame settings
* Real-time log panel and live preview
* Theme toggle (dark/light)

###  Launch
```bash
python SafeVisionGUI.py
```
---
# Live Nudity Detection with Blur Exception Rules & Gender/Age Detection

## Usage Examples

### 1. Basic Usage (Auto-loads blur rules)
```bash
python live.py
```
- Automatically checks for `BlurException.rule` in the same directory
- If found and not empty, uses existing rules
- If not found or empty, creates default rules with all labels set to `true`

### 2. Custom Rules File
```bash
python live.py -r custom_rules.rule
```
or
```bash
python live.py --rules custom_rules.rule
```

### 3. Enable Gender and Age Detection
```bash
python live.py -g
```
or
```bash
python live.py --gender-detection
```
- Requires `best_gender.onnx` model in the `Models/` folder
- Displays gender (Male/Female) and estimated age for detected faces
- Shows confidence scores for predictions

### 4. Complete Example with All Features
```bash
python live.py -c 0 -r my_rules.rule -g --auto-record --alert-threshold 2
```

## Blur Exception Rules

### Auto-Loading Behavior:
- **On startup**: Checks for `BlurException.rule` in same directory
- **If exists and not empty**: Loads existing rules
- **If missing or empty**: Creates default rules automatically
- **Optional -r parameter**: Override auto-loading with specific file

### Rules File Format:
The rules file uses the format: `LABEL = true/false`
- `true`: Apply blur/censoring to this detection type
- `false`: Skip blur/censoring for this detection type

### Available Labels:
- FEMALE_GENITALIA_EXPOSED
- MALE_GENITALIA_EXPOSED  
- FEMALE_BREAST_EXPOSED
- MALE_BREAST_EXPOSED
- BUTTOCKS_EXPOSED
- ANUS_EXPOSED
- BELLY_EXPOSED
- FEET_EXPOSED
- ARMPITS_EXPOSED
- FACE_FEMALE
- FACE_MALE
- FEMALE_GENITALIA_COVERED
- FEMALE_BREAST_COVERED
- BUTTOCKS_COVERED
- ANUS_COVERED
- BELLY_COVERED
- FEET_COVERED
- ARMPITS_COVERED

## Gender/Age Detection

### Requirements:
- `best_gender.onnx` model in `Models/` directory
- Model format should match the example in `myai/run.py`
- Input: 224x224 RGB images
- Output: Gender classification + Age regression

### Features:
- **Real-time Analysis**: Analyzes detected faces in real-time
- **Gender Classification**: Male/Female with confidence scores
- **Age Estimation**: Estimated age in years
- **Visual Feedback**: Shows results on status overlay
- **Runtime Toggle**: Press 'G' key to toggle on/off during runtime

### Status Display:
When enabled, shows:
- `Gender Detection: ON/FAILED` in status overlay
- For each detected face: `Female, 25y (0.95)` format
- Gender, estimated age, and confidence score

## Command Line Arguments

- `-c, --camera`: Camera ID (default: 0)
- `-r, --rules`: Path to blur exception rules file (optional - auto-loads if exists)
- `-g, --gender-detection`: Enable gender and age detection
- `--no-boxes`: Disable detection boxes
- `--privacy`: Start in privacy mode
- `--enhanced-blur`: Use enhanced blur mode
- `--solid-color`: Use solid color masking
- `--mask-color`: Color for solid masking (BGR format)
- `--auto-record`: Auto-record when nudity detected
- `--alert-threshold`: Consecutive detections for alert
- `--skip-frames`: Process every nth frame for performance

## Runtime Controls

- **SPACE**: Toggle recording
- **B**: Toggle detection boxes
- **P**: Toggle privacy mode  
- **G**: Toggle gender detection (if enabled)
- **Q**: Quit application

## Example Rule Configurations

### Strict Mode (Only explicit content)
```
FEMALE_GENITALIA_EXPOSED = true
MALE_GENITALIA_EXPOSED = true
FEMALE_BREAST_EXPOSED = true
BUTTOCKS_EXPOSED = true
ANUS_EXPOSED = true
MALE_BREAST_EXPOSED = false
BELLY_EXPOSED = false
FEET_EXPOSED = false
ARMPITS_EXPOSED = false
FACE_FEMALE = false
FACE_MALE = false
# All COVERED labels = false
```

### Conservative Mode (Maximum privacy)
```
# All EXPOSED labels = true
# Covered areas also censored for privacy
FEMALE_GENITALIA_COVERED = true
FEMALE_BREAST_COVERED = true
BUTTOCKS_COVERED = true
FACE_FEMALE = false
FACE_MALE = false
```

### Face Analysis Mode (Skip censoring, focus on gender/age)
```
# All labels = false (no censoring)
# Use with -g flag for gender/age analysis only
```

---

##  How It Works

###  Pipeline

1. **Preprocessing** – Resize and normalize input image or video frames.
2. **Inference** – Use `ONNXRuntime` to run the `best.onnx` model.
3. **Postprocessing** – Detect bounding boxes and labels.
4. **Censorship** – Apply blur/mask/box per user rules.
5. **Rendering** – Save censored images/videos to output folders.

---

## 📂 Output Directory Structure

| Folder          | Description                      |
| --------------- | -------------------------------- |
| `output/`       | Final censored images/videos     |
| `blur/`         | Full blurred content             |
| `prosses/`      | Detection-only visuals (no blur) |
| `video_output/` | Rendered final videos            |

---

## 📷 NSFW Demo (Spoiler Warning)

<details>
  <summary>⚠️ Click to Show Example Output Image using SafeVisionGUI (Contains NSFW Examples with Blurring)</summary>
  <p>

![Blurred Output](https://github.com/user-attachments/assets/a62d64d1-199c-4d28-a34f-46c53ba056e6)

*Example showing SafeVision blurring applied on exposed content. using the SafeVisionGUI*

  </p>
</details>
<details>
  <summary>⚠️ Click to Show Example Output using CLI (main.py) (Contains NSFW Examples with Blurring)</summary>
  <p>

![Blurred Output](https://github.com/user-attachments/assets/5a9b362b-e103-427c-b10d-8f6157578f10)

*Example showing SafeVision blurring applied on exposed content.*

  </p>
</details>

---
### Conclusion
SafeVision provides a robust solution for detecting and blurring nudity in images and videos, making it a valuable tool for content moderation and safe media sharing. Follow the instructions in this README to set up and use SafeVision effectively. 

---

## 🛠 Maintainer & Support

Maintained by [@im-syn](https://github.com/im-syn)
Pull requests, issues, and contributions are welcome!

---

> **Note:** This project is intended for ethical and responsible use only. Always follow legal and platform-specific content handling policies.


---
## ☕ Like It?

If this helped you, consider giving the repo a 🌟 or forking it to your toolkit.
Thank you for using **SafeContentText**! Feel free to open issues or PRs for improvements.
