# Classical Image Classification

루트 [README](../README.md)에서 `방법 1. 단순 이미지 분류`에 해당하는 보조 문서입니다.  
이 문서는 팀 프로젝트 최종 산출물 기준으로 정리했고, 전처리 조합을 탐구한 개인 실험 기록은 별도 폴더로 분리했습니다.

## PDF Archive

표지를 클릭하면 실제 PDF가 열립니다.

| 보고서 PDF | 발표자료 PDF |
| --- | --- |
| [![ML_1 report cover](../assets/images/ml1-report-cover.png)](<./ML_1(Image Classification  with Machine Learning).pdf>) | [![ML_1 presentation cover](../assets/images/ml1-ppt-cover.png)](<./ML_1(ppt).pdf>) |

## Focus

| 항목 | 내용 |
| --- | --- |
| 목적 | 주어진 이미지를 더 정확하게 분류하는 것 |
| 데이터 | `MNIST`, `SportsBall` |
| 핵심 기법 | `KNN`, `SVM`, `Random Forest`, `XGBoost`, `Sobel`, `PCA`, `HOG`, `ROI` |
| 핵심 포인트 | 모델 선택보다 입력 전처리와 특징 추출 설계의 영향 확인 |

## Personal Exploration Context

- SportsBall 전처리 과정의 효율성과 조합은 학교 수업에서 진행한 머신러닝 과목 프로젝트 안에서, 제가 직접 실험하며 검증한 내용입니다.
- 특히 `ROI 추출`, `전처리 순서 변경`, `HOG + 색상 히스토그램`, `XGBoost / Random Forest` 비교는 개인 실험 로그에 누적해 두고, 그 결과를 최종 프로젝트 파이프라인으로 정제했습니다.
- 관련 기록은 [`../ML_1(개인적으로 진행한 실험과정)/README.md`](<../ML_1(개인적으로 진행한 실험과정)/README.md>)에서 따로 볼 수 있습니다.

## What Was Implemented

### MNIST
- 기본 분류기 비교: `KNN`, `SVM`, `Random Forest`
- 특징 추출 비교: `Sobel`, `PCA`, `HOG`
- 숫자의 구조와 방향성을 얼마나 잘 보존하는지 중심으로 성능 비교

### SportsBall
- 객체 중심 분류를 위한 `ROI 추출` 파이프라인 구현
- contour 기반 영역 탐색과 bounding box crop 적용
- 데이터 증강, `k-fold cross validation`, `HOG`, 색상 히스토그램 조합 실험
- 최종적으로 `XGBoost` 기반 분류기로 평가

## Main Results

| 데이터 | 모델·특징 | 내부 검증 | 별도 테스트 |
| --- | --- | --- | --- |
| MNIST | KNN | 80.00% | 78.0% |
| MNIST | SVM | 86.67% | 86.0% |
| MNIST | Random Forest | 83.33% | 82.0% |
| MNIST | PCA + SVM | 86.67% | 84.0% |
| MNIST | HOG + KNN | 94.44% | 90.0% |
| MNIST | HOG + SVM | 95.56% | 94.0% |
| SportsBall | ROI, HOG, 색상 히스토그램 + XGBoost | 약 65% (교차검증) | 약 37% |

[팀 보고서](<./ML_1(Image Classification  with Machine Learning).pdf>) §3.2–3.3 기준입니다. MNIST에서는 학습 폴더 450장을 학습 360장과 검증 90장으로 나누고, 테스트 폴더의 50장으로 최종 평가했습니다. SportsBall에서는 배경을 공으로 잘못 잡은 ROI가 있었고, 교차검증 성능은 별도 테스트에서 유지되지 않았습니다.

## Evidence

![Classical image classification ROI evidence](../assets/images/classical-roi-evidence.png)

- 팀 실험에서 보존한 ROI 처리 예시입니다. ROI 실패 사례는 팀 보고서 §4.2에서 확인할 수 있습니다.
- 구성: `Original with ROI -> Processed Image -> Extracted ROI`
- 의미: 객체가 뚜렷한 경우와 배경이 복잡한 경우를 함께 보여 주어, SportsBall 분류에서 ROI 품질이 성능을 크게 좌우했다는 점을 시각적으로 확인할 수 있습니다.

## Why This Method Matters

- MNIST에서는 `HOG + SVM`이 가장 강하게 작동하며, 형태 중심 특징이 숫자 분류에 효과적이라는 점을 확인했습니다.
- SportsBall에서는 분류기보다 먼저 `공이 있는 영역을 얼마나 정확히 분리하느냐`가 성능을 좌우했습니다.
- 이 방법은 이후 표현학습 단계로 넘어가기 전, `좋은 표현의 출발점은 좋은 입력 설계`라는 기준선을 만들어 주었습니다.

## Related Files

- [`ML_1 최종 보고서 PDF`](<./ML_1(Image Classification  with Machine Learning).pdf>)
- [`ML_1 발표자료 PDF`](<./ML_1(ppt).pdf>)
- [`개인 실험 로그 README`](<../ML_1(개인적으로 진행한 실험과정)/README.md>)

## Summary

단순 이미지 분류 방법은 `입력을 잘 만드는 것`에 집중한 접근이었습니다.  
특히 SportsBall에서는 모델 교체보다 ROI 추출과 특징 조합이 더 큰 영향을 준다는 점이 분명하게 드러났습니다.
