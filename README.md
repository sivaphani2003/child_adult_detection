# People Detection, Tracking, and Classification (YOLOv8 + DeepSORT)

This project uses YOLOv8 for real-time person detection and DeepSORT for tracking individuals in video streams. It classifies detected persons as either **children** or **adults** based on bounding box height.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technologies Used](#technologies-used)
- [Installation](#installation)
- [Usage](#usage)
- [Processing Multiple Videos](#processing-multiple-videos)
- [Customization](#customization)
- [Contributing](#contributing)
- [License](#license)

## Overview

This project detects and tracks people in video streams or pre-recorded video files. The YOLOv8 model is used for detecting persons, while DeepSORT tracks the detected individuals across frames. Each individual is classified as a **child** or **adult** based on the size of their bounding box (which approximates the height of the person).

The project is suitable for real-time video processing or analyzing pre-recorded videos, such as in surveillance footage or activity monitoring systems.

## Features

- **Person Detection**: Detects people in videos using YOLOv8.
- **Object Tracking**: Tracks detected persons frame-to-frame using DeepSORT.
- **Classification**: Classifies individuals as children or adults based on bounding box height.
- **Video Output**: Processes and saves an output video with bounding boxes and labels for tracked individuals.
  
## Technologies Used

- **Python**: Scripting language for the project.
- **YOLOv8**: For object detection (person detection).
- **DeepSORT**: For tracking objects across video frames.
- **OpenCV**: For video processing and handling video input/output.
- **Google Colab**: The project runs in a Colab environment, using Google Drive for input/output video storage.

## Installation

1. **Clone the Repository** (or download the script directly if no repo exists):

    ```bash
    git clone https://github.com/sivaphani2003/yolo-deepsort-classification.git
    cd yolo-deepsort-classification
    ```

2. **Install the required dependencies**:
    - You can run this script in Google Colab, but if running locally, install the required libraries:

    ```bash
    pip install ultralytics opencv-python opencv-python-headless scikit-image filterpy lap
    pip install deep_sort_realtime
    ```

3. **Mount Google Drive** (if using Colab):

    ```python
    from google.colab import drive
    drive.mount('/content/drive')
    ```

4. **Load videos**: Place videos you want to process in your Google Drive. The script accesses videos stored in `'/content/drive/MyDrive/videos/'`.

## Usage

1. **Run Detection and Tracking on a Single Video**:
    Modify the script to provide the path of your video file and output file location.

    ```python
    video_path = '/content/drive/MyDrive/videos/input2.mp4'
    output_path = '/content/output/output2.mp4'
    detect_and_track(video_path, output_path)
    ```

2. **Function Overview**:
    - `detect_and_track(video_path, output_path)`: Detects people in the given video, tracks them, and saves the output video with bounding boxes and classification labels (child or adult).

3. **Processing Multiple Videos**:
    You can process all the videos in a folder by uncommenting the loop:

    ```python
    for filename in os.listdir(video_folder):
        video_path = os.path.join(video_folder, filename)
        output_path = f'/content/output/output_{filename}.mp4'
        detect_and_track(video_path, output_path)
    ```

## Processing Multiple Videos

To process a directory of videos, make sure the video files are placed in the specified folder (`'/content/drive/MyDrive/videos/'` by default). The output videos will be saved in the `/content/output/` folder, with a unique filename based on the input video.

### Example:

```python
video_folder = '/content/drive/MyDrive/videos'
output_folder = '/content/output/'

for filename in os.listdir(video_folder):
    video_path = os.path.join(video_folder, filename)
    output_path = f'{output_folder}/output_{filename}'
    detect_and_track(video_path, output_path)
```

## Customization

1. **Threshold for Classification**:
    - Currently, individuals are classified as children if their bounding box height is below `240 pixels`. Adjust the threshold as per your video frame resolution or real-world scaling.

    ```python
    label = child_label if bbox[3] - bbox[1] < 240 else adult_label
    ```

2. **Confidence Threshold**:
    - The confidence threshold for detecting persons is set to `0.7`. You can modify this threshold to increase or decrease the detection sensitivity.

    ```python
    if label_idx == 0 and conf > 0.7:  # 0 corresponds to "person"
    ```

3. **Model Choice**:
    - The script uses `yolov8l.pt`, a medium-sized model. You can replace this with a larger or smaller version (e.g., `yolov8x.pt` for larger, more accurate models) by updating the model loading line.

    ```python
    model = YOLO('yolov8x.pt')
    ```

## Contributing

Contributions are welcome! If you'd like to improve the detection, tracking, or classification logic, feel free to open an issue or submit a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
