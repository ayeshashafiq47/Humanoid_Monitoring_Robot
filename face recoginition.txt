import pandas as pd
import cv2
import urllib.request
import numpy as np
import os
from datetime import datetime
import face_recognition

# Path to the directory containing the reference images
path = r'D:\Python 3.9\attendace\attendace\image_folder'

# IP address of the ESP32-CAM module
esp32cam_ip = '192.168.0.113'

# Stream port number of the ESP32-CAM module
esp32cam_stream_port = 81

# URL to access the MJPEG stream of the ESP32-CAM module
url = f'http://{esp32cam_ip}:{esp32cam_stream_port}/240x176.mjpeg'

# CSV file to store attendance data
attendance_file = "Attendance.csv"

# Remove the attendance file if it already exists
if attendance_file in os.listdir(os.path.join(os.getcwd(),'image_folder')):
    print("Attendance file found, deleting...")
    os.remove(attendance_file)

# Create an empty attendance DataFrame
attendance_df = pd.DataFrame(columns=['Name', 'Time'])

# Load the reference images and encode them
images = []
classNames = []
myList = os.listdir(path)
print(myList)
for cl in myList:
    curImg = cv2.imread(f'{path}/{cl}')
    images.append(curImg)
    classNames.append(os.path.splitext(cl)[0])
print(classNames)

encodeListKnown = []
for img in images:
    img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
    encode = face_recognition.face_encodings(img)[0]
    encodeListKnown.append(encode)

print('Encoding Complete')

#open a window named webcam
cv2.namedWindow('Webcam', cv2.WINDOW_NORMAL)


# Start capturing the video stream from the ESP32-CAM module
cap = cv2.VideoCapture(url)

while True:
    # Read a frame from the video stream
    ret, frame = cap.read()

    # Exit the loop if no frame was read
    if not ret:
        print("Error reading frame from video stream")
        break

    # Resize the frame and convert it to grayscale
    imgS = cv2.resize(frame, (0, 0), None, 0.25, 0.25)
    imgS = cv2.cvtColor(imgS, cv2.COLOR_BGR2RGB)

    # Detect faces in the resized frame and encode them
    facesCurFrame = face_recognition.face_locations(imgS)
    encodesCurFrame = face_recognition.face_encodings(imgS, facesCurFrame)

    # Check if any of the detected faces match with the reference faces
    for encodeFace, faceLoc in zip(encodesCurFrame, facesCurFrame):
        matches = face_recognition.compare_faces(encodeListKnown, encodeFace)
        faceDis = face_recognition.face_distance(encodeListKnown, encodeFace)
        matchIndex = np.argmin(faceDis)

        # If a match is found, mark the attendance and draw a rectangle around the face
        if matches[matchIndex]:
            name = classNames[matchIndex].upper()
            y1, x2, y2, x1 = faceLoc
            y1, x2, y2, x1 = y1 * 4, x2 * 4, y2 * 4, x1 * 4
        cv2.rectangle(frame, (x1, y1), (x2, y2), (0, 255, 0), 2)
        cv2.rectangle(frame, (x1, y2 - 35), (x2, y2), (0, 255, 0), cv2.FILLED)
        cv2.putText(frame, name, (x1 + 6, y2 - 6), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 255, 255), 2)


    cv2.imshow('Webcam', frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()