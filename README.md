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

