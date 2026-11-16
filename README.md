import cv2
import numpy as np
img1=cv2.imread("flower.jpg")
cv2.imshow("og image",img1)
gray=cv2.cvtColor(img1,cv2.COLOR_BGR2GRAY)
cv2.imshow("gray img",gray)
ret,binary=cv2.threshold(gray,127,255,cv2.THRESH_BINARY)
cv2.imshow("bw",binary)
print(img1.shape)


img2=cv2.imread("flower2.jpg")
cv2.imshow("2nd flo",img2)
img2=cv2.resize(img2,(img1.shape[1],img1.shape[0]))
addi=cv2.add(img1,img2)
cv2.imshow("addition",addi)
su=cv2.subtract(img1,img2)
cv2.imshow("subtraction",su)
blend=cv2.addWeighted(img1,0.5,img2,0.5,0)
cv2.imshow("blend",blend)
blbl=cv2.addWeighted(addi,0.5,su,0.5,0)
cv2.imshow("blblblaaa",blbl)
image=cv2.bitwise_and(img1,img2)
cv2.imshow("ak",image)


cv2.waitKey(0)
cv2.destroyAllWindows()



////////////////////////////////////////////////////////////////


//4 neighbour and 8 neighbour question

first convert into binary

import cv2

img = cv2.imread("image.jpg", 0)

if img is None:
    print("Image not found")
    exit()

_, img = cv2.threshold(img, 127, 255, cv2.THRESH_BINARY)

h, w = img.shape

for i in range(h):
    for j in range(w):
        pixel = img[i, j]
        count4 = 0
        count8 = 0

        # 4-neighbours: N, S, W, E
        neighbours4 = [(-1, 0), (1, 0),
                       (0, -1), (0, 1)]

        # 8-neighbours: includes diagonals
        neighbours8 = [(-1, -1), (-1, 0),
                       (-1, 1), (0, -1),
                       (0, 1), (1, -1),
                       (1, 0), (1, 1)]

        for di, dj in neighbours4:
            ni, nj = i + di, j + dj
            if 0 <= ni < h and 0 <= nj < w:
                if img[ni, nj] == pixel:
                    count4 += 1

        for di, dj in neighbours8:
            ni, nj = i + di, j + dj
            if 0 <= ni < h and 0 <= nj < w:
                if img[ni, nj] == pixel:
                    count8 += 1
                    

        print(i, j, count4, count8)

cv2.imshow("Binary Image", img)
cv2.waitKey(0)
cv2.destroyAllWindows()

///////////////////////////////////////////////////////////////

//euclidiean and manhattan distance

import math

# Read coordinates of first point
x1 = int(input("Enter x1: "))
y1 = int(input("Enter y1: "))

# Read coordinates of second point
x2 = int(input("Enter x2: "))
y2 = int(input("Enter y2: "))

# Calculate Euclidean distance
euclidean = math.sqrt((x2-x1)**2 + (y2-y1)**2)

# Calculate Manhattan distance
manhattan = abs(x2-x1) + abs(y2-y1)

print("Euclidean Distance:", euclidean)
print("Manhattan Distance:", manhattan)

/////////////////////////////////////////////////////////////////

//log transformation

import cv2
import numpy as np

# Read grayscale image
img = cv2.imread("image.jpg", 0)

if img is None:
    print("Image not found")
    exit()

# Convert image to float
img_float = np.float32(img)

# Calculate scaling constant
c = 255 / np.log(1 + np.max(img_float))

# Apply log transformation
log_img = c * np.log(1 + img_float)

# Convert to 8-bit image
log_img = np.uint8(log_img)

# Display images
cv2.imshow("Original Image", img)
cv2.imshow("Log Transformed Image", log_img)

cv2.waitKey(0)
cv2.destroyAllWindows()

///////////////////////////////////////////////////////

//gamma correction

import cv2
import numpy as np

# Read colour image
img = cv2.imread("image.jpg")

if img is None:
    print("Image not found")
    exit()

# Convert to grayscale
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

# Gamma values
gamma_values = [0.2, 0.5, 1.5, 2, 3]

for gamma in gamma_values:

    # Create lookup table
    table = np.array([
        ((i / 255.0) ** gamma) * 255
        for i in range(256)
    ]).astype("uint8")

    # Apply gamma correction
    gamma_gray = cv2.LUT(gray, table)
    gamma_color = cv2.LUT(img, table)

    # Display results
    cv2.imshow("Gray Gamma " + str(gamma), gamma_gray)
    cv2.imshow("Color Gamma " + str(gamma), gamma_color)

cv2.waitKey(0)
cv2.destroyAllWindows()


/////////////////////////////////////////////////

//histogram equalisation


