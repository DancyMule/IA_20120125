# Proyecto final

## Para el proyecto de reconocimiento de emociones se utilizó LBPH con un dataSet de un aproximado de 25 personas en distintas iluminaciones

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

cap = cv.VideoCapture(0) #Aqui se podría el link del video
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

Con la información (frame) ya segmenteado (frame 2) ahora debemos estandarizar el tamaño. 

## LBPH

Se genera el XML apuntando al directorio del dataSet


```python
import cv2 as cv 
import numpy as np 
import os

dataSet = 'C:\\Users\\Jorgi\\Desktop\\emociones3'
faces  = os.listdir(dataSet)
print(faces)

labels = []
facesData = []
label = 0 
for face in faces:
    facePath = dataSet+'\\'+face
    for faceName in os.listdir(facePath):
        labels.append(label)
        facesData.append(cv.imread(facePath+'\\'+faceName,0))
    label = label + 1
#print(np.count_nonzero(np.array(labels)==0)) 
faceRecognizer = cv.face.LBPHFaceRecognizer_create()
faceRecognizer.train(facesData, np.array(labels))
faceRecognizer.write('C:\\Users\\Jorgi\\Desktop\\XML2\\EmocionesLBPHDrei.xml')
```

    ['enojo', 'feliz', 'sorpresa']
    


```python
import cv2 as cv
import os 

faceRecognizer = cv.face.LBPHFaceRecognizer_create()
faceRecognizer.read('C:\\Users\\Jorgi\\Desktop\\XML2\\EmocionesLBPHDrei.xml')
dataSet = 'C:\\Users\\Jorgi\\Desktop\\emociones3'
faces  = os.listdir(dataSet)
cap = cv.VideoCapture(0)
rostro = cv.CascadeClassifier('C:\\Users\\Jorgi\\Desktop\\Inteligencia Artificial\\haarcascade_frontalface_alt.xml')
while True:
    ret, frame = cap.read()
    if ret == False: break
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    cpGray = gray.copy()
    rostros = rostro.detectMultiScale(gray, 1.3, 3)
    for(x, y, w, h) in rostros:
        frame2 = cpGray[y:y+h, x:x+w]
        frame2 = cv.resize(frame2,  (100,100), interpolation=cv.INTER_CUBIC)
        result = faceRecognizer.predict(frame2)
        cv.putText(frame, '{}'.format(result), (x,y-20), 1,3.3, (255,255,0), 1, cv.LINE_AA)
        if result[1] < 100:
            cv.putText(frame,'{}'.format(faces[result[0]]),(x,y-25),2,1.1,(0,255,0),1,cv.LINE_AA)
            cv.rectangle(frame, (x,y),(x+w,y+h),(0,255,0),2)
        else:
            cv.putText(frame,'Desconocido',(x,y-20),2,0.8,(0,0,255),1,cv.LINE_AA)
            cv.rectangle(frame, (x,y),(x+w,y+h),(0,0,255),2) 
    cv.imshow('frame', frame)
    k = cv.waitKey(1)
    if k == 27:
        break
cap.release()
cv.destroyAllWindows()


```

Luego procederemos a leer el XML con el Haraascade y encendemos la cámara


```python
import numpy as np
import cv2 as cv
import math
rostro = cv.CascadeClassifier('C:\\Users\\Jorgi\\Desktop\\Inteligencia Artificial\\haarcascade_frontalface_alt.xml')

cap = cv.VideoCapture(0)
i = 0
while True:
    ret, frame = cap.read()
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    rostros = rostro.detectMultiScale(gray, 1.3, 5)
    for(x, y, w, h) in rostros:
        #frame = cv.rectangle(frame, (x,y), (x+w, y+h), (0, 255, 0), 2)
        frame2 = frame[y:y+h, x:x+w]
        frame2 = cv.resize(frame2, (100, 100), interpolation = cv.INTER_AREA)
        cv.imshow('rostros encontrados', frame2)
        cv.imwrite('C:\\Users\\Jorgi\\Desktop\\o\\DancyWAKWAWAKAAA'+str(i)+'.png', frame2)
    #cv.imshow('rostros', frame) 
    i=i+1
    k =cv.waitKey(1)
    if k == 27 :
        break
cap.release()
cv.destroyAllWindows()
```

En el resto de celdas están la manera de trabajarde con Eigenface y FisherFace, opciones que fueron descartadas.


## EIGENFACE

Generar xml


```python
import cv2 as cv 
import numpy as np 
import os

dataSet = 'C:\\Users\\Jorgi\\Desktop\\Emociones3'
faces  = os.listdir(dataSet)
print(faces)

labels = []
facesData = []
label = 0 
for face in faces:
    facePath = dataSet+'\\'+face
    for faceName in os.listdir(facePath):
        labels.append(label)
        facesData.append(cv.imread(facePath+'\\'+faceName,0))
    label = label + 1
print(np.count_nonzero(np.array(labels)==0))
faceRecognizer = cv.face.EigenFaceRecognizer_create()
faceRecognizer.train(facesData, np.array(labels))
faceRecognizer.write('C:\\Users\\Jorgi\\Desktop\\XML2\\Perrites2.xml')

```

A buscar rostros


```python
import cv2 as cv
import os 

faceRecognizer = cv.face.EigenFaceRecognizer_create()
faceRecognizer.read('C:\\Users\\Jorgi\\Desktop\\XML2\\Perrites2.xml')
dataSet = 'C:\\Users\\Jorgi\\Desktop\\Emociones3'
faces  = os.listdir(dataSet)
cap = cv.VideoCapture(0)
rostro = cv.CascadeClassifier('C:\\Users\\Jorgi\\Desktop\\Inteligencia Artificial\\haarcascade_frontalface_alt.xml')
while True:
    ret, frame = cap.read()
    if ret == False: break
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    cpGray = gray.copy()
    rostros = rostro.detectMultiScale(gray, 1.3, 3)
    for(x, y, w, h) in rostros:
        frame2 = cpGray[y:y+h, x:x+w]
        frame2 = cv.resize(frame2,  (100,100), interpolation=cv.INTER_CUBIC)
        result = faceRecognizer.predict(frame2)
        #cv.putText(frame, '{}'.format(result), (x,y-20), 1,3.3, (0,0,0), 1, cv.LINE_AA)
        if result[1] > 2800:
            cv.putText(frame,'{}'.format(faces[result[0]]),(x,y-25),2,1.1,(0,255,0),1,cv.LINE_AA)
            cv.rectangle(frame, (x,y),(x+w,y+h),(0,255,0),2)
        else:
            cv.putText(frame,'Desconocido',(x,y-20),2,0.8,(0,0,255),1,cv.LINE_AA)
            cv.rectangle(frame, (x,y),(x+w,y+h),(0,0,255),2)
    cv.imshow('frame', frame)
    k = cv.waitKey(1)
    if k == 27:
        break
cap.release()
cv.destroyAllWindows()
```

## FISHERFACE

Siempre hay que tener en cuenta el tamaño de la imagen y que no se mezcle la información

Puedes cambiar el nombre a los archivos, lo que importa es que estén contenidos dentro de una misma carpeta

Debemos crear un Fisher Face


```python
import cv2 as cv 
import numpy as np 
import os

dataSet = 'D:\\MuestreoRostros'
faces  = os.listdir(dataSet)
print(faces)

labels = []
facesData = []
label = 0 
for face in faces:
    facePath = dataSet+'\\'+face
    for faceName in os.listdir(facePath):
        labels.append(label)
        facesData.append(cv.imread(facePath+'\\'+faceName,0))
    label = label + 1
#print(np.count_nonzero(np.array(labels)==0)) 
faceRecognizer = cv.face.FisherFaceRecognizer_create()
faceRecognizer.train(facesData, np.array(labels))
faceRecognizer.write('D:\\XML\\FisherFace\\JorgeFisherface.xml')


```

Así mismo, la lectura se hará con Fisher

 Creamos el XML


```python
import cv2 as cv
import os 

faceRecognizer = cv.face.FisherFaceRecognizer_create()
faceRecognizer.read('D:\\XML\\FisherFace\\JorgeFisherface.xml')
dataSet = 'D:\\MuestreoRostros'
faces  = os.listdir(dataSet)
cap = cv.VideoCapture(0)
rostro = cv.CascadeClassifier('C:\\Users\\Jorgi\\Desktop\\Inteligencia Artificial\\haarcascade_frontalface_alt.xml')
while True:
    ret, frame = cap.read()
    if ret == False: break
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    cpGray = gray.copy()
    rostros = rostro.detectMultiScale(gray, 1.3, 3)
    for(x, y, w, h) in rostros:
        frame2 = cpGray[y:y+h, x:x+w]
        frame2 = cv.resize(frame2,  (100,100), interpolation=cv.INTER_CUBIC)
        result = faceRecognizer.predict(frame2)
        cv.putText(frame, '{}'.format(result), (x,y-20), 1,3.3, (255,255,0), 1, cv.LINE_AA)
        if result[1] < 500:
            cv.putText(frame,'{}'.format(faces[result[0]]),(x,y-25),2,1.1,(0,255,0),1,cv.LINE_AA)
            cv.rectangle(frame, (x,y),(x+w,y+h),(0,255,0),2)
        else:
            cv.putText(frame,'Desconocido',(x,y-20),2,0.8,(0,0,255),1,cv.LINE_AA)
            cv.rectangle(frame, (x,y),(x+w,y+h),(0,0,255),2)
    cv.imshow('frame', frame)
    k = cv.waitKey(1)
    if k == 27:
        break
cap.release()
cv.destroyAllWindows()
```


