# YOLO11 실험 근거 및 자료 출처

2026-10-22에 제공된 `자료.zip`에서 선별했다. 이미지 바이트는 수정하지 않았으며 설명 가능한 영문 파일명으로만 변경했다. 기존 노트북과 가중치는 변경하지 않았다.

## 출처 목록

| 저장소 파일 | ZIP 내부 원본 경로 |
|---|---|
| [assets/experiments/final_epoch_summary.jpg](../assets/experiments/final_epoch_summary.jpg) | `비교/화면 캡처 2026-08-03 172111.jpg` |
| [assets/experiments/best_epoch_summary.jpg](../assets/experiments/best_epoch_summary.jpg) | `비교/화면 캡처 2026-08-03 172146.jpg` |
| [assets/experiments/final_vs_best_summary.jpg](../assets/experiments/final_vs_best_summary.jpg) | `비교/화면 캡처 2026-08-03 172218.jpg` |
| [assets/experiments/yolo11s_epoch_training_curves.png](../assets/experiments/yolo11s_epoch_training_curves.png) | `비교/다운로드 (1).png` |
| [assets/experiments/yolo11s_epoch_segmentation_loss.png](../assets/experiments/yolo11s_epoch_segmentation_loss.png) | `비교/다운로드 (2).png` |
| [assets/experiments/yolo11m_100epoch_training_curves.png](../assets/experiments/yolo11m_100epoch_training_curves.png) | `m100 지표/다운로드 (3).png` |
| [assets/experiments/yolo11s_epoch_prediction_comparison.png](../assets/experiments/yolo11s_epoch_prediction_comparison.png) | `비교/다운로드 (11).png` |
| [assets/experiments/crack_mask_instance_example.png](../assets/experiments/crack_mask_instance_example.png) | `비교/다운로드 (28).png` |

## 수치의 의미와 재현 범위

- `results/yolo11s_epoch_metrics.json`은 요약 캡처를 사람이 읽어 전사한 자료다. 원본 학습 CSV나 새로 수행한 평가 결과가 아니다.
- `final_epoch_summary.jpg`에서 마지막 epoch의 Box/Mask 수치를, `best_epoch_summary.jpg`에서 Mask mAP50-95가 최대인 행의 수치를 읽었다. `final_vs_best_summary.jpg`로 Mask 수치를 교차 확인했다.
- 실행명은 10·50·100 epochs이지만 캡처의 마지막 epoch 표기는 11·51·101이다. 최고 epoch 표기는 11·48·95이다. 원본 CSV와 추출 코드를 확보하기 전에는 이를 임의로 보정하지 않는다.
- 최고 Mask mAP50-95는 0.1538 / 0.2450 / 0.2453이다. 50→100의 절대 차이는 0.0003(0.03%p)이다. 반복 실행·분산·독립 테스트셋 결과가 없어 우위를 확정할 수 없다.
- YOLO11m 100-epoch 결과는 `m100 지표` 폴더의 그래프를 참고 자료로 보존했다. 정확한 숫자를 그래프에서 추정하여 정량 지표 파일에 넣지 않았다.
- 데이터 split, seed, 라이브러리 버전, 체크포인트와 각 이미지의 일대일 대응은 첨부만으로 완전히 확인되지 않는다. 저장소의 `weights/best.pt`가 이 표의 최고 모델이라는 주장은 하지 않는다.

## 정성 예시 해석

- epoch 비교 그림의 제목에는 `ChatGPT Image`가 포함된다. 생성 이미지일 가능성이 있는 데모로 구분하며, 실제 구조물의 현장 성능을 입증하는 자료로 쓰지 않는다. 원본 제목 겹침은 편집하지 않았다.
- 작은 분리 예측은 오검출 가능성을 보여주지만, 정답 마스크가 없으므로 해당 예시의 IoU·정확도는 계산하지 않았다.
- 마스크/인스턴스 그림의 5.3481%는 예측 픽셀 면적비다. 실제 면적, 손상률 또는 안전등급으로 해석할 수 없다. 연속 균열의 인스턴스 분할과 주변 예측은 수동 검수가 필요하다.
- 첨부 그림은 SAM 보정 전후 개선이나 YOLO26 성능 향상을 입증하지 않는다.

## 이번 반영에서 제외한 자료

- Roboflow 설치 TXT: API 키로 보이는 인증 문자열 포함. 원문과 키는 업로드하지 않았다.
- 외부 강의 PDF와 논문 목록 XLSX: 이번 실험 결과의 직접적인 근거가 아니므로 제외했다.
- 중복 이미지, 실행 연결이 불명확한 추가 곡선, 비슷한 개별 예측 이미지: 대표 사례만 선별했다.
- 첨부에는 학습 원본 `results.csv`, `args.yaml`, 실행 노트북과 모델 가중치가 없어 새 학습 코드나 모델을 추가하지 않았다.

## 다음 확인 항목

1. 각 실행의 원본 `results.csv`와 `args.yaml` 확보 및 epoch indexing 확인.
2. 동일 데이터 split/입력 크기/평가 설정으로 YOLO11s·YOLO11m·YOLO26 재평가.
3. 실제 현장 이미지와 생성/예시 이미지를 구분하고, 정답 마스크 기반 실패 사례 및 SAM 전후 지표 평가.

## 원본 이미지 SHA-256

- `assets/experiments/final_epoch_summary.jpg`: `40e5a372eeba81668f26d4283fb1e3d4fa1242b60622ce5edff982d50f7707f0`
- `assets/experiments/best_epoch_summary.jpg`: `fb7094f987670a2ff95ca75829e870c705778e7052f4483629ce8d22109e53f3`
- `assets/experiments/final_vs_best_summary.jpg`: `1f086d315edf209a4aa968ab77bef19b59862b5ed1bbbcbfd25747dfab850a58`
- `assets/experiments/yolo11s_epoch_training_curves.png`: `324bcc9c23667e645b2af637943be148e63b9650fe15a37a6b31843c345734dd`
- `assets/experiments/yolo11s_epoch_segmentation_loss.png`: `ce5a75950476ef463a67d5f79a368823b3c7173ee6943661a0918cbd8e9350ce`
- `assets/experiments/yolo11m_100epoch_training_curves.png`: `c85dd8a5d145aacd93f6c4566880b0ef65256f8c2a9e81e47ed2c3f7d4304beb`
- `assets/experiments/yolo11s_epoch_prediction_comparison.png`: `f01bf0d286993830d4707d9b78ec82d8a46cc598a5b659877bbec437f90342b7`
- `assets/experiments/crack_mask_instance_example.png`: `e89a33709065a034b787271c0d12f0e95ef6c5732baf94e276c6bea9ef6dbc29`
