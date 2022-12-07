#  Teachable Machine으로 닮은 그룹 찾기

## 프로젝트 
<b>프로젝트 소개</b> : Teachable Machine을 이용하여 모델을 만들고 웹페이지를  사진 파일을 업로드하면 닮은 여자아이돌 그룹을 찾을 수 있다. - 디지털영상처리 종합 팀 과제<br>
<b>팀원</b> : 컴퓨터공학과 최하람(기획, 디자인, 개발, 테스트)<br>
<b>기간</b> : 22.11.17 ~ 12.05

## 실행환경
- MacOS : 12.3.1
- Visual Studio Code : 1.73.1 (Universal)
- [Visual Studio Code 확장] Live Server : v5.7.4

## 사용한 라이브러리

## 폴더 구조
![image](https://user-images.githubusercontent.com/62204475/205503092-0fb888b5-7631-4842-8783-44e39b4a1049.png)

- my_model(디렉토리)
- index.html
- style.css

## 소스코드
#### index.html
```HTML
<!-- Copyright (c) 2022 by Aaron Vanston (https://codepen.io/aaronvanston/pen/yNYOXR)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE. -->


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div>Teachable Machine Image Model</div>
<button type="button" onclick="init()">Start</button>
<button type="button" onclick="predict()">예측</button>
<script class="jsbin" src="https://ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js"></script>
<div class="file-upload">
  <button class="file-upload-btn" type="button" onclick="$('.file-upload-input').trigger( 'click' )">Add Image</button>

  <div class="image-upload-wrap">
    <input class="file-upload-input" type='file' onchange="readURL(this);" accept="image/*" />
    <div class="drag-text">
      <h3>Drag and drop a file or select add Image</h3>
    </div>
  </div>
  <div class="file-upload-content">
    <img class="file-upload-image" id="face-image" src="#" alt="your image" />
    <div class="image-title-wrap">
      <button type="button" onclick="removeUpload()" class="remove-image">Remove <span class="image-title">Uploaded Image</span></button>
    </div>
  </div>
</div>
<div id="webcam-container"></div>
<div id="label-container"></div>
<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@1.3.1/dist/tf.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@teachablemachine/image@0.8/dist/teachablemachine-image.min.js"></script>
<script type="text/javascript">
    // More API functions here:
    // https://github.com/googlecreativelab/teachablemachine-community/tree/master/libraries/image

    // the link to your model provided by Teachable Machine export panel
    const URL = "./my_model/";

    let model, webcam, labelContainer, maxPredictions;

    // Load the image model and setup the webcam

    async function init() {
        const modelURL = URL + "model.json";
        const metadataURL = URL + "metadata.json";

        // load the model and metadata, [번역] 모델 및 메타데이터 로드
        // Refer to tmImage.loadFromFiles() in the API to support files from a file picker
        // or files from your local hard drive
        // [번역] 파일 선택기의 파일 또는 로컬 하드 드라이브의 파일을 지원하려면 API의 tmImage.loadFromFiles()를 참조하십시오.
        // Note: the pose library adds "tmImage" object to your window (window.tmImage)
        // [번역] 참고: 포즈 라이브러리는 창에 "tmImage" 개체를 추가합니다(window.tmImage).
        model = await tmImage.load(modelURL, metadataURL);
        maxPredictions = model.getTotalClasses();
        labelContainer = document.getElementById("label-container");
        for (let i = 0; i < maxPredictions; i++) { // and class labels
            labelContainer.appendChild(document.createElement("div"));
        }
    }

    // run the webcam image through the image model
    async function predict() {
        // predict can take in an image, video or canvas html element
        var image = document.getElementById("face-image")
        const prediction = await model.predict(image, false);
        for (let i = 0; i < maxPredictions; i++) {
            const classPrediction =
                prediction[i].className + ": " + prediction[i].probability.toFixed(2);
            labelContainer.childNodes[i].innerHTML = classPrediction;
        }
    }
</script>
<script>function readURL(input) {
    if (input.files && input.files[0]) {
  
      var reader = new FileReader();
  
      reader.onload = function(e) {
        $('.image-upload-wrap').hide();
  
        $('.file-upload-image').attr('src', e.target.result);
        $('.file-upload-content').show();
  
        $('.image-title').html(input.files[0].name);
      };
  
      reader.readAsDataURL(input.files[0]);
  
    } else {
      removeUpload();
    }
  }
  
  function removeUpload() {
    $('.file-upload-input').replaceWith($('.file-upload-input').clone());
    $('.file-upload-content').hide();
    $('.image-upload-wrap').show();
  }
  $('.image-upload-wrap').bind('dragover', function () {
          $('.image-upload-wrap').addClass('image-dropping');
      });
      $('.image-upload-wrap').bind('dragleave', function () {
          $('.image-upload-wrap').removeClass('image-dropping');
  });
  </script>

</body>
</html>
```

#### style.css
```css
body {
    font-family: sans-serif;
    background-color: #eeeeee;
  }
  
  .file-upload {
    background-color: #ffffff;
    width: 600px;
    margin: 0 auto;
    padding: 20px;
  }
  
  .file-upload-btn {
    width: 100%;
    margin: 0;
    color: #fff;
    background: #1FB264;
    border: none;
    padding: 10px;
    border-radius: 4px;
    border-bottom: 4px solid #15824B;
    transition: all .2s ease;
    outline: none;
    text-transform: uppercase;
    font-weight: 700;
  }
  
  .file-upload-btn:hover {
    background: #1AA059;
    color: #ffffff;
    transition: all .2s ease;
    cursor: pointer;
  }
  
  .file-upload-btn:active {
    border: 0;
    transition: all .2s ease;
  }
  
  .file-upload-content {
    display: none;
    text-align: center;
  }
  
  .file-upload-input {
    position: absolute;
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
    outline: none;
    opacity: 0;
    cursor: pointer;
  }
  
  .image-upload-wrap {
    margin-top: 20px;
    border: 4px dashed #1FB264;
    position: relative;
  }
  
  .image-dropping,
  .image-upload-wrap:hover {
    background-color: #1FB264;
    border: 4px dashed #ffffff;
  }
  
  .image-title-wrap {
    padding: 0 15px 15px 15px;
    color: #222;
  }
  
  .drag-text {
    text-align: center;
  }
  
  .drag-text h3 {
    font-weight: 100;
    text-transform: uppercase;
    color: #15824B;
    padding: 60px 0;
  }
  
  .file-upload-image {
    max-height: 200px;
    max-width: 200px;
    margin: auto;
    padding: 20px;
  }
  
  .remove-image {
    width: 200px;
    margin: 0;
    color: #fff;
    background: #cd4535;
    border: none;
    padding: 10px;
    border-radius: 4px;
    border-bottom: 4px solid #b02818;
    transition: all .2s ease;
    outline: none;
    text-transform: uppercase;
    font-weight: 700;
  }
  
  .remove-image:hover {
    background: #c13b2a;
    color: #ffffff;
    transition: all .2s ease;
    cursor: pointer;
  }
  
  .remove-image:active {
    border: 0;
    transition: all .2s ease;
  }
```

#### my_model 디렉토리

저작권 이슈로 업로드 안함(Teachable Machine에서 학습한 모델을 내보내기 하여 만들어진 디렉토리를 my_model로 이름 변경하고 넣으면 된다)

## 실행 결과
![image](https://user-images.githubusercontent.com/62204475/205503142-a6b29d5e-3d8f-43a1-80fe-21cc26833072.png)



## Commit Rule

### Commit Message Structure

> type: Subject
> 
> 
> body
> 
> footer
> 

### Commit Subject  Rule

> 커밋 제목은 최대 50자 
제목 끝에 마침표(.) 금지
Commit 메세지
> 

### Commit Type Rule

🪛[chore]: 코드 수정, 내부 파일 수정

✨[feat]: 새로운 기능 구현

🎨[style]: 스타일 관련 기능.(코드의 구조/형태 개선)

➕[add]: Feat 이외의 부수적인 코드 추가, 라이브러리 추가

🔧[file]: 새로운 파일 생성, 삭제 시

🐛[fix]: 버그, 오류 해결

🔥[del]: 쓸모없는 코드/파일 삭제

📝[docs]: README나 WIKI 등의 문서 개정

💄[mod]: storyboard 파일,UI 수정한 경우

✏️[correct]: 주로 문법의 오류나 타입의 변경, 이름 변경 등에 사용

🚚[move]: 프로젝트 내 파일이나 코드(리소스)의 이동

⏪️[rename]: 파일 이름 변경이 있을 때 사용

⚡️[improve]: 향상이 있을 때 사용

♻️[refactor]: 전면 수정이 있을 때 사용

🔀[merge]: 다른브렌치를 merge 할 때 사용

✅ [test]: 테스트 코드를 작성할 때 사용


### **Commit Body 규칙**

> 제목과 본문을 빈 행으로 분리
입력본문은 한 줄 최대 72자 입력
본문을 사용하여 변경 한 내용과 이유 설명(어떻게 보다는 무엇과 왜를 설명)
한글로 작성
브랜치 이름 규칙
> 
- `STEP1`, `STEP2`, `STEP3`

## 출처 & 참고
- 세상에서 가장 쉬운 인공지능 만들기 1탄 | Teachable Machine으로 AI 과일도감 제작하기 : https://youtu.be/USQGTW34lO8
- file upload input souce code : https://codepen.io/aaronvanston/pen/yNYOXR
