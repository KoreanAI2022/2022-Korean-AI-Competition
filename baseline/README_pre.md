
# Dataset
> 본 문서는 2022 한국어 AI 경진대회 예선 데이터셋에 대한 설명입니다.
### Dataset Name
`TRACK1 - 자유대화` : `t1-free`  
`TRACK1 - 명령어` : `t1-cmd`  
`TRACK2 - 차량 내 대화 및 명령어` : `t2-car`  
`TRACK2 - 주요 영역별 회의 음성` : `t2-conf`  

`rootpath = nsml.DATSET_PATH`
### Train Dataset

`DATASET_PATH/train/train_data/`  
- `train_data (헤더가 없는 pcm 형식)`
  - 파일명: idx000000 ~ 
  - PCM 샘플링 주파수: 16000Hz
  - Mono Channel


### Train Label

`DATASET_PATH/train/train_label`
  - `train_label (csv 형식)`
    - columns - `["filename", "text"]`
    - `filename` - train_data 폴더에 존재하는 파일명 (ex. idx000000)
    - `text` - train_data 폴더에 존재하는 파일의 음성 전사 Text 정보 (ex. 인공지능 훈민정음에 꽃 피우다)


# Baseline code
- `main.py` : 실행파일
- `setup.py`: 환경설정(Base Docker Image, Python libraries)
- `nsml_package.txt`: packages(by apt or yum)
- `modules`
  - `audio` : 오디오 모듈(parsing)
  - `data.py` : 데이터 로더
  - `inference.py`: 인퍼런싱
  - `metrics.py` : 평가지표 관련(CER)
  - `model.py`: 모델 빌드 관련(DeepSpeech2)
  - `preprocess.py`: 전처리(라벨/transcripts 제작)
  - `trainer.py`: 학습 관련
  - `utils.py` : 기타 설정 및 필요 함수
  - `vocab.py` : Vocabulary Class 파일

## 실행 방법
```bash
# 명칭이 't1-cmd'인 데이터셋을 사용해 세션 실행하기
$ nsml run -d t1-cmd
# 메인 파일명이 'main.py'가 아닌 경우('-e' 옵션으로 entry point 지정)
# 예: nsml run -d t1-cmd -e anotherfile.py
$ nsml run -d t1-cmd -e [파일명]
# 2GPU와 16CPU, 160GB 메모리를 사용하여 세션 실행하기   
$ nsml run -d t1-cmd -g 2 -c 16 --memory 160G  

# 세션 로그 확인하기
# 세션명: [유저ID/데이터셋/세션번호] 구조
$ nsml logs -f [세션명]

# 세션 종료 후 모델 목록 및 제출하고자 하는 모델의 checkpoint 번호 확인하기
# 세션명: [유저ID/데이터셋/세션번호] 구조
$ nsml model ls [세션명]

# 모델 제출 전 제출 코드에 문제가 없는지 점검하기('-t' 옵션)
$ nsml submit -t [세션명] [모델_checkpoint_번호]

# 모델 제출하기
# 제출 후 리더보드에서 점수 확인 가능
nsml submit [세션명] [모델_checkpoint_번호]
```

본 베이스라인 코드는 김수환 님께서 개발해 공개하신 kospeech (https://github.com/sooftware/kospeech) 를 기반으로 하였으며 
NSML 플랫폼에서 사용 가능한 형태로 수정하였습니다.
