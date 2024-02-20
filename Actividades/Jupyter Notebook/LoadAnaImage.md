# IMPORTAR UNA IMAGEN


```python
import cv2 as cv
```


```python
img = cv.imread('C:\\Users\\Jorgi\\pictures\\imagen.jpg', 1)
print(img.shape)
w, h = img.shape
cv.imshow('Ejemplo', img)
for i in range(w):
    for j in range(h):
        #img[i,j] = 255-img[i,j]
        if(img[i,j]>150):
            img[i,j]=255
        else:
            img[i,j]=0
cv.imshow('Ejemplo2', img)            
cv.waitKey(0)
cv.destroyAllWindows()
```

    (1000, 1000, 3)
    


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    Cell In[15], line 3
          1 img = cv.imread('C:\\Users\\Jorgi\\pictures\\imagen.jpg', 1)
          2 print(img.shape)
    ----> 3 w, h = img.shape
          4 cv.imshow('Ejemplo', img)
          5 for i in range(w):
    

    ValueError: too many values to unpack (expected 2)



```python
cv.imshow('Ejemplo', img)
cv.waitKey(0)
cv.destroyAllWindows()
```


```python

```
