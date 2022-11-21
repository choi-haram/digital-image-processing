#  	Java Script로 영상을 읽어 화면에 출력하기

## 프로젝트 
<b>프로젝트 소개</b> : Java Script로 영상을 읽어 화면에 출력하기 프로젝트 - 디지털영상처리 과제<br>
<b>팀원</b> : 컴퓨터공학과 최하람<br>
<b>기간</b> : 22.11.03(일회성)

## 실행환경
- Visual Studio Code Version: 1.73.1 (Universal)
- MacOS : 12.3.1

## 소스코드
#### index.html
```html
<html>
    <script defer src="rfile.js"></script>
    <input type='file' accept='image/*' onchange='openFile(event)'><br>
    <img id='imageUpload'>
<html>
```

#### rfile.js
```JavaScript
var openFile = function(file) {
    var input = file.target;
    var reader = new FileReader();
    reader.onload = function(){
        var dataURL = reader.result;
        var imageUpload = document.getElementById('imageUpload');
        imageUpload.src = dataURL;
    };
    reader.readAsDataURL(input.files[0]);
};
```

## 실행결과

<img width="410" alt="image" src="https://user-images.githubusercontent.com/62204475/202946053-afe668e3-d161-4bce-b5c9-d381cdf3318f.png">


## 사용한 라이브러리

## 디자인

## 앱 UI

## 앱 아이콘


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

> 커밋 제목은 최대 50자<br>
제목 끝에 마침표(.) 금지<br>
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

> 제목과 본문을 빈 행으로 분리<br>
입력본문은 한 줄 최대 72자 입력<br>
본문을 사용하여 변경 한 내용과 이유 설명(어떻게 보다는 무엇과 왜를 설명)<br>
한글로 작성<br>
브랜치 이름 규칙<br>
- `STEP1`, `STEP2`, `STEP3`

## 출처 & 참고
