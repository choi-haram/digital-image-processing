## 얼굴 추출 방법 및 구현

### 참여인원
팀장 : 안동대학교 컴퓨터공학과 20180684 오예준<br>
팀원 : 안동대학교 컴퓨터공학과 20191007 최하람

### 목차
- 딥러닝을 활용한 얼굴 추출 방법
- 코드
- Haar Cascade을 이용하여 얼굴추출 하기
## 딥러닝을 활용한 얼굴 추출 방법

![image](https://user-images.githubusercontent.com/62204475/201092887-37550c10-5bae-446f-a0c4-db94fbf8fe67.png)



## 코드

```
let src = cv.imread('canvasInput'); //이미지 불러오기
let gray = new cv.Mat();
cv.cvtColor(src, gray, cv.COLOR_RGBA2GRAY, 0);
let faces = new cv.RectVector();
let eyes = new cv.RectVector();
let faceCascade = new cv.CascadeClassifier();
let eyeCascade = new cv.CascadeClassifier();
// load pre-trained classifiers
faceCascade.load('haarcascade_frontalface_default.xml');
eyeCascade.load('haarcascade_eye.xml');
// detect faces
let msize = new cv.Size(0, 0);
faceCascade.detectMultiScale(gray, faces, 1.1, 3, 0, msize, msize);
for (let i = 0; i < faces.size(); ++i) {
    let roiGray = gray.roi(faces.get(i));
    let roiSrc = src.roi(faces.get(i));
    let point1 = new cv.Point(faces.get(i).x, faces.get(i).y);
    let point2 = new cv.Point(faces.get(i).x + faces.get(i).width,
                              faces.get(i).y + faces.get(i).height);
    cv.rectangle(src, point1, point2, [255, 255, 0, 255]); //노란색 사각형 출력
    // detect eyes in face ROI
    eyeCascade.detectMultiScale(roiGray, eyes);
    for (let j = 0; j < eyes.size(); ++j) {
        let point1 = new cv.Point(eyes.get(j).x, eyes.get(j).y);
        let point2 = new cv.Point(eyes.get(j).x + eyes.get(j).width,
                                  eyes.get(j).y + eyes.get(j).height);
        cv.rectangle(roiSrc, point1, point2, [255, 0, 255, 255]); //분홍색 사각형 출력
    }
    roiGray.delete(); roiSrc.delete();
}
cv.imshow('canvasOutput', src);
src.delete(); gray.delete(); faceCascade.delete();
eyeCascade.delete(); faces.delete(); eyes.delete();
```

## Haar Cascade을 이용하여 얼굴추출 하기
우리팀은 OpenCV 라이브러리에 있는 Haar Cascade를 사용하여 얼굴 추출을 해보았다.

### 입력사진<br>
<img width="312" alt="입력 사진" src="https://user-images.githubusercontent.com/62204475/201570900-fee0c8c9-c1ce-462f-bd7d-aa34ce7a2245.png">


### 출력사진<br>
![출력 사진](https://user-images.githubusercontent.com/62204475/201570879-4a362455-e913-4513-ad8e-10d4b2beff83.png)

