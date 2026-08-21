# YOLO–SAM Crack Segmentation

YOLO로 균열 후보 마스크를 검출하고 SAM 2.1로 경계를 보정하는 Google Colab 프로젝트입니다.

## 구성

- `notebooks/YOLO_SAM_Crack_Final_Process.ipynb`: YOLO → SAM 일괄 처리 Colab 노트북
- `weights/best.pt`: 학습된 YOLO 균열 세그멘테이션 가중치
- `requirements.txt`: 주요 Python 패키지
- `.gitignore`: 입력 이미지와 생성 결과 등 대용량 작업 파일 제외

## Colab 실행

1. 노트북을 Google Colab에서 엽니다.
2. `weights/best.pt`를 Google Drive에 업로드합니다.
3. 노트북의 `YOLO_MODEL_PATH`, `INPUT_DIR`, `OUTPUT_DIR`를 실제 Drive 경로에 맞게 수정합니다.
4. 입력 폴더에 균열 이미지를 넣고 셀을 순서대로 실행합니다.

기본 설정은 YOLO 신뢰도 0.25, 입력 크기 1280이며, SAM 후보는 YOLO 마스크와의 IoU 및 면적비 조건으로 검증됩니다.

## 주요 결과

노트북은 원본, YOLO 마스크, SAM 보정 마스크, 비교 오버레이, YOLO 형식 폴리곤 라벨, 이미지별 요약 CSV 및 Roboflow 업로드용 ZIP을 생성합니다.

## 주의

`best.pt`는 GitHub에서 직접 실행되는 파일이 아닙니다. Colab 또는 로컬 환경에서 경로를 지정해 사용하세요. 데이터셋과 실행 결과는 저장소에 포함하지 않습니다.
