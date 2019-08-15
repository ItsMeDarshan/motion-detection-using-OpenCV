# motion-detection-using-OpenCV
This program senses the motion of an object moving infront of webcam.


import cv2
import numpy as np
import matplotlib.pyplot as plt

# Camera Resolution #
   
   w = 800
   h = 600
   
   cap = cv2.VideoCapture(0)
   
   cap.set(3, w)
   cap.set(4, h)
   
   print cap.get(3)
   print cap.get(4)
   
   
   ret, frame1 = cap.read()
   ret, frame2 = cap.read()
   
while True:
    
    absdiff = cv2.absdiff(frame1, frame2)
    
    cv2.imshow('absdiff', absdiff)
    
    if cv2.waitKey() == 27:
        break
        
cv2.destroyAllWindows()
       
   
   
    