```python
import cv2 as cv
import os 

faceRecognizer = cv.face.LBPHFaceRecognizer_create()
faceRecognizer.read('C:\\Users\\Jorgi\\Desktop\\XML2\\PersonesLBPH3.xml')
dataSet = 'C:\\Users\\Jorgi\\Desktop\\persones'
faces  = os.listdir(dataSet)
cap = cv.VideoCapture(0)
rostro = cv.CascadeClassifier('C:\\Users\\Jorgi\\Desktop\\Inteligencia Artificial\\haarcascade_frontalface_alt.xml')
while True:
    ret, frame = cap.read()
    if ret == False: break
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    cpGray = gray.copy()
    rostros = rostro.detectMultiScale(gray, 1.3, 3)
    for(x, y, w, h) in rostros:
        frame2 = cpGray[y:y+h, x:x+w]
        frame2 = cv.resize(frame2,  (100,100), interpolation=cv.INTER_CUBIC)
        result = faceRecognizer.predict(frame2)
        cv.putText(frame, '{}'.format(result), (x,y-20), 1,3.3, (255,255,0), 1, cv.LINE_AA)
        if result[1] < 100:
            cv.putText(frame,'{}'.format(faces[result[0]]),(x,y-25),2,1.1,(0,255,0),1,cv.LINE_AA)
            cv.rectangle(frame, (x,y),(x+w,y+h),(0,255,0),2)
        else:
            cv.putText(frame,'Desconocido',(x,y-20),2,0.8,(0,0,255),1,cv.LINE_AA)
            cv.rectangle(frame, (x,y),(x+w,y+h),(0,0,255),2) 
    cv.imshow('frame', frame)
    k = cv.waitKey(1)
    if k == 27:
        break
cap.release()
cv.destroyAllWindows()


```

## RESIZE


```python
# Importar las bibliotecas necesarias
import numpy as np
import cv2 as cv
import os

# Definir la ruta a la carpeta con las imágenes
folder_path = 'D:\\ZWally\\Ejercicios'  # Reemplaza con la ruta correcta
# Definir la función para redimensionar imágenes
def resize_images(folder_path, size=(50, 50)):
    # Verificar si la carpeta existe
    if not os.path.exists(folder_path):
        print(f"La carpeta {folder_path} no existe.")
        return
    
    # Iterar sobre todos los archivos en la carpeta
    for filename in os.listdir(folder_path):
        if filename.endswith(('.png', '.jpg', '.jpeg', '.bmp', '.gif')):
            try:
                img_path = os.path.join(folder_path, filename)
                img = cv.imread(img_path)
                if img is not None:
                    # Redimensionar la imagen
                    img_resized = cv.resize(img, size, interpolation=cv.INTER_AREA)
                    # Sobrescribir la imagen original con la imagen redimensionada
                    cv.imwrite(img_path, img_resized)
                    print(f"Imagen {filename} redimensionada a {size}.")
                else:
                    print(f"No se pudo leer la imagen {filename}.")
            except Exception as e:
                print(f"No se pudo redimensionar la imagen {filename}: {e}")


# Llamar a la función para redimensionar imágenes
resize_images(folder_path)

```

    Imagen wally.png redimensionada a (65, 65).
    Imagen wally1.jpg redimensionada a (65, 65).
    Imagen wally10.jpg redimensionada a (65, 65).
    Imagen wally12.jpg redimensionada a (65, 65).
    Imagen wally13.jpg redimensionada a (65, 65).
    Imagen Wally2.jpg redimensionada a (65, 65).
    Imagen wally2.png redimensionada a (65, 65).
    Imagen wally3.jpg redimensionada a (65, 65).
    Imagen wally3.png redimensionada a (65, 65).
    Imagen wally4.png redimensionada a (65, 65).
    Imagen wally5.jpg redimensionada a (65, 65).
    Imagen wally6.jpg redimensionada a (65, 65).
    Imagen wally7.jpg redimensionada a (65, 65).
    Imagen wally8.jpg redimensionada a (65, 65).
    

## CAMBIAR NOMBRE


