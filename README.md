# Concrete Crack Segmentation with YOLO & SAM

> 콘크리트 균열을 픽셀 단위로 탐지하고, YOLO의 빠른 예측과 SAM의 경계 보정을 결합한 컴퓨터 비전 프로젝트

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![Ultralytics](https://img.shields.io/badge/Ultralytics-YOLO11%20%7C%20YOLO26-111F68)
![SAM](https://img.shields.io/badge/Segmentation-SAM%202.1-6C5CE7)
![Colab](https://img.shields.io/badge/Environment-Google%20Colab-F9AB00?logo=googlecolab&logoColor=white)
![Task](https://img.shields.io/badge/Task-Instance%20Segmentation-2EA44F)

김주형 · 단국대학교 토목환경공학과  
Civil Engineering × Computer Vision

## Project Overview

시설물 균열 점검에서는 균열의 존재뿐 아니라 위치, 형태, 연속성 및 면적을 정밀하게 파악해야 합니다. 이 프로젝트는 콘크리트 이미지에서 균열을 인스턴스 세그멘테이션하고, 후처리 결과를 정량 분석 및 수동 검수 가능한 형태로 저장하는 파이프라인을 구축합니다.

핵심 목표는 다음과 같습니다.

- YOLO11·YOLO26 모델의 균열 세그멘테이션 학습 및 비교
- YOLO 예측 마스크를 SAM 2.1로 보정
- 균열별 신뢰도와 픽셀 면적 산출
- 비교 오버레이와 YOLO polygon 라벨 자동 생성
- Google Colab과 Drive 기반의 재현 가능한 학습·추론 환경 구성

## Pipeline

```mermaid
flowchart TD
    A["Concrete image"] --> B["YOLO segmentation"]
    B --> C["Candidate mask"]
    C --> D["SAM 2.1 refinement"]
    D --> E["Quality filtering"]
    E --> F["Final mask & polygon"]
    F --> G["Area, confidence & CSV"]
```

YOLO가 균열 후보를 빠르게 찾고, SAM이 해당 후보 주변에서 경계를 다시 추정합니다. SAM 결과가 YOLO 마스크와 지나치게 다르면 IoU·면적비 조건으로 제외하여 콘크리트 배경 전체가 선택되는 오류를 줄입니다.

## Key Features

| 기능 | 내용 |
|---|---|
| Model training | YOLO11 및 YOLO26m 균열 세그멘테이션 학습 |
| Model inference | 단일 이미지 업로드, 마스크·박스·신뢰도 시각화 |
| SAM refinement | YOLO 마스크를 prompt로 사용한 SAM 2.1 경계 보정 |
| Quality control | 최소 면적, IoU, 면적비 기반 후보 필터링 |
| Quantification | 균열별·전체 픽셀 면적과 이미지 대비 면적비 산출 |
| Label export | Roboflow 수정용 YOLO polygon 라벨과 ZIP 생성 |
| Reproducibility | Colab, Google Drive, 고정 seed 기반 실행 흐름 |

## Experiments

| 모델 | 입력 크기 | 학습 설정 | 용도 |
|---|---:|---|---|
| YOLO11s-seg | 640 | 10·50·100 epochs | epoch 증가에 따른 성능 비교 |
| YOLO11m-seg | 640 | 50·100 epochs | 모델 크기 변화 비교 |
| YOLO26m-seg | 1024 | 최대 250 epochs, patience 50 | 고해상도 균열 학습 |
| YOLO + SAM 2.1 | YOLO 1280 / SAM 1024 | 마스크 기반 refinement | 경계 보정 및 라벨 생성 |

### Preliminary Model Comparison

아래 값은 각 학습 결과 그래프의 **마지막 epoch 지점에서 읽은 근삿값**입니다. 서로 다른 학습 실행 결과이므로 잠정 비교용이며, 최종 성능은 동일한 validation split에서 각 `best.pt`를 재평가하여 확정할 예정입니다.

#### Segmentation Mask Metrics

| Run | Model | Input | Epochs | Precision (M) | Recall (M) | mAP50 (M) | mAP50-95 (M) |
|---|---|---:|---:|---:|---:|---:|---:|
| s10 | YOLO11s-seg | 640 | 10 | ≈0.69 | ≈0.53 | ≈0.47 | ≈0.15 |
| **s50** | **YOLO11s-seg** | **640** | **50** | **≈0.75** | **≈0.67** | **≈0.68** | **≈0.24** |
| m100 | YOLO11m-seg | 640 | 100 | ≈0.75 | ≈0.64 | ≈0.66 | ≈0.23 |

#### Bounding Box Metrics

| Run | Model | Input | Epochs | Precision (B) | Recall (B) | mAP50 (B) | mAP50-95 (B) |
|---|---|---:|---:|---:|---:|---:|---:|
| s10 | YOLO11s-seg | 640 | 10 | ≈0.78 | ≈0.61 | ≈0.63 | ≈0.41 |
| s50 | YOLO11s-seg | 640 | 50 | ≈0.79 | ≈0.76 | ≈0.80 | ≈0.61 |
| m100 | YOLO11m-seg | 640 | 100 | ≈0.84 | ≈0.75 | ≈0.80 | ≈0.60 |

현재 제공된 그래프 기준으로는 **s50이 가장 높은 Mask Recall, Mask mAP50 및 Mask mAP50-95를 기록**했습니다. 다만 이 결론은 동일 조건 재평가 전의 잠정 결과입니다.

## Repository Structure

```text
YOLO/
├── notebooks/
│   ├── YOLO11_Crack_Model_Open_and_Test_Colab.ipynb
│   ├── YOLO26m_Crack_Training.ipynb
│   └── YOLO_SAM_Crack_Final_Process.ipynb
├── weights/
│   └── best.pt
├── requirements.txt
├── .gitignore
└── README.md
```

### Notebooks

| 노트북 | 역할 |
|---|---|
| [YOLO11 model test](notebooks/YOLO11_Crack_Model_Open_and_Test_Colab.ipynb) | 저장된 YOLO11 모델을 불러와 단일 이미지 예측 및 Drive 저장 |
| [YOLO26m training](notebooks/YOLO26m_Crack_Training.ipynb) | Roboflow 데이터셋 다운로드, YOLO26m 학습 및 최적 모델 검증 |
| [YOLO → SAM process](notebooks/YOLO_SAM_Crack_Final_Process.ipynb) | 여러 이미지의 YOLO 예측, SAM 보정, 결과·라벨 일괄 저장 |

## Quick Start

### 1. Google Colab에서 노트북 열기

목적에 맞는 노트북을 선택하고 GPU 런타임에서 위에서부터 실행합니다.

### 2. 모델 경로 설정

저장소의 `weights/best.pt`를 Google Drive에 업로드한 뒤 노트북의 모델 경로를 수정합니다.

```python
YOLO_MODEL_PATH = "/content/drive/MyDrive/your_folder/best.pt"
```

### 3. 추론 설정 조정

```python
YOLO_CONF = 0.25
YOLO_IMGSZ = 1280
MIN_COMPONENT_AREA = 100
MIN_IOU = 0.50
MIN_AREA_RATIO = 0.50
MAX_AREA_RATIO = 2.00
```

### 4. 결과 확인

YOLO–SAM 파이프라인은 다음 결과를 생성합니다.

```text
results/
├── masks/
│   ├── yolo/
│   ├── sam_raw/
│   └── final/
├── comparison_overlays/
├── roboflow_dataset/
├── processing_summary.csv
└── processing_errors.csv
```

## Dataset

데이터셋은 Roboflow에서 관리하며 저장소에는 포함하지 않습니다.

- YOLO11 experiments: `university-bswxt / crack-uuw5e / version 8`
- YOLO26 experiments: `aos-workspace / erc2-59n5c / version 1`
- Task: instance segmentation
- Class: `crack`

데이터 사용 전 각 데이터셋의 공개 범위와 라이선스를 별도로 확인해야 합니다.

## Current Limitations

- 픽셀 면적을 실제 면적으로 환산하려면 기준 길이 또는 카메라 보정 정보가 필요합니다.
- SAM은 입력 prompt가 부정확하면 균열 주변 배경을 과도하게 선택할 수 있습니다.
- 서로 다른 모델의 공정한 비교를 위해 동일 test split 기반 재평가가 필요합니다.
- 구조적 위험도 평가는 균열 마스크만으로 확정할 수 없으며 전문가 판단과 현장 정보가 필요합니다.

## Next Steps

- [ ] YOLO11s·YOLO11m·YOLO26m 동일 조건 정량 비교
- [ ] 대표 성공·실패 사례 시각화
- [ ] 균열 폭·길이·연속성을 반영한 평가 지표 검증
- [ ] 실제 축척을 이용한 면적 및 폭 측정
- [ ] YOLO–SAM 보정 전후 성능 비교

## Security

Roboflow API 키나 기타 인증정보를 노트북에 직접 입력해 저장하지 않습니다. 실행 시 환경 변수 또는 보이지 않는 입력 방식을 사용하세요. 이미 노출된 키는 폐기하고 재발급해야 합니다.

## Disclaimer

본 저장소는 연구 및 교육 목적의 프로젝트입니다. 예측 결과만으로 시설물의 안전등급이나 보수 필요성을 확정해서는 안 됩니다.
