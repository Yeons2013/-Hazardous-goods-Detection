# X-Ray 이미지를 이용한 위해물품 검출(Object Detection)

Third Mini Project(Object Detection)
<br>
<br>
목록 | 설명
------------|--------------------
[Yolo_v3,_v4.ipynb](https://github.com/Yeons2013/-Hazardous-goods-Detection/blob/master/Preprocessing_YoloV5.ipynb) | YoloV3,V4(Colab)
[Preprocessing_YoloV5.ipynb](https://github.com/Yeons2013/AdvertisingClassification/blob/master/DataGet.ipynb) | YoloV5(JupyterNotebook)

---

## 프로젝트 개요

### 1. 주제 및 선정 배경
+ 선정 배경 : 출국시간을 단축하면서 저장매체를 따로 빼줘야 하는 불편함이 해소되는 방안 고려
+ 주제 : X-Ray 이미지를 활용한 위해물품 검출
<br>

### 2. 진행 방향
+ YOLO V3, V4, V5, V6, V7 등 여러 모델을 활용해 Best mAP 모델 구축
<br>

### 3. 개발 환경
+ Google Colab Pro + (Yolo V3, V4, V6)
+ Anaconda + Pytorch + VScode(Yolo V5, V7)
<br>

### 4. 기대 효과
+ Object Detection Model의 Security Chect Task 자동화 (출국시간 단출 + 불편 해소)
<br>


---

## 프로젝트 팀 구성 및 역할
<br>

팀 원 | 담당업무|
----  |------|
강동호 | YOLO v7 모델 구축, 시각화
이정아 | YOLO v3, v4 모델 구축, 모델 핸들링
박혜정 | YOLO v3,4 모델 구축, ppt 제작
정가영 | YOLO v4 모델 구축, 모델 핸들링, 발표
최현호 | 데이터 전처리, YOLO v6 모델 구축
황성연 | YOLO v3,v5 모델 구축, 모델 핸들링

<br>
<br>


---

## 프로젝트 수행 기간

<img src = 'https://media.discordapp.net/attachments/1002189622912221250/1088307783973224499/image.png' width= 500>



<br>
<br>


---

## 학습 데이터
AI Hub의 '위해물품 엑스레이 이미지' <br>
(https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=233)


<img src='https://media.discordapp.net/attachments/1002189622912221250/1088308506752458802/image.png'>

<img src='https://media.discordapp.net/attachments/1002189622912221250/1088309072954150973/image.png?width=1406&height=671'>


<br>

### 데이터의 수 (Single은 단 하나의 객체만 포함하고 있는 이미지를 의미)
분 류 | 480 × 480 | 416 × 416(single O) | 416 × 416(single X)|
----  |----------|------| ------|
데이터 개수 | 109,932개 | 107,883개 | 75,335개
객체 개수 | 301,475개 | 294,699개 | 261,190개



<br>
<br>


---
## 전처리
### 1. 사이즈 조정 작업
+ 원본의 사이즈는 1920 * 1080. 가지고 있는 자원으로 학습이 어려움
+ 480 * 480, 416 * 416 으로 사이즈 축소(가로 세로 비율이 유지 될 수 있도록 패딩)
+ 상, 하, 좌, 우의 여백을 제거해주는 Crop 작업
(Crop 작업 시 실제 객체 부분의 비율을 늘려 작은 객체를 잘 잡을 수 있도록 함 + 일부 이미지 끝단에 걸친 이상치 제거)

### 2. Annotation Format 변경
+ Pascal 형식의 Bbox : (Xmin, Ymin, Xma, Ymax) 절대좌표
+ Yolo 형식 : ( X-Center, Y-Center, Width, Height ) 상대좌표
+ Pascal → Yolo Format 으로 변경 작업
  
<img src='https://media.discordapp.net/attachments/1002189622912221250/1088313084445208636/image.png' width=400>

<br>
<br>

---
## 모델 학습

### 1. Crop 유무의 결과 차이
+ Yolo V4 Non-cropping Result
<br>
<img src='https://media.discordapp.net/attachments/1002189622912221250/1088314734698303589/image.png' width = 500>

<br>

+ Yolo V4 Cropping Result
<br>
<img src='https://media.discordapp.net/attachments/1002189622912221250/1088315115591442533/image.png' width = 500>

▶ Crop 후의 결과가 보다 높은 mAP를 보임


### 2. 각 모델 별 결과
+ 각 모델별로 하이퍼 파라미터 튜닝을 통해 Best 성능을 뽑아봄 (Crop O)
+ YoloV3 : mAP 0.904 (Single 데이터 미포함)
+ YoloV4 : mAP 0.947 (Single 데이터 포함)
+ YoloV5 : mAP 0.932 (Single 데이터 미포함)
+ YoloV6 : mAP 0.865 (Single 데이터 미포함)
+ YoloV7 : mAP 0.950 (Single 데이터 미포함)

<br>

### 3. Best Model 지표(Yolo V7)
<img src='https://media.discordapp.net/attachments/1002189622912221250/1088317834209271890/image.png' width = 600>
<br>
<img src='https://media.discordapp.net/attachments/1002189622912221250/1088318067605504070/image.png' width = 600>
<br>

(간단한 시연 https://www.youtube.com/watch?v=_O-fLSeARNE&ab_channel=Alpacoteam)


---
## 최종 결과 및 피드백
### 1. 긍정적인 점
+ 최종 mAP는 0.95로 높은 수치를 보여줌 → Yolo V7은 기존의 Object Dector보다 빠른 속도와 정확도를 자랑하는 모델. inferece cost를 증가시키기 않는 최적화 된 모듈과 방법들이 사용되고, 이런 부분들이 높은 mAP로 이어졌다고 판단

### 개선해야 할 점
+ 학습과정에서 시간적인 문제로 완전한 통제하에 학습하지 못함. (Yolo V5, V6, V7의 경우 Single 포함 데이터 학습은 시도하지 못함)
+ 높은 mAP에도 불구하고 Inference 시 객체를 정확하게 잡아내지 못함 → 학습 전 확인한 샘플 데이터의 Annotation 좌표에는 문제가 없었으나, 전체 데이터를 분석한 결과 상당 수의 데이터의 Annotation B-Box 좌표가 정확하지 않음을 확인 → 학습할 데이터 정제작업이 보다 깊게 선행되어야 함


<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


