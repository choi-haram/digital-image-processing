# 얼굴인식 프로젝트


## 프로젝트 
<b>프로젝트 소개</b> : 어벤져스 멤버들의 얼굴을 학습시켜 놓고 얼굴을 인식시키는 프로젝트 - 디지털 영상처리 과제<br>
<b>팀원</b> : 컴퓨터공학과 최하람<br>
<b>기간</b> : 22.11.16

## 실행환경
- Visual Studio Code : 1.72.2
- MacOS : 12.3.1
- Live Server : v5.7.4

## 사용한 라이브러리
- faceapi.nets


## 소스코드

```javaScript
// 파일을 가지고 오기 위해 이미지 업로드 ID를 사용해 이미지 요소 갖고 오기
const imageUpload = document.getElementById('imageUpload')


// 사용할 다양한 모델을 로드하기
Promise.all([
  faceapi.nets.faceRecognitionNet.loadFromUri('/models'),
  faceapi.nets.faceLandmark68Net.loadFromUri('/models'),
  faceapi.nets.ssdMobilenetv1.loadFromUri('/models')
]).then(start)

async function start() {
  const container = document.createElement('div')
  container.style.position = 'relative'
  document.body.append(container)
  const labeledFaceDescriptors = await loadLabeledImages()
  const faceMatcher = new faceapi.FaceMatcher(labeledFaceDescriptors, 0.6)
  let image
  let canvas
  //페이지가 모든 모델 로드를 완료했을 때 알 수 있도록 하기
  document.body.append('Loaded')

  // 이미지 업로드 하기
  imageUpload.addEventListener('change', async () => {
    if (image) image.remove()
    if (canvas) canvas.remove()
    // 첫 번째 이미지 업로드 파일을 갖고 와 image 변수에 넣기
    image = await faceapi.bufferToImage(imageUpload.files[0])
    container.append(image)
    canvas = faceapi.createCanvasFromMedia(image)
    container.append(canvas)
    const displaySize = { width: image.width, height: image.height }
    faceapi.matchDimensions(canvas, displaySize)
    const detections = await faceapi.detectAllFaces(image).withFaceLandmarks().withFaceDescriptors()
    const resizedDetections = faceapi.resizeResults(detections, displaySize)
    const results = resizedDetections.map(d => faceMatcher.findBestMatch(d.descriptor))
    results.forEach((result, i) => {
      const box = resizedDetections[i].detection.box
      const drawBox = new faceapi.draw.DrawBox(box, { label: result.toString() })
      drawBox.draw(canvas)
    })
  })
}

// 저장된 이미지 이름 출력시키는 함수
function loadLabeledImages() {
  const labels = ['Black Widow', 'Captain America', 'Captain Marvel', 'Hawkeye', 'Jim Rhodes', 'Thor', 'Tony Stark']
  return Promise.all(
    labels.map(async label => {
      const descriptions = []
      // 많은 이미지를 넣을수록 이미지 인식률이 올라가기 떄문에 많은 이미지 넣을 때를 대비해 루프를 넣음
      for (let i = 1; i <= 2; i++) {
        // 깃허브에 있는 이미지 가져오기
        const img = await faceapi.fetchImage(`https://raw.githubusercontent.com/WebDevSimplified/Face-Recognition-JavaScript/master/labeled_images/${label}/${i}.jpg`)
        const detections = await faceapi.detectSingleFace(img).withFaceLandmarks().withFaceDescriptor()
        descriptions.push(detections.descriptor)
      }

      return new faceapi.LabeledFaceDescriptors(label, descriptions)
    })
  )
}

```


## Commit Rule

<aside>
💡 커밋 제목은 최대 50자 
입력본문은 한 줄 최대 72자 입력
Commit 메세지

</aside>

🪛[chore]: 코드 수정, 내부 파일 수정.

✨[feat]: 새로운 기능 구현.

🎨[style]: 스타일 관련 기능.(코드의 구조/형태 개선)

➕[add]: Feat 이외의 부수적인 코드 추가, 라이브러리 추가

🔧[file]: 새로운 파일 생성, 삭제 시.

🐛[fix]: 버그, 오류 해결.

🔥[del]: 쓸모없는 코드/파일 삭제.

📝[docs]: README나 WIKI 등의 문서 개정.

💄[mod]: storyboard 파일,UI 수정한 경우.

✏️[correct]: 주로 문법의 오류나 타입의 변경, 이름 변경 등에 사용.

🚚[move]: 프로젝트 내 파일이나 코드(리소스)의 이동.

⏪️[rename]: 파일 이름 변경이 있을 때 사용.

⚡️[improve]: 향상이 있을 때 사용.

♻️[refactor]: 전면 수정이 있을 때 사용.

🔀[merge]: 다른브렌치를 merge 할 때 사용.

✅ [test]: 테스트 코드를 작성할 때 사용.


## 출처 & 참고
- https://github.com/WebDevSimplified/Face-Recognition-JavaScript
- https://youtu.be/AZ4PdALMqx0