import cv2
import matplotlib.pyplot as plt

# Read grayscale image
img = cv2.imread("flower.jpg", 0)

if img is None:
    print("Image not found")
    exit()

# Perform histogram equalization
equalized = cv2.equalizeHist(img)

# Display images and histograms
plt.figure(figsize=(10, 8))

plt.subplot(2, 2, 1)
plt.imshow(img, cmap='gray')
plt.title("Original Image")
plt.axis("off")

plt.subplot(2, 2, 2)
plt.hist(img.ravel(), 256, [0, 256])
plt.title("Original Histogram")

plt.subplot(2, 2, 3)
plt.imshow(equalized, cmap='gray')
plt.title("Equalized Image")
plt.axis("off")

plt.subplot(2, 2, 4)
plt.hist(equalized.ravel(), 256, [0, 256])
plt.title("Equalized Histogram")

plt.tight_layout()
plt.show()

////////////////////////////////////////////////////////

//histogram equalising hsv

import cv2
import matplotlib.pyplot as plt

# Read colour image
img = cv2.imread("image.jpg")

if img is None:
    print("Image not found")
    exit()

# Convert BGR to HSV
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)

# Split HSV channels
h, s, v = cv2.split(hsv)

# Equalize the V channel
v_equalized = cv2.equalizeHist(v)

# Merge the channels
hsv_equalized = cv2.merge([h, s, v_equalized])

# Convert HSV back to BGR
result = cv2.cvtColor(hsv_equalized, cv2.COLOR_HSV2BGR)

# Display images
plt.subplot(1, 2, 1)
plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
plt.title("Original Image")
plt.axis("off")

plt.subplot(1, 2, 2)
plt.imshow(cv2.cvtColor(result, cv2.COLOR_BGR2RGB))
plt.title("Equalized Image")
plt.axis("off")

plt.show()

////////////////////////////////////////////////////////////////////////

//saving jpeg,png and jpg 

import cv2
import os

img = cv2.imread("image.jpg")

if img is None:
    print("Image not found")
    exit()

qualities = [100, 70, 50, 30, 10]

for q in qualities:
    filename = "image_" + str(q) + ".webp"

    cv2.imwrite(filename, img,
                [cv2.IMWRITE_WEBP_QUALITY, q]) #IMWRITE_PNG_COMPRESSION,IMWRITE_JPEG_QUALITY

    size = os.path.getsize(filename)

    print("Quality:", q, "File Size:", size, "bytes")

///////////////////////////////////////////////////////////////////////

//spacial averaging filters

import cv2
import matplotlib.pyplot as plt
import numpy as np

img = cv2.imread("image.jpg", 0)

if img is None:
    print("Image not found")
    exit()

# Apply averaging filters
f3 = cv2.filter2D(img, -1, np.ones((3,3), np.float32)/9)
f5 = cv2.filter2D(img, -1, np.ones((5,5), np.float32)/25)
f7 = cv2.filter2D(img, -1, np.ones((7,7), np.float32)/49)

# Display results
plt.subplot(2, 2, 1)
plt.imshow(img, cmap='gray')
plt.title("Original")

plt.subplot(2, 2, 2)
plt.imshow(f3, cmap='gray')
plt.title("3x3 Filter")

plt.subplot(2, 2, 3)
plt.imshow(f5, cmap='gray')
plt.title("5x5 Filter")

plt.subplot(2, 2, 4)
plt.imshow(f7, cmap='gray')
plt.title("7x7 Filter")

plt.tight_layout()
plt.show()

//////////////////////////////////////////////////////////////////////////

//gaussian blur

import cv2
import matplotlib.pyplot as plt

img = cv2.imread("image.jpg", 0)

if img is None:
    print("Image not found")
    exit()

g3 = cv2.GaussianBlur(img, (3,3), 0)
g5 = cv2.GaussianBlur(img, (5,5), 0)
g7 = cv2.GaussianBlur(img, (7,7), 0)

plt.subplot(2,2,1)
plt.imshow(img, cmap='gray')
plt.title("Original")

plt.subplot(2,2,2)
plt.imshow(g3, cmap='gray')
plt.title("3x3 Gaussian")

plt.subplot(2,2,3)
plt.imshow(g5, cmap='gray')
plt.title("5x5 Gaussian")

plt.subplot(2,2,4)
plt.imshow(g7, cmap='gray')
plt.title("7x7 Gaussian")

plt.tight_layout()
plt.show()

////////////////////////////////////////////////////////

//median blurring

import cv2
import matplotlib.pyplot as plt

img = cv2.imread("image.jpg", 0)

if img is None:
    print("Image not found")
    exit()

m3 = cv2.medianBlur(img, 3)
m5 = cv2.medianBlur(img, 5)
m7 = cv2.medianBlur(img, 7)

