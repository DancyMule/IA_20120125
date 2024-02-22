# SEGEMENTACIÓN DE COLORES EN MÁSCARAS

Esto se realiza para poder identificar una combinación de máscaras, que viene siendo una serie de colores; en el caso de "¿Dónde está Wally?" posee una combinación de colores muy específica; por lo que con este metodo podemos encontrarlo más facilmente


```python
import cv2 as cv
img = cv.imread('C:\\Users\\Jorgi\\pictures\imagen5.png', 1)
img2 = cv.cvtColor(img, cv.COLOR_BGR2HSV)

ubb = (0,60,0)
uba = (10, 255, 255)

ubb1 = (170,60,60)
uba1 = (180,255,255)

ubb2 = (200, 10, 0)
uba2 = (245, 209, 133)

ubb3 = (0, 100, 203)
uba3 = (81, 100, 203)


mask1 = cv.inRange(img2, ubb ,uba)
mask2 = cv.inRange(img2, ubb1 ,uba1)
mask3 = cv.inRange(img2, ubb2 ,uba2)
mask4 = cv.inRange(img2, ubb3 ,uba3)

mask5 = mask1 + mask2 + mask3 + mask4

img3 = cv.bitwise_and(img, img, mask=mask5)
cv.imshow('img', img3)
#cv.imshow('img2', img)
cv.waitKey(0)
cv.destroyAllWindows()
```

Podemos colocar las escalas HSV del rango de colores deseado


```python
cap = cv.VideoCapture(0)
while(True):
    ret, img = cap.read()
    r,g,b = cv.split(img)
    img2 = cv.cvtColor(img, cv.COLOR_BGR2HSV)
    ubb = (145, 250, 183)
    uba = (19, 3, 28)
    mask = cv.inRange(img, ubb ,uba)
    img3 = cv.bitwise_and(img, img, mask=mask)
    if ret == True:
        
        cv.imshow('video', img)
        #cv.imshow('video2', img2)
        cv.imshow('video2', img3)
        #imshow('r', r)
        #cv.imshow('g', g)
        #cv.imshow('b', b)
        k = cv.waitKey(1) & 0xFF
        if k == 27 :
            break
    else:
        break
cap.release()
cv.destroyAllWindows()
```


```python
import cv2 as cv
```


```python

```
