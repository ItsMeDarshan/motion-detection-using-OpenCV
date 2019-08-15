# motion-detection-using-OpenCV
This program senses the motion of an object, moving infront of webcam.


import cv2
import numpy as np
import matplotlib.pyplot as plt




cap = cv2.VideoCapture(0)



print cap.get(3)
print cap.get(4)

ret, frame1 = cap.read()
ret, frame2 = cap.read()

fourcc = cv2.VideoWriter_fourcc(*'XVID')
out = cv2.VideoWriter('output.avi', fourcc, 30.0, (640,480))
outcolor = cv2.VideoWriter('outcolor.avi',fourcc,30.0,(640,480))


while True:

   
    absdiff = cv2.absdiff(frame1, frame2)
    gray = cv2.cvtColor(absdiff, cv2.COLOR_BGR2GRAY)
    ret, grayThres = cv2.threshold(gray, 15,255, cv2.THRESH_BINARY)
    grayBlur = cv2.GaussianBlur(gray, (3,3), 2)
    grayThresBlur = cv2.GaussianBlur(grayThres, (3,3), 2)
    dilated = cv2.dilate(grayThresBlur, np.ones((3,3),np.uint8), iterations = 10)
    opening = cv2.morphologyEx(dilated, cv2.MORPH_OPEN,(5,5))
    closing = cv2.morphologyEx(opening, cv2.MORPH_CLOSE, (5,5))

    contours,hierarchy = cv2.findContours(closing, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)

    cv2.drawContours(frame1, contours , -1, (0,255,0),2)

    img = cv2.merge((grayThres,grayThres,grayThres))
    
    out.write(img)
    outcolor.write(frame1)

    

    
    cv2.imshow('absdiff', absdiff)
    cv2.imshow('gray', gray)
    cv2.imshow('frame2', frame2)
    cv2.imshow('grayThres', grayThres)
    cv2.imshow('grayThresBlur', grayThresBlur)
    cv2.imshow('dilated', dilated)
    cv2.imshow('frame1', frame1)
    


    frame3 = frame1
    frame1 = frame2
    ret, frame2 = cap.read()

    if cv2.waitKey(1) == 27:
        break
cv2.destroyAllWindows()
cap.release()
outcolor.release()
out.release()

       
   
   
    