plt.subplot(2,2,1)
plt.imshow(img, cmap='gray')
plt.title("Original")

plt.subplot(2,2,2)
plt.imshow(m3, cmap='gray')
plt.title("3x3 Median")

plt.subplot(2,2,3)
plt.imshow(m5, cmap='gray')
plt.title("5x5 Median")

plt.subplot(2,2,4)
plt.imshow(m7, cmap='gray')
plt.title("7x7 Median")

plt.tight_layout()
plt.show()

////////////////////////////////////////////////////////////////

//laplacian filter

import cv2
import matplotlib.pyplot as plt

img = cv2.imread("image.jpg", 0)

if img is None:
    print("Image not found")
    exit()

l3 = cv2.Laplacian(img, cv2.CV_64F, ksize=1)
l5 = cv2.Laplacian(img, cv2.CV_64F, ksize=3)
l7 = cv2.Laplacian(img, cv2.CV_64F, ksize=5)

l3 = cv2.convertScaleAbs(l3)
l5 = cv2.convertScaleAbs(l5)
l7 = cv2.convertScaleAbs(l7)

plt.subplot(2,2,1)
plt.imshow(img, cmap='gray')
plt.title("Original")

plt.subplot(2,2,2)
plt.imshow(l3, cmap='gray')
plt.title("3x3 Laplacian")

plt.subplot(2,2,3)
plt.imshow(l5, cmap='gray')
plt.title("5x5 Laplacian")

plt.subplot(2,2,4)
plt.imshow(l7, cmap='gray')
plt.title("7x7 Laplacian")

plt.tight_layout()
plt.show()

/////////////////////////////////////////////////////////////

//k means

import pandas as pd
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
df = pd.read_csv('/content/drive/MyDrive/AI_LAB/Exp_22/clustering_data.csv')
X = df.iloc[:, :2].values
fig, axes = plt.subplots(2, 2, figsize=(12, 10))
axes = axes.flatten()
axes[0].scatter(X[:, 0], X[:, 1], c='gray', s=50)
axes[0].set_title("Before Clustering", fontsize=14)
k_values = [2, 3, 4]
for idx, k in enumerate(k_values, start=1):
kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
labels = kmeans.fit_predict(X)
centroids = kmeans.cluster_centers_
axes[idx].scatter(X[:, 0], X[:, 1], c=labels, cmap='viridis', s=50)

axes[idx].scatter(centroids[:, 0], centroids[:, 1], c='red', marker='X', s=150, la-
bel='Centroids')

axes[idx].set_title(f"K-Means (k={k})", fontsize=14)
plt.tight_layout()
plt.show()

////////////////////////////////////////////////////////////////

//knn


import pandas as pd
import numpy as np
from collections import Counter
from sklearn.metrics import *

data = pd.read_csv('/content/drive/MyDrive/AI_LAB/iris.data', header=None).dropna()
X, y = data.iloc[:, :4].values, data.iloc[:, 4].values

np.random.seed(10)
idx = np.random.permutation(len(X))
n = int(0.7 * len(X))
X_train, X_test = X[idx[:n]], X[idx[n:]]
y_train, y_test = y[idx[:n]], y[idx[n:]]

def knn(X_train, y_train, X_test, k):
    pred = []
    for p in X_test:
        d = np.sqrt(np.sum((X_train - p)**2, axis=1))
        nearest = y_train[np.argsort(d)[:k]]
        pred.append(Counter(nearest).most_common(1)[0][0])
    return pred

print("Training:", len(X_train), "Testing:", len(X_test))

for k in [3, 5, 7, 9]:
    y_pred = knn(X_train, y_train, X_test, k)
    print("\nK =", k)
    print("Confusion Matrix:\n", confusion_matrix(y_test, y_pred))
    print("Precision:", precision_score(y_test, y_pred, average='weighted', zero_division=0))
    print("Recall:", recall_score(y_test, y_pred, average='weighted', zero_division=0))
    print("Accuracy:", accuracy_score(y_test, y_pred))
    print("F-Score:", f1_score(y_test, y_pred, average='weighted', zero_division=0))


////////////////////////////////////////////////////////////////////////////

//histogram manually


import cv2
import numpy as np
import matplotlib.pyplot as plt

img = cv2.imread('image.jpg', 0)

hist = np.zeros(256, dtype=int)

for i in range(img.shape[0]):
    for j in range(img.shape[1]):
        hist[img[i, j]] += 1

plt.plot(hist)
plt.title("Image Histogram")
plt.xlabel("Pixel Intensity")
plt.ylabel("Frequency")
plt.show()


///////////////////////////////////////////////////////////////////////////
