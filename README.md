Great choice! Using Python gives you more flexibility, especially if you plan to add features later (like uploading, filtering, or image analysis).


---

✅ Plan: Use Python to Capture Images from Your Celestron Microscope

We’ll use:

OpenCV (to access the USB camera)

schedule or time (to take snapshots every 10 minutes)



---

🧰 Step-by-Step Instructions

📦 1. Install Required Packages

First, install OpenCV:

sudo apt update
sudo apt install python3-opencv

Also, if you'd like to use scheduling in a cleaner way:

pip3 install schedule


---

🐍 2. Python Script to Capture Images

Create a file called capture_microscope.py:

nano capture_microscope.py

Paste this code:

import cv2
import os
from datetime import datetime
import time
import schedule

# Directory to save images
output_dir = "/home/pi/microscope_images"
os.makedirs(output_dir, exist_ok=True)

# Set your USB camera ID (usually 0, 1 if multiple cameras)
CAMERA_INDEX = 0

def capture_image():
    now = datetime.now().strftime("%Y-%m-%d_%H-%M-%S")
    filename = os.path.join(output_dir, f"{now}.jpg")

    cap = cv2.VideoCapture(CAMERA_INDEX)

    # Set resolution (5MP = 2592x1944 if supported)
    cap.set(cv2.CAP_PROP_FRAME_WIDTH, 2592)
    cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 1944)

    if not cap.isOpened():
        print("❌ Could not open camera.")
        return

    ret, frame = cap.read()
    if ret:
        cv2.imwrite(filename, frame)
        print(f"✅ Image saved: {filename}")
    else:
        print("⚠️ Failed to capture image.")

    cap.release()

# Schedule to run every 10 minutes
schedule.every(10).minutes.do(capture_image)

print("📷 Microscope image capture started (every 10 minutes)...")
capture_image()  # capture once immediately

while True:
    schedule.run_pending()
    time.sleep(1)


---

🏃 3. Run It

Make sure it's executable and run it:

python3 capture_microscope.py

This will:

Capture an image immediately

Keep running in the background capturing every 10 minutes



---

⚙️ 4. (Optional) Run It on Boot

You can make it run on startup using cron:

crontab -e

Add this line at the bottom:

@reboot python3 /home/pi/capture_microscope.py &

Make sure the script has correct permissions and absolute paths.


---

🔎 Verify Camera Index (if needed)

If the camera index 0 doesn't work, try listing video devices:

ls /dev/video*

Try video0, video1, etc. in this line of the script:

cap = cv2.VideoCapture(CAMERA_INDEX)


---

🧪 Test Before Scheduling

Run it manually to confirm:

python3 capture_microscope.py

Check the images in /home/pi/microscope_images/.


---

Let me know if you want to:

Add real-time preview

Upload images to cloud

Detect blur/focus automatically

Create a web interface


I can help you extend the script easily.

