# FacialRecognize

mediante el archivo xml puedes reconocer ciertos patrones asociados a un rostro.


```python
import cv2 as cv

cap = cv.VideoCapture(0)
while(True):
    ret, img = cap.read()
    if ret == True:
        img2 = cv.cvtColor(img, cv.COLOR_BGR2HSV)
        ubb=(100,0,0)
        uba=(110, 255, 255)
        mask = cv.inRange(img2, ubb, uba) 
        res = cv.bitwise_and(img,img, mask-mask)
        cv.imshow('img2', img)
        cv.imshow('res', res)
        cv.imshow('mask', mask)
        k =cv.waitKey(20) & 0xFF
        if k == 27 :
            break
cap.release()
cv.destroyAllWindows()
```


```python
import numpy as np
import cv2 as cv
import math
rostro = cv.CascadeClassifier('C:\\Users\\Jorgi\\Desktop\\Inteligencia Artificial\\haarcascade_frontalface_alt.xml')
#rostro = cv.CascadeClassifier('C:\\Users\\Jorgi\\Pictures\\haarcascade_frontalface_alt.xml')
#rostro = cv.CascadeClassifier('C:\\Users\\Px Angel\\Documents\\IA\\Material\\haarcascade_frontalface_alt.xml')
```


```python
cap = cv.VideoCapture(0)
while True:
    ret, frame = cap.read()
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    rostros = rostro.detectMultiScale(gray, 1.3, 5)
    for(x, y, w, h) in rostros:
        frame = cv.rectangle(frame, (x,y), (x+w, y+h), (0, 255, 0), 2)
    cv.imshow('rostros', frame)
    k =cv.waitKey(1)
    if k == 27 :
        break
cap.release()
cv.destroyAllWindows()
```

Podemos guardar el segmentado del rostro encontrado, con esto crearemos nuestro propio DataSet; solo falta acondicionar el tamaño real del pixelaje.


```python
cap = cv.VideoCapture(0)
i = 0
while True:
    ret, frame = cap.read()
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    rostros = rostro.detectMultiScale(gray, 1.3, 5)
    for(x, y, w, h) in rostros:
        #frame = cv.rectangle(frame, (x,y), (x+w, y+h), (0, 255, 0), 2)
        frame2 = frame[y:y+h, x:x+w]
        cv.imshow('rostros encontrados', frame2)
        cv.imwrite('D:\\MuestreoRostros\\'+str(i)+'.png', frame2)
    #cv.imshow('rostros', frame) 
    i=i+1
    k =cv.waitKey(1)
    if k == 27 :
        break
cap.release()
cv.destroyAllWindows()
```
