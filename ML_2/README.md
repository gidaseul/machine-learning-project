# Representation Learning

루트 [README](../README.md)에서 `방법 2. 표현학습`에 해당하는 보조 문서입니다.  
이 프로젝트는 전처리와 특징 조합만으로는 설명하기 어려운 SportsBall 데이터의 복잡성을, `latent representation` 관점에서 다시 해석하려는 시도였습니다.

## PDF Archive

표지를 클릭하면 실제 PDF가 열립니다.

| 보고서 PDF | 발표자료 PDF |
| --- | --- |
| [![ML_2 report cover](../assets/images/ml2-report-cover.png)](<./ML프로젝트2_02팀/ML_2(Image Classification  with Representation Learning).pdf>) | [![ML_2 presentation cover](../assets/images/ml2-ppt-cover.png)](<./ML프로젝트2_02팀/ML_2(ppt).pdf>) |

## Focus

| 항목 | 내용 |
| --- | --- |
| 목적 | 모델이 데이터를 어떤 공간에 어떻게 표현하는지 이해하는 것 |
| 데이터 | `MNIST`, `SportsBall` |
| 핵심 기법 | `Base CNN Encoder`, `Autoencoder`, `Projection Head`, `Contrastive Learning`, `InfoNCE`, `t-SNE`, `Heatmap` |
| 핵심 포인트 | 성능 향상뿐 아니라 표현 공간이 실제로 어떻게 형성되는지 해석 |

## Why This Project Started

- `ML_1`에서 전처리와 특징 조합을 직접 탐구한 결과, SportsBall 데이터는 입력 정제만으로 해결하기 어려운 한계가 있다는 점이 분명해졌습니다.
- 그래서 다음 단계에서는 `입력을 더 잘 만드는 문제`를 넘어서, `모델 내부 표현이 클래스 구조를 얼마나 잘 분리하는가`를 보는 방향으로 전환했습니다.

## What Was Implemented

- `Base CNN Encoder`를 설계해 특징 표현 학습
- `Autoencoder`를 적용해 latent representation과 복원 성능 확인
- `Projection Head`를 추가해 contrastive learning 실험
- `InfoNCE` 기반 학습으로 같은 클래스는 가깝게, 다른 클래스는 멀어지게 만드는 구조 시도
- `t-SNE`로 train/test latent space 분포 시각화
- Heatmap으로 모델이 실제로 주목한 영역 해석

## Main Results

| 구분 | 결과 |
| --- | --- |
| MNIST | Train Accuracy `99.56%` |
| MNIST | Test Accuracy `98.00%` |
| SportsBall | Train Accuracy `70.90%` |
| SportsBall | Base model Test Accuracy `48.00%` |
| SportsBall | Autoencoder 기반 표현학습 실험에서는 테스트 정확도 `10%` 수준 구간 확인 |
| SportsBall | 최종 분석에서 성능이 좋지 않았던 contrastive learning 실험 결과 `9.00%` 비교 |

## Evidence

![Representation learning clustering and latent-space evidence](../assets/images/representation-latent-evidence.png)

아래쪽 두 t-SNE 패널은 CNN 중간 특징 벡터를 2차원으로 줄이고 정답 클래스에 따라 색을 입힌 것입니다. SportsBall에서 클래스가 겹치는 구간을 정성적으로 비교했습니다. 분리의 정도는 분류 정확도나 표현학습의 개선을 입증하는 수치로 사용할 수 없습니다.

[MNIST 특징 추출 및 시각화](../mnist/mnist_latent_feature.ipynb) · [SportsBall 특징 추출 및 시각화](../sportsball/sportsball_latent_feature.ipynb)

## Why This Method Matters

- MNIST처럼 구조가 단순한 데이터는 표현학습이 안정적으로 작동했습니다.
- SportsBall처럼 배경 잡음이 많고 객체가 복잡한 데이터는, 표현학습만으로 바로 성능이 좋아지지 않았습니다.
- 이 단계의 가장 큰 의미는 "왜 성능이 낮았는가"를 latent space와 heatmap으로 설명한 점입니다.
- 결과적으로 표현학습은 항상 성능 향상으로 이어지는 것이 아니라, 데이터 구조와 학습 목표가 잘 맞아야 한다는 점을 확인했습니다.

## Related Files

- [`ML_2 최종 보고서 PDF`](<./ML프로젝트2_02팀/ML_2(Image Classification  with Representation Learning).pdf>)
- [`ML_2 발표자료 PDF`](<./ML프로젝트2_02팀/ML_2(ppt).pdf>)
- [SportsBall 대조학습 실험](../sportsball/vae_latent.ipynb)

## Notes Before Reproducing

- `datas/MNIST`에는 학습 450장·테스트 50장, `datas/SportsBall`에는 학습 1,000장·테스트 100장이 있습니다. 위 노트북의 `train_dir`, `test_dir`을 체크아웃한 저장소의 해당 `train`, `test` 폴더로 바꿉니다. SportsBall 체크포인트 경로도 해당 노트북의 위치에 맞게 지정합니다.
- 공개 SportsBall 표현 노트북에는 클래스 라벨과 유클리드 거리를 쓰는 대조 손실, 분류 손실, 복원 손실을 함께 쓴 실험이 있습니다. 팀 보고서 §3.2의 InfoNCE 실험과 구분해 읽어야 합니다. `9%`는 팀 보고서의 해당 실행 기록입니다.
- CNN 노트북의 일부 셀은 테스트 손실을 매 epoch 확인하고 최적 체크포인트 선택에도 사용합니다. 당시 테스트 수치는 실험 과정의 기록으로 읽고, 다시 비교할 때는 학습에서 검증 세트를 분리한 뒤 테스트는 마지막에 한 번만 평가합니다.

## Summary

표현학습 방법은 단순히 정확도를 높이기 위한 실험이 아니라,  
`latent space가 어떻게 형성되는지`, `왜 어떤 경우에는 잘 작동하지 않는지`를 해석하기 위한 접근이었습니다.
