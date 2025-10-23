# AI Assignment: Footfall Counter using Computer Vision

This project is a computer vision-based system that counts the number of people entering and exiting through a specific area in a video.

The system uses the YOLOv8 object detection model with built-in tracking to detect and follow individuals. It establishes a virtual line and counts people as they cross it, distinguishing between "IN" (left-to-right) and "OUT" (right-to-left) movements.

##  Features

* **Person Detection**: Uses the `YOLOv8n` model to detect people in each frame.
* **Object Tracking**: Employs YOLO's built-in tracker (ByteTrack) to assign and maintain a unique ID for each person, even through some occlusions.
* **Counting Logic**: Defines a dynamic, vertical line in the center of the frame.It counts a person *once* when their centroid crosses this line.
* **Visualization**: Draws bounding boxes, track IDs, and total "IN" / "OUT" counts on the video in real-time.
* **Video Output**: Saves the processed video with all visualizations to an `output.mp4` file.
* **Bonus: Real-time Webcam (Bonus)**: Can be configured to run on a live webcam feed.
* **Bonus: Trajectory Paths (Bonus)**: Draws a "tail" behind each person to visualize their path.

##  Video Source

The primary video source used for development is the `people-detection.mp4` video from the [Intel IoT DevKit Sample Videos](https://github.com/intel-iot-devkit/sample-videos) repository[cite: 36].

The script can also be set to use a live webcam feed by changing the `VIDEO_SOURCE` constant.

##  Counting Logic Explained

The counting logic is based on tracking the centroid (center point) of each person:

1.  **Define Line**: A vertical line is dynamically placed in the exact center of the video frame.
2.  **Track History**: The script stores a history of the `(x, y)` centroid for each tracked `ID`.
3.  **Check Crossing**: In each frame, the script checks if a person's *current* `x` coordinate has crossed the line relative to their *previous* `x` coordinate.
    * **IN**: If `previous_x < line_x` and `current_x >= line_x`, the `IN` counter increases.
    * **OUT**: If `previous_x > line_x` and `current_x <= line_x`, the `OUT` counter increases.
4.  **Prevent Double Counts**: A `set` (named `counted_ids`) stores the ID of every person who has already crossed the line, ensuring they are only counted once.

##  Dependencies and Setup

]This project requires **Python 3.8 or newer*.

### 1. Create a Virtual Environment

It is highly recommended to use a virtual environment.

```bash
# Create the environment
python -m venv venv

# Activate on Windows
.\venv\Scripts\activate

# Activate on macOS/Linux
source venv/bin/activate
# Footfall-Counter