```python
import os
import glob

def renombrar_imagenes(ruta):
    # Verificar si la ruta existe
    if not os.path.exists(ruta):
        print(f"La ruta {ruta} no existe.")
        return

    # Buscar todas las imágenes en la ruta (considerando las extensiones más comunes)
    imagenes = glob.glob(os.path.join(ruta, "*.png")) + \
               glob.glob(os.path.join(ruta, "*.jpg")) + \
               glob.glob(os.path.join(ruta, "*.jpeg")) + \
               glob.glob(os.path.join(ruta, "*.bmp")) + \
               glob.glob(os.path.join(ruta, "*.gif"))

    if not imagenes:
        print(f"No se encontraron imágenes en la ruta {ruta}.")
        return

    # Renombrar cada imagen
    for i, imagen in enumerate(imagenes):
        # Obtener la extensión del archivo
        extension = os.path.splitext(imagen)[1]
        # Crear el nuevo nombre
        nuevo_nombre = f"WallyPos{i}.png" #aqui colocar el nombre deseado de las imagenes
        # Obtener la ruta completa del nuevo nombre
        nueva_ruta = os.path.join(ruta, nuevo_nombre)
        # Renombrar el archivo
        os.rename(imagen, nueva_ruta)
        print(f"Renombrado: {imagen} -> {nueva_ruta}")

    print("Renombrado completado.")

# Forma de usar
ruta_imagenes = "D:\\ZWally\\DataSetWally\\p"
renombrar_imagenes(ruta_imagenes)
```

    Renombrado: D:\ZWally\DataSetWally\p\10.png -> D:\ZWally\DataSetWally\p\WallyPos0.png
    Renombrado: D:\ZWally\DataSetWally\p\100.png -> D:\ZWally\DataSetWally\p\WallyPos1.png
    Renombrado: D:\ZWally\DataSetWally\p\101.png -> D:\ZWally\DataSetWally\p\WallyPos2.png
    Renombrado: D:\ZWally\DataSetWally\p\102.png -> D:\ZWally\DataSetWally\p\WallyPos3.png
    Renombrado: D:\ZWally\DataSetWally\p\103.png -> D:\ZWally\DataSetWally\p\WallyPos4.png
    Renombrado: D:\ZWally\DataSetWally\p\104.png -> D:\ZWally\DataSetWally\p\WallyPos5.png
    Renombrado: D:\ZWally\DataSetWally\p\105.png -> D:\ZWally\DataSetWally\p\WallyPos6.png
    Renombrado: D:\ZWally\DataSetWally\p\106.png -> D:\ZWally\DataSetWally\p\WallyPos7.png
    Renombrado: D:\ZWally\DataSetWally\p\107.png -> D:\ZWally\DataSetWally\p\WallyPos8.png
    Renombrado: D:\ZWally\DataSetWally\p\108.png -> D:\ZWally\DataSetWally\p\WallyPos9.png
    Renombrado: D:\ZWally\DataSetWally\p\109.png -> D:\ZWally\DataSetWally\p\WallyPos10.png
    Renombrado: D:\ZWally\DataSetWally\p\11.png -> D:\ZWally\DataSetWally\p\WallyPos11.png
    Renombrado: D:\ZWally\DataSetWally\p\110.png -> D:\ZWally\DataSetWally\p\WallyPos12.png
    Renombrado: D:\ZWally\DataSetWally\p\112.png -> D:\ZWally\DataSetWally\p\WallyPos13.png
    Renombrado: D:\ZWally\DataSetWally\p\113.png -> D:\ZWally\DataSetWally\p\WallyPos14.png
    Renombrado: D:\ZWally\DataSetWally\p\114.png -> D:\ZWally\DataSetWally\p\WallyPos15.png
    Renombrado: D:\ZWally\DataSetWally\p\115.png -> D:\ZWally\DataSetWally\p\WallyPos16.png
    Renombrado: D:\ZWally\DataSetWally\p\116.png -> D:\ZWally\DataSetWally\p\WallyPos17.png
    Renombrado: D:\ZWally\DataSetWally\p\117.png -> D:\ZWally\DataSetWally\p\WallyPos18.png
    Renombrado: D:\ZWally\DataSetWally\p\118.png -> D:\ZWally\DataSetWally\p\WallyPos19.png
    Renombrado: D:\ZWally\DataSetWally\p\119.png -> D:\ZWally\DataSetWally\p\WallyPos20.png
    Renombrado: D:\ZWally\DataSetWally\p\12.png -> D:\ZWally\DataSetWally\p\WallyPos21.png
    Renombrado: D:\ZWally\DataSetWally\p\120.png -> D:\ZWally\DataSetWally\p\WallyPos22.png
    Renombrado: D:\ZWally\DataSetWally\p\121.png -> D:\ZWally\DataSetWally\p\WallyPos23.png
    Renombrado: D:\ZWally\DataSetWally\p\122.png -> D:\ZWally\DataSetWally\p\WallyPos24.png
    Renombrado: D:\ZWally\DataSetWally\p\124.png -> D:\ZWally\DataSetWally\p\WallyPos25.png
    Renombrado: D:\ZWally\DataSetWally\p\126.png -> D:\ZWally\DataSetWally\p\WallyPos26.png
    Renombrado: D:\ZWally\DataSetWally\p\127.png -> D:\ZWally\DataSetWally\p\WallyPos27.png
    Renombrado: D:\ZWally\DataSetWally\p\128.png -> D:\ZWally\DataSetWally\p\WallyPos28.png
    Renombrado: D:\ZWally\DataSetWally\p\129.png -> D:\ZWally\DataSetWally\p\WallyPos29.png
    Renombrado: D:\ZWally\DataSetWally\p\13.png -> D:\ZWally\DataSetWally\p\WallyPos30.png
    Renombrado: D:\ZWally\DataSetWally\p\130.png -> D:\ZWally\DataSetWally\p\WallyPos31.png
    Renombrado: D:\ZWally\DataSetWally\p\131.png -> D:\ZWally\DataSetWally\p\WallyPos32.png
    Renombrado: D:\ZWally\DataSetWally\p\132.png -> D:\ZWally\DataSetWally\p\WallyPos33.png
    Renombrado: D:\ZWally\DataSetWally\p\133.png -> D:\ZWally\DataSetWally\p\WallyPos34.png
    Renombrado: D:\ZWally\DataSetWally\p\134.png -> D:\ZWally\DataSetWally\p\WallyPos35.png
    Renombrado: D:\ZWally\DataSetWally\p\135.png -> D:\ZWally\DataSetWally\p\WallyPos36.png
    Renombrado: D:\ZWally\DataSetWally\p\136.png -> D:\ZWally\DataSetWally\p\WallyPos37.png
    Renombrado: D:\ZWally\DataSetWally\p\138.png -> D:\ZWally\DataSetWally\p\WallyPos38.png
    Renombrado: D:\ZWally\DataSetWally\p\139.png -> D:\ZWally\DataSetWally\p\WallyPos39.png
    Renombrado: D:\ZWally\DataSetWally\p\14.png -> D:\ZWally\DataSetWally\p\WallyPos40.png
    Renombrado: D:\ZWally\DataSetWally\p\140.png -> D:\ZWally\DataSetWally\p\WallyPos41.png
    Renombrado: D:\ZWally\DataSetWally\p\141.png -> D:\ZWally\DataSetWally\p\WallyPos42.png
    Renombrado: D:\ZWally\DataSetWally\p\142.png -> D:\ZWally\DataSetWally\p\WallyPos43.png
    Renombrado: D:\ZWally\DataSetWally\p\143.png -> D:\ZWally\DataSetWally\p\WallyPos44.png
    Renombrado: D:\ZWally\DataSetWally\p\144.png -> D:\ZWally\DataSetWally\p\WallyPos45.png
    Renombrado: D:\ZWally\DataSetWally\p\145.png -> D:\ZWally\DataSetWally\p\WallyPos46.png
    Renombrado: D:\ZWally\DataSetWally\p\146.png -> D:\ZWally\DataSetWally\p\WallyPos47.png
    Renombrado: D:\ZWally\DataSetWally\p\147.png -> D:\ZWally\DataSetWally\p\WallyPos48.png
    Renombrado: D:\ZWally\DataSetWally\p\148.png -> D:\ZWally\DataSetWally\p\WallyPos49.png
    Renombrado: D:\ZWally\DataSetWally\p\150.png -> D:\ZWally\DataSetWally\p\WallyPos50.png
    Renombrado: D:\ZWally\DataSetWally\p\151.png -> D:\ZWally\DataSetWally\p\WallyPos51.png
    Renombrado: D:\ZWally\DataSetWally\p\152.png -> D:\ZWally\DataSetWally\p\WallyPos52.png
    Renombrado: D:\ZWally\DataSetWally\p\153.png -> D:\ZWally\DataSetWally\p\WallyPos53.png
    Renombrado: D:\ZWally\DataSetWally\p\154.png -> D:\ZWally\DataSetWally\p\WallyPos54.png
    Renombrado: D:\ZWally\DataSetWally\p\155.png -> D:\ZWally\DataSetWally\p\WallyPos55.png
    Renombrado: D:\ZWally\DataSetWally\p\156.png -> D:\ZWally\DataSetWally\p\WallyPos56.png
    Renombrado: D:\ZWally\DataSetWally\p\157.png -> D:\ZWally\DataSetWally\p\WallyPos57.png
    Renombrado: D:\ZWally\DataSetWally\p\158.png -> D:\ZWally\DataSetWally\p\WallyPos58.png
    Renombrado: D:\ZWally\DataSetWally\p\159.png -> D:\ZWally\DataSetWally\p\WallyPos59.png
    Renombrado: D:\ZWally\DataSetWally\p\16.png -> D:\ZWally\DataSetWally\p\WallyPos60.png
    Renombrado: D:\ZWally\DataSetWally\p\160.png -> D:\ZWally\DataSetWally\p\WallyPos61.png
    Renombrado: D:\ZWally\DataSetWally\p\162.png -> D:\ZWally\DataSetWally\p\WallyPos62.png
    Renombrado: D:\ZWally\DataSetWally\p\163.png -> D:\ZWally\DataSetWally\p\WallyPos63.png
    Renombrado: D:\ZWally\DataSetWally\p\164.png -> D:\ZWally\DataSetWally\p\WallyPos64.png
    Renombrado: D:\ZWally\DataSetWally\p\165.png -> D:\ZWally\DataSetWally\p\WallyPos65.png
    Renombrado: D:\ZWally\DataSetWally\p\166.png -> D:\ZWally\DataSetWally\p\WallyPos66.png
    Renombrado: D:\ZWally\DataSetWally\p\167.png -> D:\ZWally\DataSetWally\p\WallyPos67.png
    Renombrado: D:\ZWally\DataSetWally\p\168.png -> D:\ZWally\DataSetWally\p\WallyPos68.png
    Renombrado: D:\ZWally\DataSetWally\p\169.png -> D:\ZWally\DataSetWally\p\WallyPos69.png
    Renombrado: D:\ZWally\DataSetWally\p\17.png -> D:\ZWally\DataSetWally\p\WallyPos70.png
    Renombrado: D:\ZWally\DataSetWally\p\170.png -> D:\ZWally\DataSetWally\p\WallyPos71.png
    Renombrado: D:\ZWally\DataSetWally\p\171.png -> D:\ZWally\DataSetWally\p\WallyPos72.png
    Renombrado: D:\ZWally\DataSetWally\p\172.png -> D:\ZWally\DataSetWally\p\WallyPos73.png
    Renombrado: D:\ZWally\DataSetWally\p\174.png -> D:\ZWally\DataSetWally\p\WallyPos74.png
    Renombrado: D:\ZWally\DataSetWally\p\175.png -> D:\ZWally\DataSetWally\p\WallyPos75.png
    Renombrado: D:\ZWally\DataSetWally\p\176.png -> D:\ZWally\DataSetWally\p\WallyPos76.png
    Renombrado: D:\ZWally\DataSetWally\p\177.png -> D:\ZWally\DataSetWally\p\WallyPos77.png
    Renombrado: D:\ZWally\DataSetWally\p\178.png -> D:\ZWally\DataSetWally\p\WallyPos78.png
    Renombrado: D:\ZWally\DataSetWally\p\179.png -> D:\ZWally\DataSetWally\p\WallyPos79.png
    Renombrado: D:\ZWally\DataSetWally\p\18.png -> D:\ZWally\DataSetWally\p\WallyPos80.png
    Renombrado: D:\ZWally\DataSetWally\p\180.png -> D:\ZWally\DataSetWally\p\WallyPos81.png
    Renombrado: D:\ZWally\DataSetWally\p\181.png -> D:\ZWally\DataSetWally\p\WallyPos82.png
    Renombrado: D:\ZWally\DataSetWally\p\182.png -> D:\ZWally\DataSetWally\p\WallyPos83.png
    Renombrado: D:\ZWally\DataSetWally\p\183.png -> D:\ZWally\DataSetWally\p\WallyPos84.png
    Renombrado: D:\ZWally\DataSetWally\p\184.png -> D:\ZWally\DataSetWally\p\WallyPos85.png
    Renombrado: D:\ZWally\DataSetWally\p\186.png -> D:\ZWally\DataSetWally\p\WallyPos86.png
    Renombrado: D:\ZWally\DataSetWally\p\187.png -> D:\ZWally\DataSetWally\p\WallyPos87.png
    Renombrado: D:\ZWally\DataSetWally\p\188.png -> D:\ZWally\DataSetWally\p\WallyPos88.png
    Renombrado: D:\ZWally\DataSetWally\p\189.png -> D:\ZWally\DataSetWally\p\WallyPos89.png
    Renombrado: D:\ZWally\DataSetWally\p\19.png -> D:\ZWally\DataSetWally\p\WallyPos90.png
    Renombrado: D:\ZWally\DataSetWally\p\190.png -> D:\ZWally\DataSetWally\p\WallyPos91.png
    Renombrado: D:\ZWally\DataSetWally\p\191.png -> D:\ZWally\DataSetWally\p\WallyPos92.png
    Renombrado: D:\ZWally\DataSetWally\p\192.png -> D:\ZWally\DataSetWally\p\WallyPos93.png
    Renombrado: D:\ZWally\DataSetWally\p\193.png -> D:\ZWally\DataSetWally\p\WallyPos94.png
    Renombrado: D:\ZWally\DataSetWally\p\194.png -> D:\ZWally\DataSetWally\p\WallyPos95.png
    Renombrado: D:\ZWally\DataSetWally\p\195.png -> D:\ZWally\DataSetWally\p\WallyPos96.png
    Renombrado: D:\ZWally\DataSetWally\p\196.png -> D:\ZWally\DataSetWally\p\WallyPos97.png
    Renombrado: D:\ZWally\DataSetWally\p\198.png -> D:\ZWally\DataSetWally\p\WallyPos98.png
    Renombrado: D:\ZWally\DataSetWally\p\199.png -> D:\ZWally\DataSetWally\p\WallyPos99.png
    Renombrado: D:\ZWally\DataSetWally\p\2.png -> D:\ZWally\DataSetWally\p\WallyPos100.png
    Renombrado: D:\ZWally\DataSetWally\p\20.png -> D:\ZWally\DataSetWally\p\WallyPos101.png
    Renombrado: D:\ZWally\DataSetWally\p\200.png -> D:\ZWally\DataSetWally\p\WallyPos102.png
    Renombrado: D:\ZWally\DataSetWally\p\201.png -> D:\ZWally\DataSetWally\p\WallyPos103.png
    Renombrado: D:\ZWally\DataSetWally\p\202.png -> D:\ZWally\DataSetWally\p\WallyPos104.png
    Renombrado: D:\ZWally\DataSetWally\p\203.png -> D:\ZWally\DataSetWally\p\WallyPos105.png
    Renombrado: D:\ZWally\DataSetWally\p\204.png -> D:\ZWally\DataSetWally\p\WallyPos106.png
    Renombrado: D:\ZWally\DataSetWally\p\205.png -> D:\ZWally\DataSetWally\p\WallyPos107.png
    Renombrado: D:\ZWally\DataSetWally\p\206.png -> D:\ZWally\DataSetWally\p\WallyPos108.png
    Renombrado: D:\ZWally\DataSetWally\p\207.png -> D:\ZWally\DataSetWally\p\WallyPos109.png
    Renombrado: D:\ZWally\DataSetWally\p\208.png -> D:\ZWally\DataSetWally\p\WallyPos110.png
    Renombrado: D:\ZWally\DataSetWally\p\21.png -> D:\ZWally\DataSetWally\p\WallyPos111.png
    Renombrado: D:\ZWally\DataSetWally\p\210.png -> D:\ZWally\DataSetWally\p\WallyPos112.png
    Renombrado: D:\ZWally\DataSetWally\p\211.png -> D:\ZWally\DataSetWally\p\WallyPos113.png
    Renombrado: D:\ZWally\DataSetWally\p\212.png -> D:\ZWally\DataSetWally\p\WallyPos114.png
    Renombrado: D:\ZWally\DataSetWally\p\213.png -> D:\ZWally\DataSetWally\p\WallyPos115.png
    Renombrado: D:\ZWally\DataSetWally\p\214.png -> D:\ZWally\DataSetWally\p\WallyPos116.png
    Renombrado: D:\ZWally\DataSetWally\p\215.png -> D:\ZWally\DataSetWally\p\WallyPos117.png
    Renombrado: D:\ZWally\DataSetWally\p\216.png -> D:\ZWally\DataSetWally\p\WallyPos118.png
    Renombrado: D:\ZWally\DataSetWally\p\217.png -> D:\ZWally\DataSetWally\p\WallyPos119.png
    Renombrado: D:\ZWally\DataSetWally\p\218.png -> D:\ZWally\DataSetWally\p\WallyPos120.png
    Renombrado: D:\ZWally\DataSetWally\p\219.png -> D:\ZWally\DataSetWally\p\WallyPos121.png
    Renombrado: D:\ZWally\DataSetWally\p\22.png -> D:\ZWally\DataSetWally\p\WallyPos122.png
    Renombrado: D:\ZWally\DataSetWally\p\220.png -> D:\ZWally\DataSetWally\p\WallyPos123.png
    Renombrado: D:\ZWally\DataSetWally\p\222.png -> D:\ZWally\DataSetWally\p\WallyPos124.png
    Renombrado: D:\ZWally\DataSetWally\p\223.png -> D:\ZWally\DataSetWally\p\WallyPos125.png
    Renombrado: D:\ZWally\DataSetWally\p\224.png -> D:\ZWally\DataSetWally\p\WallyPos126.png
    Renombrado: D:\ZWally\DataSetWally\p\225.png -> D:\ZWally\DataSetWally\p\WallyPos127.png
    Renombrado: D:\ZWally\DataSetWally\p\226.png -> D:\ZWally\DataSetWally\p\WallyPos128.png
    Renombrado: D:\ZWally\DataSetWally\p\227.png -> D:\ZWally\DataSetWally\p\WallyPos129.png
    Renombrado: D:\ZWally\DataSetWally\p\228.png -> D:\ZWally\DataSetWally\p\WallyPos130.png
    Renombrado: D:\ZWally\DataSetWally\p\229.png -> D:\ZWally\DataSetWally\p\WallyPos131.png
    Renombrado: D:\ZWally\DataSetWally\p\23.png -> D:\ZWally\DataSetWally\p\WallyPos132.png
    Renombrado: D:\ZWally\DataSetWally\p\230.png -> D:\ZWally\DataSetWally\p\WallyPos133.png
    Renombrado: D:\ZWally\DataSetWally\p\231.png -> D:\ZWally\DataSetWally\p\WallyPos134.png
    Renombrado: D:\ZWally\DataSetWally\p\232.png -> D:\ZWally\DataSetWally\p\WallyPos135.png
    Renombrado: D:\ZWally\DataSetWally\p\234.png -> D:\ZWally\DataSetWally\p\WallyPos136.png
    Renombrado: D:\ZWally\DataSetWally\p\235.png -> D:\ZWally\DataSetWally\p\WallyPos137.png
    Renombrado: D:\ZWally\DataSetWally\p\236.png -> D:\ZWally\DataSetWally\p\WallyPos138.png
    Renombrado: D:\ZWally\DataSetWally\p\237.png -> D:\ZWally\DataSetWally\p\WallyPos139.png
    Renombrado: D:\ZWally\DataSetWally\p\238.png -> D:\ZWally\DataSetWally\p\WallyPos140.png
    Renombrado: D:\ZWally\DataSetWally\p\239.png -> D:\ZWally\DataSetWally\p\WallyPos141.png
    Renombrado: D:\ZWally\DataSetWally\p\24.png -> D:\ZWally\DataSetWally\p\WallyPos142.png
    Renombrado: D:\ZWally\DataSetWally\p\240.png -> D:\ZWally\DataSetWally\p\WallyPos143.png
    Renombrado: D:\ZWally\DataSetWally\p\241.png -> D:\ZWally\DataSetWally\p\WallyPos144.png
    Renombrado: D:\ZWally\DataSetWally\p\242.png -> D:\ZWally\DataSetWally\p\WallyPos145.png
    Renombrado: D:\ZWally\DataSetWally\p\243.png -> D:\ZWally\DataSetWally\p\WallyPos146.png
    Renombrado: D:\ZWally\DataSetWally\p\244.png -> D:\ZWally\DataSetWally\p\WallyPos147.png
    Renombrado: D:\ZWally\DataSetWally\p\246.png -> D:\ZWally\DataSetWally\p\WallyPos148.png
    Renombrado: D:\ZWally\DataSetWally\p\248.png -> D:\ZWally\DataSetWally\p\WallyPos149.png
    Renombrado: D:\ZWally\DataSetWally\p\249.png -> D:\ZWally\DataSetWally\p\WallyPos150.png
    Renombrado: D:\ZWally\DataSetWally\p\25.png -> D:\ZWally\DataSetWally\p\WallyPos151.png
    Renombrado: D:\ZWally\DataSetWally\p\250.png -> D:\ZWally\DataSetWally\p\WallyPos152.png
    Renombrado: D:\ZWally\DataSetWally\p\251.png -> D:\ZWally\DataSetWally\p\WallyPos153.png
    Renombrado: D:\ZWally\DataSetWally\p\252.png -> D:\ZWally\DataSetWally\p\WallyPos154.png
    Renombrado: D:\ZWally\DataSetWally\p\253.png -> D:\ZWally\DataSetWally\p\WallyPos155.png
    Renombrado: D:\ZWally\DataSetWally\p\254.png -> D:\ZWally\DataSetWally\p\WallyPos156.png
    Renombrado: D:\ZWally\DataSetWally\p\255.png -> D:\ZWally\DataSetWally\p\WallyPos157.png
    Renombrado: D:\ZWally\DataSetWally\p\256.png -> D:\ZWally\DataSetWally\p\WallyPos158.png
    Renombrado: D:\ZWally\DataSetWally\p\257.png -> D:\ZWally\DataSetWally\p\WallyPos159.png
    Renombrado: D:\ZWally\DataSetWally\p\258.png -> D:\ZWally\DataSetWally\p\WallyPos160.png
    Renombrado: D:\ZWally\DataSetWally\p\26.png -> D:\ZWally\DataSetWally\p\WallyPos161.png
    Renombrado: D:\ZWally\DataSetWally\p\260.png -> D:\ZWally\DataSetWally\p\WallyPos162.png
    Renombrado: D:\ZWally\DataSetWally\p\261.png -> D:\ZWally\DataSetWally\p\WallyPos163.png
    Renombrado: D:\ZWally\DataSetWally\p\262.png -> D:\ZWally\DataSetWally\p\WallyPos164.png
    Renombrado: D:\ZWally\DataSetWally\p\263.png -> D:\ZWally\DataSetWally\p\WallyPos165.png
    Renombrado: D:\ZWally\DataSetWally\p\264.png -> D:\ZWally\DataSetWally\p\WallyPos166.png
    Renombrado: D:\ZWally\DataSetWally\p\265.png -> D:\ZWally\DataSetWally\p\WallyPos167.png
    Renombrado: D:\ZWally\DataSetWally\p\266.png -> D:\ZWally\DataSetWally\p\WallyPos168.png
    Renombrado: D:\ZWally\DataSetWally\p\267.png -> D:\ZWally\DataSetWally\p\WallyPos169.png
    Renombrado: D:\ZWally\DataSetWally\p\268.png -> D:\ZWally\DataSetWally\p\WallyPos170.png
    Renombrado: D:\ZWally\DataSetWally\p\269.png -> D:\ZWally\DataSetWally\p\WallyPos171.png
    Renombrado: D:\ZWally\DataSetWally\p\270.png -> D:\ZWally\DataSetWally\p\WallyPos172.png
    Renombrado: D:\ZWally\DataSetWally\p\272.png -> D:\ZWally\DataSetWally\p\WallyPos173.png
    Renombrado: D:\ZWally\DataSetWally\p\273.png -> D:\ZWally\DataSetWally\p\WallyPos174.png
    Renombrado: D:\ZWally\DataSetWally\p\274.png -> D:\ZWally\DataSetWally\p\WallyPos175.png
    Renombrado: D:\ZWally\DataSetWally\p\275.png -> D:\ZWally\DataSetWally\p\WallyPos176.png
    Renombrado: D:\ZWally\DataSetWally\p\276.png -> D:\ZWally\DataSetWally\p\WallyPos177.png
    Renombrado: D:\ZWally\DataSetWally\p\277.png -> D:\ZWally\DataSetWally\p\WallyPos178.png
    Renombrado: D:\ZWally\DataSetWally\p\278.png -> D:\ZWally\DataSetWally\p\WallyPos179.png
    Renombrado: D:\ZWally\DataSetWally\p\279.png -> D:\ZWally\DataSetWally\p\WallyPos180.png
    Renombrado: D:\ZWally\DataSetWally\p\28.png -> D:\ZWally\DataSetWally\p\WallyPos181.png
    Renombrado: D:\ZWally\DataSetWally\p\280.png -> D:\ZWally\DataSetWally\p\WallyPos182.png
    Renombrado: D:\ZWally\DataSetWally\p\281.png -> D:\ZWally\DataSetWally\p\WallyPos183.png
    Renombrado: D:\ZWally\DataSetWally\p\282.png -> D:\ZWally\DataSetWally\p\WallyPos184.png
    Renombrado: D:\ZWally\DataSetWally\p\284.png -> D:\ZWally\DataSetWally\p\WallyPos185.png
    Renombrado: D:\ZWally\DataSetWally\p\285.png -> D:\ZWally\DataSetWally\p\WallyPos186.png
    Renombrado: D:\ZWally\DataSetWally\p\286.png -> D:\ZWally\DataSetWally\p\WallyPos187.png
    Renombrado: D:\ZWally\DataSetWally\p\287.png -> D:\ZWally\DataSetWally\p\WallyPos188.png
    Renombrado: D:\ZWally\DataSetWally\p\288.png -> D:\ZWally\DataSetWally\p\WallyPos189.png
    Renombrado: D:\ZWally\DataSetWally\p\289.png -> D:\ZWally\DataSetWally\p\WallyPos190.png
    Renombrado: D:\ZWally\DataSetWally\p\29.png -> D:\ZWally\DataSetWally\p\WallyPos191.png
    Renombrado: D:\ZWally\DataSetWally\p\290.png -> D:\ZWally\DataSetWally\p\WallyPos192.png
    Renombrado: D:\ZWally\DataSetWally\p\291.png -> D:\ZWally\DataSetWally\p\WallyPos193.png
    Renombrado: D:\ZWally\DataSetWally\p\292.png -> D:\ZWally\DataSetWally\p\WallyPos194.png
    Renombrado: D:\ZWally\DataSetWally\p\293.png -> D:\ZWally\DataSetWally\p\WallyPos195.png
    Renombrado: D:\ZWally\DataSetWally\p\294.png -> D:\ZWally\DataSetWally\p\WallyPos196.png
    Renombrado: D:\ZWally\DataSetWally\p\296.png -> D:\ZWally\DataSetWally\p\WallyPos197.png
    Renombrado: D:\ZWally\DataSetWally\p\297.png -> D:\ZWally\DataSetWally\p\WallyPos198.png
    Renombrado: D:\ZWally\DataSetWally\p\298.png -> D:\ZWally\DataSetWally\p\WallyPos199.png
    Renombrado: D:\ZWally\DataSetWally\p\299.png -> D:\ZWally\DataSetWally\p\WallyPos200.png
    Renombrado: D:\ZWally\DataSetWally\p\30.png -> D:\ZWally\DataSetWally\p\WallyPos201.png
    Renombrado: D:\ZWally\DataSetWally\p\300.png -> D:\ZWally\DataSetWally\p\WallyPos202.png
    Renombrado: D:\ZWally\DataSetWally\p\301.png -> D:\ZWally\DataSetWally\p\WallyPos203.png
    Renombrado: D:\ZWally\DataSetWally\p\302.png -> D:\ZWally\DataSetWally\p\WallyPos204.png
    Renombrado: D:\ZWally\DataSetWally\p\303.png -> D:\ZWally\DataSetWally\p\WallyPos205.png
    Renombrado: D:\ZWally\DataSetWally\p\304.png -> D:\ZWally\DataSetWally\p\WallyPos206.png
    Renombrado: D:\ZWally\DataSetWally\p\305.png -> D:\ZWally\DataSetWally\p\WallyPos207.png
    Renombrado: D:\ZWally\DataSetWally\p\306.png -> D:\ZWally\DataSetWally\p\WallyPos208.png
    Renombrado: D:\ZWally\DataSetWally\p\308.png -> D:\ZWally\DataSetWally\p\WallyPos209.png
    Renombrado: D:\ZWally\DataSetWally\p\309.png -> D:\ZWally\DataSetWally\p\WallyPos210.png
    Renombrado: D:\ZWally\DataSetWally\p\31.png -> D:\ZWally\DataSetWally\p\WallyPos211.png
    Renombrado: D:\ZWally\DataSetWally\p\310.png -> D:\ZWally\DataSetWally\p\WallyPos212.png
    Renombrado: D:\ZWally\DataSetWally\p\311.png -> D:\ZWally\DataSetWally\p\WallyPos213.png
    Renombrado: D:\ZWally\DataSetWally\p\312.png -> D:\ZWally\DataSetWally\p\WallyPos214.png
    Renombrado: D:\ZWally\DataSetWally\p\313.png -> D:\ZWally\DataSetWally\p\WallyPos215.png
    Renombrado: D:\ZWally\DataSetWally\p\314.png -> D:\ZWally\DataSetWally\p\WallyPos216.png
    Renombrado: D:\ZWally\DataSetWally\p\315.png -> D:\ZWally\DataSetWally\p\WallyPos217.png
    Renombrado: D:\ZWally\DataSetWally\p\316.png -> D:\ZWally\DataSetWally\p\WallyPos218.png
    Renombrado: D:\ZWally\DataSetWally\p\317.png -> D:\ZWally\DataSetWally\p\WallyPos219.png
    Renombrado: D:\ZWally\DataSetWally\p\318.png -> D:\ZWally\DataSetWally\p\WallyPos220.png
    Renombrado: D:\ZWally\DataSetWally\p\32.png -> D:\ZWally\DataSetWally\p\WallyPos221.png
    Renombrado: D:\ZWally\DataSetWally\p\320.png -> D:\ZWally\DataSetWally\p\WallyPos222.png
    Renombrado: D:\ZWally\DataSetWally\p\321.png -> D:\ZWally\DataSetWally\p\WallyPos223.png
    Renombrado: D:\ZWally\DataSetWally\p\322.png -> D:\ZWally\DataSetWally\p\WallyPos224.png
    Renombrado: D:\ZWally\DataSetWally\p\323.png -> D:\ZWally\DataSetWally\p\WallyPos225.png
    Renombrado: D:\ZWally\DataSetWally\p\324.png -> D:\ZWally\DataSetWally\p\WallyPos226.png
    Renombrado: D:\ZWally\DataSetWally\p\325.png -> D:\ZWally\DataSetWally\p\WallyPos227.png
    Renombrado: D:\ZWally\DataSetWally\p\326.png -> D:\ZWally\DataSetWally\p\WallyPos228.png
    Renombrado: D:\ZWally\DataSetWally\p\327.png -> D:\ZWally\DataSetWally\p\WallyPos229.png
    Renombrado: D:\ZWally\DataSetWally\p\328.png -> D:\ZWally\DataSetWally\p\WallyPos230.png
    Renombrado: D:\ZWally\DataSetWally\p\329.png -> D:\ZWally\DataSetWally\p\WallyPos231.png
    Renombrado: D:\ZWally\DataSetWally\p\33.png -> D:\ZWally\DataSetWally\p\WallyPos232.png
    Renombrado: D:\ZWally\DataSetWally\p\330.png -> D:\ZWally\DataSetWally\p\WallyPos233.png
    Renombrado: D:\ZWally\DataSetWally\p\332.png -> D:\ZWally\DataSetWally\p\WallyPos234.png
    Renombrado: D:\ZWally\DataSetWally\p\333.png -> D:\ZWally\DataSetWally\p\WallyPos235.png
    Renombrado: D:\ZWally\DataSetWally\p\334.png -> D:\ZWally\DataSetWally\p\WallyPos236.png
    Renombrado: D:\ZWally\DataSetWally\p\335.png -> D:\ZWally\DataSetWally\p\WallyPos237.png
    Renombrado: D:\ZWally\DataSetWally\p\336.png -> D:\ZWally\DataSetWally\p\WallyPos238.png
    Renombrado: D:\ZWally\DataSetWally\p\337.png -> D:\ZWally\DataSetWally\p\WallyPos239.png
    Renombrado: D:\ZWally\DataSetWally\p\338.png -> D:\ZWally\DataSetWally\p\WallyPos240.png
    Renombrado: D:\ZWally\DataSetWally\p\339.png -> D:\ZWally\DataSetWally\p\WallyPos241.png
    Renombrado: D:\ZWally\DataSetWally\p\34.png -> D:\ZWally\DataSetWally\p\WallyPos242.png
    Renombrado: D:\ZWally\DataSetWally\p\340.png -> D:\ZWally\DataSetWally\p\WallyPos243.png
    Renombrado: D:\ZWally\DataSetWally\p\341.png -> D:\ZWally\DataSetWally\p\WallyPos244.png
    Renombrado: D:\ZWally\DataSetWally\p\342.png -> D:\ZWally\DataSetWally\p\WallyPos245.png
    Renombrado: D:\ZWally\DataSetWally\p\344.png -> D:\ZWally\DataSetWally\p\WallyPos246.png
    Renombrado: D:\ZWally\DataSetWally\p\345.png -> D:\ZWally\DataSetWally\p\WallyPos247.png
    Renombrado: D:\ZWally\DataSetWally\p\346.png -> D:\ZWally\DataSetWally\p\WallyPos248.png
    Renombrado: D:\ZWally\DataSetWally\p\347.png -> D:\ZWally\DataSetWally\p\WallyPos249.png
    Renombrado: D:\ZWally\DataSetWally\p\348.png -> D:\ZWally\DataSetWally\p\WallyPos250.png
    Renombrado: D:\ZWally\DataSetWally\p\349.png -> D:\ZWally\DataSetWally\p\WallyPos251.png
    Renombrado: D:\ZWally\DataSetWally\p\35.png -> D:\ZWally\DataSetWally\p\WallyPos252.png
    Renombrado: D:\ZWally\DataSetWally\p\350.png -> D:\ZWally\DataSetWally\p\WallyPos253.png
    Renombrado: D:\ZWally\DataSetWally\p\351.png -> D:\ZWally\DataSetWally\p\WallyPos254.png
    Renombrado: D:\ZWally\DataSetWally\p\352.png -> D:\ZWally\DataSetWally\p\WallyPos255.png
    Renombrado: D:\ZWally\DataSetWally\p\353.png -> D:\ZWally\DataSetWally\p\WallyPos256.png
    Renombrado: D:\ZWally\DataSetWally\p\354.png -> D:\ZWally\DataSetWally\p\WallyPos257.png
    Renombrado: D:\ZWally\DataSetWally\p\356.png -> D:\ZWally\DataSetWally\p\WallyPos258.png
    Renombrado: D:\ZWally\DataSetWally\p\357.png -> D:\ZWally\DataSetWally\p\WallyPos259.png
    Renombrado: D:\ZWally\DataSetWally\p\358.png -> D:\ZWally\DataSetWally\p\WallyPos260.png
    Renombrado: D:\ZWally\DataSetWally\p\359.png -> D:\ZWally\DataSetWally\p\WallyPos261.png
    Renombrado: D:\ZWally\DataSetWally\p\36.png -> D:\ZWally\DataSetWally\p\WallyPos262.png
    Renombrado: D:\ZWally\DataSetWally\p\360.png -> D:\ZWally\DataSetWally\p\WallyPos263.png
    Renombrado: D:\ZWally\DataSetWally\p\361.png -> D:\ZWally\DataSetWally\p\WallyPos264.png
    Renombrado: D:\ZWally\DataSetWally\p\362.png -> D:\ZWally\DataSetWally\p\WallyPos265.png
    Renombrado: D:\ZWally\DataSetWally\p\363.png -> D:\ZWally\DataSetWally\p\WallyPos266.png
    Renombrado: D:\ZWally\DataSetWally\p\364.png -> D:\ZWally\DataSetWally\p\WallyPos267.png
    Renombrado: D:\ZWally\DataSetWally\p\365.png -> D:\ZWally\DataSetWally\p\WallyPos268.png
    Renombrado: D:\ZWally\DataSetWally\p\366.png -> D:\ZWally\DataSetWally\p\WallyPos269.png
    Renombrado: D:\ZWally\DataSetWally\p\368.png -> D:\ZWally\DataSetWally\p\WallyPos270.png
    Renombrado: D:\ZWally\DataSetWally\p\37.png -> D:\ZWally\DataSetWally\p\WallyPos271.png
    Renombrado: D:\ZWally\DataSetWally\p\370.png -> D:\ZWally\DataSetWally\p\WallyPos272.png
    Renombrado: D:\ZWally\DataSetWally\p\371.png -> D:\ZWally\DataSetWally\p\WallyPos273.png
    Renombrado: D:\ZWally\DataSetWally\p\372.png -> D:\ZWally\DataSetWally\p\WallyPos274.png
    Renombrado: D:\ZWally\DataSetWally\p\373.png -> D:\ZWally\DataSetWally\p\WallyPos275.png
    Renombrado: D:\ZWally\DataSetWally\p\374.png -> D:\ZWally\DataSetWally\p\WallyPos276.png
    Renombrado: D:\ZWally\DataSetWally\p\375.png -> D:\ZWally\DataSetWally\p\WallyPos277.png
    Renombrado: D:\ZWally\DataSetWally\p\376.png -> D:\ZWally\DataSetWally\p\WallyPos278.png
    Renombrado: D:\ZWally\DataSetWally\p\377.png -> D:\ZWally\DataSetWally\p\WallyPos279.png
    Renombrado: D:\ZWally\DataSetWally\p\378.png -> D:\ZWally\DataSetWally\p\WallyPos280.png
    Renombrado: D:\ZWally\DataSetWally\p\379.png -> D:\ZWally\DataSetWally\p\WallyPos281.png
    Renombrado: D:\ZWally\DataSetWally\p\38.png -> D:\ZWally\DataSetWally\p\WallyPos282.png
    Renombrado: D:\ZWally\DataSetWally\p\380.png -> D:\ZWally\DataSetWally\p\WallyPos283.png
    Renombrado: D:\ZWally\DataSetWally\p\382.png -> D:\ZWally\DataSetWally\p\WallyPos284.png
    Renombrado: D:\ZWally\DataSetWally\p\383.png -> D:\ZWally\DataSetWally\p\WallyPos285.png
    Renombrado: D:\ZWally\DataSetWally\p\385.png -> D:\ZWally\DataSetWally\p\WallyPos286.png
    Renombrado: D:\ZWally\DataSetWally\p\387.png -> D:\ZWally\DataSetWally\p\WallyPos287.png
    Renombrado: D:\ZWally\DataSetWally\p\389.png -> D:\ZWally\DataSetWally\p\WallyPos288.png
    Renombrado: D:\ZWally\DataSetWally\p\391.png -> D:\ZWally\DataSetWally\p\WallyPos289.png
    Renombrado: D:\ZWally\DataSetWally\p\393.png -> D:\ZWally\DataSetWally\p\WallyPos290.png
    Renombrado: D:\ZWally\DataSetWally\p\395.png -> D:\ZWally\DataSetWally\p\WallyPos291.png
    Renombrado: D:\ZWally\DataSetWally\p\397.png -> D:\ZWally\DataSetWally\p\WallyPos292.png
    Renombrado: D:\ZWally\DataSetWally\p\399.png -> D:\ZWally\DataSetWally\p\WallyPos293.png
    Renombrado: D:\ZWally\DataSetWally\p\4.png -> D:\ZWally\DataSetWally\p\WallyPos294.png
    Renombrado: D:\ZWally\DataSetWally\p\40.png -> D:\ZWally\DataSetWally\p\WallyPos295.png
    Renombrado: D:\ZWally\DataSetWally\p\401.png -> D:\ZWally\DataSetWally\p\WallyPos296.png
    Renombrado: D:\ZWally\DataSetWally\p\403.png -> D:\ZWally\DataSetWally\p\WallyPos297.png
    Renombrado: D:\ZWally\DataSetWally\p\405.png -> D:\ZWally\DataSetWally\p\WallyPos298.png
    Renombrado: D:\ZWally\DataSetWally\p\407.png -> D:\ZWally\DataSetWally\p\WallyPos299.png
    Renombrado: D:\ZWally\DataSetWally\p\409.png -> D:\ZWally\DataSetWally\p\WallyPos300.png
    Renombrado: D:\ZWally\DataSetWally\p\41.png -> D:\ZWally\DataSetWally\p\WallyPos301.png
    Renombrado: D:\ZWally\DataSetWally\p\411.png -> D:\ZWally\DataSetWally\p\WallyPos302.png
    Renombrado: D:\ZWally\DataSetWally\p\413.png -> D:\ZWally\DataSetWally\p\WallyPos303.png
    Renombrado: D:\ZWally\DataSetWally\p\415.png -> D:\ZWally\DataSetWally\p\WallyPos304.png
    Renombrado: D:\ZWally\DataSetWally\p\417.png -> D:\ZWally\DataSetWally\p\WallyPos305.png
    Renombrado: D:\ZWally\DataSetWally\p\419.png -> D:\ZWally\DataSetWally\p\WallyPos306.png
    Renombrado: D:\ZWally\DataSetWally\p\42.png -> D:\ZWally\DataSetWally\p\WallyPos307.png
    Renombrado: D:\ZWally\DataSetWally\p\421.png -> D:\ZWally\DataSetWally\p\WallyPos308.png
    Renombrado: D:\ZWally\DataSetWally\p\423.png -> D:\ZWally\DataSetWally\p\WallyPos309.png
    Renombrado: D:\ZWally\DataSetWally\p\425.png -> D:\ZWally\DataSetWally\p\WallyPos310.png
    Renombrado: D:\ZWally\DataSetWally\p\427.png -> D:\ZWally\DataSetWally\p\WallyPos311.png
    Renombrado: D:\ZWally\DataSetWally\p\429.png -> D:\ZWally\DataSetWally\p\WallyPos312.png
    Renombrado: D:\ZWally\DataSetWally\p\43.png -> D:\ZWally\DataSetWally\p\WallyPos313.png
    Renombrado: D:\ZWally\DataSetWally\p\431.png -> D:\ZWally\DataSetWally\p\WallyPos314.png
    Renombrado: D:\ZWally\DataSetWally\p\433.png -> D:\ZWally\DataSetWally\p\WallyPos315.png
    Renombrado: D:\ZWally\DataSetWally\p\435.png -> D:\ZWally\DataSetWally\p\WallyPos316.png
    Renombrado: D:\ZWally\DataSetWally\p\436.png -> D:\ZWally\DataSetWally\p\WallyPos317.png
    Renombrado: D:\ZWally\DataSetWally\p\437.png -> D:\ZWally\DataSetWally\p\WallyPos318.png
    Renombrado: D:\ZWally\DataSetWally\p\438.png -> D:\ZWally\DataSetWally\p\WallyPos319.png
    Renombrado: D:\ZWally\DataSetWally\p\439.png -> D:\ZWally\DataSetWally\p\WallyPos320.png
    Renombrado: D:\ZWally\DataSetWally\p\44.png -> D:\ZWally\DataSetWally\p\WallyPos321.png
    Renombrado: D:\ZWally\DataSetWally\p\441.png -> D:\ZWally\DataSetWally\p\WallyPos322.png
    Renombrado: D:\ZWally\DataSetWally\p\442.png -> D:\ZWally\DataSetWally\p\WallyPos323.png
    Renombrado: D:\ZWally\DataSetWally\p\443.png -> D:\ZWally\DataSetWally\p\WallyPos324.png
    Renombrado: D:\ZWally\DataSetWally\p\444.png -> D:\ZWally\DataSetWally\p\WallyPos325.png
    Renombrado: D:\ZWally\DataSetWally\p\445.png -> D:\ZWally\DataSetWally\p\WallyPos326.png
    Renombrado: D:\ZWally\DataSetWally\p\446.png -> D:\ZWally\DataSetWally\p\WallyPos327.png
    Renombrado: D:\ZWally\DataSetWally\p\447.png -> D:\ZWally\DataSetWally\p\WallyPos328.png
    Renombrado: D:\ZWally\DataSetWally\p\448.png -> D:\ZWally\DataSetWally\p\WallyPos329.png
    Renombrado: D:\ZWally\DataSetWally\p\449.png -> D:\ZWally\DataSetWally\p\WallyPos330.png
    Renombrado: D:\ZWally\DataSetWally\p\45.png -> D:\ZWally\DataSetWally\p\WallyPos331.png
    Renombrado: D:\ZWally\DataSetWally\p\450.png -> D:\ZWally\DataSetWally\p\WallyPos332.png
    Renombrado: D:\ZWally\DataSetWally\p\451.png -> D:\ZWally\DataSetWally\p\WallyPos333.png
    Renombrado: D:\ZWally\DataSetWally\p\453.png -> D:\ZWally\DataSetWally\p\WallyPos334.png
    Renombrado: D:\ZWally\DataSetWally\p\454.png -> D:\ZWally\DataSetWally\p\WallyPos335.png
    Renombrado: D:\ZWally\DataSetWally\p\455.png -> D:\ZWally\DataSetWally\p\WallyPos336.png
    Renombrado: D:\ZWally\DataSetWally\p\456.png -> D:\ZWally\DataSetWally\p\WallyPos337.png
    Renombrado: D:\ZWally\DataSetWally\p\457.png -> D:\ZWally\DataSetWally\p\WallyPos338.png
    Renombrado: D:\ZWally\DataSetWally\p\458.png -> D:\ZWally\DataSetWally\p\WallyPos339.png
    Renombrado: D:\ZWally\DataSetWally\p\459.png -> D:\ZWally\DataSetWally\p\WallyPos340.png
    Renombrado: D:\ZWally\DataSetWally\p\46.png -> D:\ZWally\DataSetWally\p\WallyPos341.png
    Renombrado: D:\ZWally\DataSetWally\p\460.png -> D:\ZWally\DataSetWally\p\WallyPos342.png
    Renombrado: D:\ZWally\DataSetWally\p\461.png -> D:\ZWally\DataSetWally\p\WallyPos343.png
    Renombrado: D:\ZWally\DataSetWally\p\462.png -> D:\ZWally\DataSetWally\p\WallyPos344.png
    Renombrado: D:\ZWally\DataSetWally\p\463.png -> D:\ZWally\DataSetWally\p\WallyPos345.png
    Renombrado: D:\ZWally\DataSetWally\p\465.png -> D:\ZWally\DataSetWally\p\WallyPos346.png
    Renombrado: D:\ZWally\DataSetWally\p\466.png -> D:\ZWally\DataSetWally\p\WallyPos347.png
    Renombrado: D:\ZWally\DataSetWally\p\467.png -> D:\ZWally\DataSetWally\p\WallyPos348.png
    Renombrado: D:\ZWally\DataSetWally\p\468.png -> D:\ZWally\DataSetWally\p\WallyPos349.png
    Renombrado: D:\ZWally\DataSetWally\p\469.png -> D:\ZWally\DataSetWally\p\WallyPos350.png
    Renombrado: D:\ZWally\DataSetWally\p\47.png -> D:\ZWally\DataSetWally\p\WallyPos351.png
    Renombrado: D:\ZWally\DataSetWally\p\470.png -> D:\ZWally\DataSetWally\p\WallyPos352.png
    Renombrado: D:\ZWally\DataSetWally\p\471.png -> D:\ZWally\DataSetWally\p\WallyPos353.png
    Renombrado: D:\ZWally\DataSetWally\p\472.png -> D:\ZWally\DataSetWally\p\WallyPos354.png
    Renombrado: D:\ZWally\DataSetWally\p\473.png -> D:\ZWally\DataSetWally\p\WallyPos355.png
    Renombrado: D:\ZWally\DataSetWally\p\474.png -> D:\ZWally\DataSetWally\p\WallyPos356.png
    Renombrado: D:\ZWally\DataSetWally\p\475.png -> D:\ZWally\DataSetWally\p\WallyPos357.png
    Renombrado: D:\ZWally\DataSetWally\p\476.png -> D:\ZWally\DataSetWally\p\WallyPos358.png
    Renombrado: D:\ZWally\DataSetWally\p\477.png -> D:\ZWally\DataSetWally\p\WallyPos359.png
    Renombrado: D:\ZWally\DataSetWally\p\48.png -> D:\ZWally\DataSetWally\p\WallyPos360.png
    Renombrado: D:\ZWally\DataSetWally\p\49.png -> D:\ZWally\DataSetWally\p\WallyPos361.png
    Renombrado: D:\ZWally\DataSetWally\p\5.png -> D:\ZWally\DataSetWally\p\WallyPos362.png
    Renombrado: D:\ZWally\DataSetWally\p\50.png -> D:\ZWally\DataSetWally\p\WallyPos363.png
    Renombrado: D:\ZWally\DataSetWally\p\52.png -> D:\ZWally\DataSetWally\p\WallyPos364.png
    Renombrado: D:\ZWally\DataSetWally\p\53.png -> D:\ZWally\DataSetWally\p\WallyPos365.png
    Renombrado: D:\ZWally\DataSetWally\p\54.png -> D:\ZWally\DataSetWally\p\WallyPos366.png
    Renombrado: D:\ZWally\DataSetWally\p\55.png -> D:\ZWally\DataSetWally\p\WallyPos367.png
    Renombrado: D:\ZWally\DataSetWally\p\56.png -> D:\ZWally\DataSetWally\p\WallyPos368.png
    Renombrado: D:\ZWally\DataSetWally\p\57.png -> D:\ZWally\DataSetWally\p\WallyPos369.png
    Renombrado: D:\ZWally\DataSetWally\p\58.png -> D:\ZWally\DataSetWally\p\WallyPos370.png
    Renombrado: D:\ZWally\DataSetWally\p\59.png -> D:\ZWally\DataSetWally\p\WallyPos371.png
    Renombrado: D:\ZWally\DataSetWally\p\6.png -> D:\ZWally\DataSetWally\p\WallyPos372.png
    Renombrado: D:\ZWally\DataSetWally\p\60.png -> D:\ZWally\DataSetWally\p\WallyPos373.png
    Renombrado: D:\ZWally\DataSetWally\p\61.png -> D:\ZWally\DataSetWally\p\WallyPos374.png
    Renombrado: D:\ZWally\DataSetWally\p\62.png -> D:\ZWally\DataSetWally\p\WallyPos375.png
    Renombrado: D:\ZWally\DataSetWally\p\64.png -> D:\ZWally\DataSetWally\p\WallyPos376.png
    Renombrado: D:\ZWally\DataSetWally\p\65.png -> D:\ZWally\DataSetWally\p\WallyPos377.png
    Renombrado: D:\ZWally\DataSetWally\p\66.png -> D:\ZWally\DataSetWally\p\WallyPos378.png
    Renombrado: D:\ZWally\DataSetWally\p\67.png -> D:\ZWally\DataSetWally\p\WallyPos379.png
    Renombrado: D:\ZWally\DataSetWally\p\68.png -> D:\ZWally\DataSetWally\p\WallyPos380.png
    Renombrado: D:\ZWally\DataSetWally\p\69.png -> D:\ZWally\DataSetWally\p\WallyPos381.png
    Renombrado: D:\ZWally\DataSetWally\p\7.png -> D:\ZWally\DataSetWally\p\WallyPos382.png
    Renombrado: D:\ZWally\DataSetWally\p\70.png -> D:\ZWally\DataSetWally\p\WallyPos383.png
    Renombrado: D:\ZWally\DataSetWally\p\71.png -> D:\ZWally\DataSetWally\p\WallyPos384.png
    Renombrado: D:\ZWally\DataSetWally\p\72.png -> D:\ZWally\DataSetWally\p\WallyPos385.png
    Renombrado: D:\ZWally\DataSetWally\p\73.png -> D:\ZWally\DataSetWally\p\WallyPos386.png
    Renombrado: D:\ZWally\DataSetWally\p\74.png -> D:\ZWally\DataSetWally\p\WallyPos387.png
    Renombrado: D:\ZWally\DataSetWally\p\76.png -> D:\ZWally\DataSetWally\p\WallyPos388.png
    Renombrado: D:\ZWally\DataSetWally\p\77.png -> D:\ZWally\DataSetWally\p\WallyPos389.png
    Renombrado: D:\ZWally\DataSetWally\p\78.png -> D:\ZWally\DataSetWally\p\WallyPos390.png
    Renombrado: D:\ZWally\DataSetWally\p\79.png -> D:\ZWally\DataSetWally\p\WallyPos391.png
    Renombrado: D:\ZWally\DataSetWally\p\8.png -> D:\ZWally\DataSetWally\p\WallyPos392.png
    Renombrado: D:\ZWally\DataSetWally\p\80.png -> D:\ZWally\DataSetWally\p\WallyPos393.png
    Renombrado: D:\ZWally\DataSetWally\p\81.png -> D:\ZWally\DataSetWally\p\WallyPos394.png
    Renombrado: D:\ZWally\DataSetWally\p\82.png -> D:\ZWally\DataSetWally\p\WallyPos395.png
    Renombrado: D:\ZWally\DataSetWally\p\83.png -> D:\ZWally\DataSetWally\p\WallyPos396.png
    Renombrado: D:\ZWally\DataSetWally\p\84.png -> D:\ZWally\DataSetWally\p\WallyPos397.png
    Renombrado: D:\ZWally\DataSetWally\p\85.png -> D:\ZWally\DataSetWally\p\WallyPos398.png
    Renombrado: D:\ZWally\DataSetWally\p\86.png -> D:\ZWally\DataSetWally\p\WallyPos399.png
    Renombrado: D:\ZWally\DataSetWally\p\88.png -> D:\ZWally\DataSetWally\p\WallyPos400.png
    Renombrado: D:\ZWally\DataSetWally\p\89.png -> D:\ZWally\DataSetWally\p\WallyPos401.png
    Renombrado: D:\ZWally\DataSetWally\p\9.png -> D:\ZWally\DataSetWally\p\WallyPos402.png
    Renombrado: D:\ZWally\DataSetWally\p\90.png -> D:\ZWally\DataSetWally\p\WallyPos403.png
    Renombrado: D:\ZWally\DataSetWally\p\91.png -> D:\ZWally\DataSetWally\p\WallyPos404.png
    Renombrado: D:\ZWally\DataSetWally\p\92.png -> D:\ZWally\DataSetWally\p\WallyPos405.png
    Renombrado: D:\ZWally\DataSetWally\p\93.png -> D:\ZWally\DataSetWally\p\WallyPos406.png
    Renombrado: D:\ZWally\DataSetWally\p\94.png -> D:\ZWally\DataSetWally\p\WallyPos407.png
    Renombrado: D:\ZWally\DataSetWally\p\95.png -> D:\ZWally\DataSetWally\p\WallyPos408.png
    Renombrado: D:\ZWally\DataSetWally\p\96.png -> D:\ZWally\DataSetWally\p\WallyPos409.png
    Renombrado: D:\ZWally\DataSetWally\p\97.png -> D:\ZWally\DataSetWally\p\WallyPos410.png
    Renombrado: D:\ZWally\DataSetWally\p\98.png -> D:\ZWally\DataSetWally\p\WallyPos411.png
    Renombrado: D:\ZWally\DataSetWally\p\Captura de pantalla 2024-05-27 031909.png -> D:\ZWally\DataSetWally\p\WallyPos412.png
    Renombrado: D:\ZWally\DataSetWally\p\Captura de pantalla 2024-05-27 032105.png -> D:\ZWally\DataSetWally\p\WallyPos413.png
    Renombrado: D:\ZWally\DataSetWally\p\Captura de pantalla 2024-05-27 032210.png -> D:\ZWally\DataSetWally\p\WallyPos414.png
    Renombrado: D:\ZWally\DataSetWally\p\Captura de pantalla 2024-05-27 032237.png -> D:\ZWally\DataSetWally\p\WallyPos415.png
    Renombrado: D:\ZWally\DataSetWally\p\Captura de pantalla 2024-05-27 032315.png -> D:\ZWally\DataSetWally\p\WallyPos416.png
    Renombrado: D:\ZWally\DataSetWally\p\Captura de pantalla 2024-05-27 032341.png -> D:\ZWally\DataSetWally\p\WallyPos417.png
    Renombrado: D:\ZWally\DataSetWally\p\Captura de pantalla 2024-05-27 032629.png -> D:\ZWally\DataSetWally\p\WallyPos418.png
    Renombrado: D:\ZWally\DataSetWally\p\Captura de pantalla 2024-05-27 032742.png -> D:\ZWally\DataSetWally\p\WallyPos419.png
    Renombrado: D:\ZWally\DataSetWally\p\Captura de pantalla 2024-05-27 032802.png -> D:\ZWally\DataSetWally\p\WallyPos420.png
    Renombrado: D:\ZWally\DataSetWally\p\Captura de pantalla 2024-05-27 032809.png -> D:\ZWally\DataSetWally\p\WallyPos421.png
    Renombrado: D:\ZWally\DataSetWally\p\Captura de pantalla 2024-05-27 032816.png -> D:\ZWally\DataSetWally\p\WallyPos422.png
    Renombrado: D:\ZWally\DataSetWally\p\Captura de pantalla 2024-05-27 032831.png -> D:\ZWally\DataSetWally\p\WallyPos423.png
    Renombrado: D:\ZWally\DataSetWally\p\Captura de pantalla 2024-05-27 032936.png -> D:\ZWally\DataSetWally\p\WallyPos424.png
    Renombrado: D:\ZWally\DataSetWally\p\1.jpg -> D:\ZWally\DataSetWally\p\WallyPos425.png
    Renombrado: D:\ZWally\DataSetWally\p\111.jpg -> D:\ZWally\DataSetWally\p\WallyPos426.png
    Renombrado: D:\ZWally\DataSetWally\p\123.jpg -> D:\ZWally\DataSetWally\p\WallyPos427.png
    Renombrado: D:\ZWally\DataSetWally\p\125.jpg -> D:\ZWally\DataSetWally\p\WallyPos428.png
    Renombrado: D:\ZWally\DataSetWally\p\137.jpg -> D:\ZWally\DataSetWally\p\WallyPos429.png
    Renombrado: D:\ZWally\DataSetWally\p\149.jpg -> D:\ZWally\DataSetWally\p\WallyPos430.png
    Renombrado: D:\ZWally\DataSetWally\p\15.jpg -> D:\ZWally\DataSetWally\p\WallyPos431.png
    Renombrado: D:\ZWally\DataSetWally\p\161.jpg -> D:\ZWally\DataSetWally\p\WallyPos432.png
    Renombrado: D:\ZWally\DataSetWally\p\173.jpg -> D:\ZWally\DataSetWally\p\WallyPos433.png
    Renombrado: D:\ZWally\DataSetWally\p\185.jpg -> D:\ZWally\DataSetWally\p\WallyPos434.png
    Renombrado: D:\ZWally\DataSetWally\p\197.jpg -> D:\ZWally\DataSetWally\p\WallyPos435.png
    Renombrado: D:\ZWally\DataSetWally\p\209.jpg -> D:\ZWally\DataSetWally\p\WallyPos436.png
    Renombrado: D:\ZWally\DataSetWally\p\221.jpg -> D:\ZWally\DataSetWally\p\WallyPos437.png
    Renombrado: D:\ZWally\DataSetWally\p\233.jpg -> D:\ZWally\DataSetWally\p\WallyPos438.png
    Renombrado: D:\ZWally\DataSetWally\p\245.jpg -> D:\ZWally\DataSetWally\p\WallyPos439.png
    Renombrado: D:\ZWally\DataSetWally\p\247.jpg -> D:\ZWally\DataSetWally\p\WallyPos440.png
    Renombrado: D:\ZWally\DataSetWally\p\259.jpg -> D:\ZWally\DataSetWally\p\WallyPos441.png
    Renombrado: D:\ZWally\DataSetWally\p\27.jpg -> D:\ZWally\DataSetWally\p\WallyPos442.png
    Renombrado: D:\ZWally\DataSetWally\p\271.jpg -> D:\ZWally\DataSetWally\p\WallyPos443.png
    Renombrado: D:\ZWally\DataSetWally\p\283.jpg -> D:\ZWally\DataSetWally\p\WallyPos444.png
    Renombrado: D:\ZWally\DataSetWally\p\295.jpg -> D:\ZWally\DataSetWally\p\WallyPos445.png
    Renombrado: D:\ZWally\DataSetWally\p\307.jpg -> D:\ZWally\DataSetWally\p\WallyPos446.png
    Renombrado: D:\ZWally\DataSetWally\p\319.jpg -> D:\ZWally\DataSetWally\p\WallyPos447.png
    Renombrado: D:\ZWally\DataSetWally\p\331.jpg -> D:\ZWally\DataSetWally\p\WallyPos448.png
    Renombrado: D:\ZWally\DataSetWally\p\343.jpg -> D:\ZWally\DataSetWally\p\WallyPos449.png
    Renombrado: D:\ZWally\DataSetWally\p\355.jpg -> D:\ZWally\DataSetWally\p\WallyPos450.png
    Renombrado: D:\ZWally\DataSetWally\p\367.jpg -> D:\ZWally\DataSetWally\p\WallyPos451.png
    Renombrado: D:\ZWally\DataSetWally\p\369.jpg -> D:\ZWally\DataSetWally\p\WallyPos452.png
    Renombrado: D:\ZWally\DataSetWally\p\384.jpg -> D:\ZWally\DataSetWally\p\WallyPos453.png
    Renombrado: D:\ZWally\DataSetWally\p\386.jpg -> D:\ZWally\DataSetWally\p\WallyPos454.png
    Renombrado: D:\ZWally\DataSetWally\p\388.jpg -> D:\ZWally\DataSetWally\p\WallyPos455.png
    Renombrado: D:\ZWally\DataSetWally\p\39.jpg -> D:\ZWally\DataSetWally\p\WallyPos456.png
    Renombrado: D:\ZWally\DataSetWally\p\390.jpg -> D:\ZWally\DataSetWally\p\WallyPos457.png
    Renombrado: D:\ZWally\DataSetWally\p\392.jpg -> D:\ZWally\DataSetWally\p\WallyPos458.png
    Renombrado: D:\ZWally\DataSetWally\p\394.jpg -> D:\ZWally\DataSetWally\p\WallyPos459.png
    Renombrado: D:\ZWally\DataSetWally\p\396.jpg -> D:\ZWally\DataSetWally\p\WallyPos460.png
    Renombrado: D:\ZWally\DataSetWally\p\398.jpg -> D:\ZWally\DataSetWally\p\WallyPos461.png
    Renombrado: D:\ZWally\DataSetWally\p\400.jpg -> D:\ZWally\DataSetWally\p\WallyPos462.png
    Renombrado: D:\ZWally\DataSetWally\p\402.jpg -> D:\ZWally\DataSetWally\p\WallyPos463.png
    Renombrado: D:\ZWally\DataSetWally\p\404.jpg -> D:\ZWally\DataSetWally\p\WallyPos464.png
    Renombrado: D:\ZWally\DataSetWally\p\406.jpg -> D:\ZWally\DataSetWally\p\WallyPos465.png
    Renombrado: D:\ZWally\DataSetWally\p\408.jpg -> D:\ZWally\DataSetWally\p\WallyPos466.png
    Renombrado: D:\ZWally\DataSetWally\p\410.jpg -> D:\ZWally\DataSetWally\p\WallyPos467.png
    Renombrado: D:\ZWally\DataSetWally\p\412.jpg -> D:\ZWally\DataSetWally\p\WallyPos468.png
    Renombrado: D:\ZWally\DataSetWally\p\414.jpg -> D:\ZWally\DataSetWally\p\WallyPos469.png
    Renombrado: D:\ZWally\DataSetWally\p\416.jpg -> D:\ZWally\DataSetWally\p\WallyPos470.png
    Renombrado: D:\ZWally\DataSetWally\p\418.jpg -> D:\ZWally\DataSetWally\p\WallyPos471.png
    Renombrado: D:\ZWally\DataSetWally\p\420.jpg -> D:\ZWally\DataSetWally\p\WallyPos472.png
    Renombrado: D:\ZWally\DataSetWally\p\422.jpg -> D:\ZWally\DataSetWally\p\WallyPos473.png
    Renombrado: D:\ZWally\DataSetWally\p\424.jpg -> D:\ZWally\DataSetWally\p\WallyPos474.png
    Renombrado: D:\ZWally\DataSetWally\p\426.jpg -> D:\ZWally\DataSetWally\p\WallyPos475.png
    Renombrado: D:\ZWally\DataSetWally\p\428.jpg -> D:\ZWally\DataSetWally\p\WallyPos476.png
    Renombrado: D:\ZWally\DataSetWally\p\430.jpg -> D:\ZWally\DataSetWally\p\WallyPos477.png
    Renombrado: D:\ZWally\DataSetWally\p\432.jpg -> D:\ZWally\DataSetWally\p\WallyPos478.png
    Renombrado: D:\ZWally\DataSetWally\p\434.jpg -> D:\ZWally\DataSetWally\p\WallyPos479.png
    Renombrado: D:\ZWally\DataSetWally\p\440.jpg -> D:\ZWally\DataSetWally\p\WallyPos480.png
    Renombrado: D:\ZWally\DataSetWally\p\452.jpg -> D:\ZWally\DataSetWally\p\WallyPos481.png
    Renombrado: D:\ZWally\DataSetWally\p\464.jpg -> D:\ZWally\DataSetWally\p\WallyPos482.png
    Renombrado: D:\ZWally\DataSetWally\p\51.jpg -> D:\ZWally\DataSetWally\p\WallyPos483.png
    Renombrado: D:\ZWally\DataSetWally\p\63.jpg -> D:\ZWally\DataSetWally\p\WallyPos484.png
    Renombrado: D:\ZWally\DataSetWally\p\75.jpg -> D:\ZWally\DataSetWally\p\WallyPos485.png
    Renombrado: D:\ZWally\DataSetWally\p\87.jpg -> D:\ZWally\DataSetWally\p\WallyPos486.png
    Renombrado: D:\ZWally\DataSetWally\p\99.jpg -> D:\ZWally\DataSetWally\p\WallyPos487.png
    Renombrado completado.
    


```python

```
