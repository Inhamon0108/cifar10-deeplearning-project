# CIFAR-10 딥러닝 이미지 분류 코드 개선하기

## 과제 개요

- **과제명:** 내가 만든 딥러닝 (진짜로)
- **활용 데이터셋:** CIFAR-10
- **데이터:** 32×32 RGB 이미지 60,000장, 10개 클래스
- **프로젝트 목표:** 기본 CNN 모델에서 시작하여 모델 구조와 학습 조건을 단계적으로 변경, 각 실험 결과를 비교하여 CIFAR-10 이미지 분류 성능을 향상하기
- **기본 목표치:** Test Accuracy 80% 이상
- **최종 예상 목표치:** 85~90%

## 파일 안내

```text
cifar10-deeplearning-project/
├─ README.md
├─ CIFAR10_딥러닝실험.ipynb
├─ 실험기록.csv
├─ requirements.txt
├─ .gitignore
└─ 결과/
   └─ 실험결과.md
```

**Jupyter에서는 `CIFAR10_딥러닝실험.ipynb` 하나를 엽니다.** 그래프는 Notebook의 실행 출력에 남깁니다. `실험기록.csv`는 실험별 정확도와 설정을 빠르게 비교하는 표이며, 사람이 읽는 결과 요약과 메모는 `결과/실험결과.md` 하나에 계속 추가합니다.

## CNN-01: 초기 실습 모델

기본 코드로 실행한 최초 결과입니다.

| 항목 | 결과 |
| --- | --- |
| Dataset | CIFAR-10 |
| CNN 구조 | Conv2D 32 → MaxPooling → Conv2D 64 → MaxPooling → Conv2D 64 → Flatten → Dense 64 → Dense 10 |
| Optimizer | Adam |
| Epoch | 5 |
| Train Accuracy | 70.90% |
| Validation Accuracy | 67.13% |
| Test Accuracy | 67.13% |
| Train Loss | 0.8312 |
| Validation/Test Loss | 0.9522 |
| 판정 | 참고용 |

**CNN-01의 검증정확도와 테스트정확도 67.13%는 서로 독립된 평가 결과가 아닙니다.** 동일한 Test Set을 `validation_data`와 `evaluate`에 사용했습니다. 최초 실습 결과를 보존하는 참고용 기록이며, 최종 모델의 공정한 비교 기준으로 사용하지 않습니다.

## 실험 진행 방식

1. 초기 CNN 모델에서 시작
2. 한 번의 실험에서는 가능하면 한 가지 조건만 변경
3. Validation Accuracy와 Loss를 기준으로 성능 확인
4. 성능이 좋아진 설정은 가져가기
5. 성능이 떨어지거나 효과가 없는 설정은 제외
6. 효과가 확인된 설정을 최종 CNN 모델에 추가하기
7. 최종 모델이 결정된 후 Test Set을 평가
8. 마지막에 CNN, ViT, GPT 이미지 분류 결과를 비교

다음 **CNN-02 기준 모델**에서는 CNN 구조를 유지, Train 45,000장 / Validation 5,000장 / Test 10,000장으로 분리하기
모델 선정과 학습 조건 조정에는 Validation Set만 사용하고, Test Set은 최종 평가 전까지 사용 X

## 앞으로 진행할 실험

- Epoch 변경
- CNN Layer 구조 변경
- Filter 수 변경
- Dense 구조 변경
- Batch Size 변경
- Learning Rate 변경
- Batch Normalization 적용
- Dropout 적용
- Data Augmentation 적용
- Optimizer 비교
- Learning Rate Scheduler 적용

최종적으로 **Final CNN vs Vision Transformer(ViT) vs GPT 이미지 분류** 결과를 비교하기

| 실험번호 | 실험명 (예정) |
| --- | --- |
| CNN-01 | 초기 실습 모델 · 완료 |
| CNN-02 | 기준 모델 |
| CNN-03 | Epoch 변경 실험 |
| CNN-04 | CNN 구조 변경 실험 |
| CNN-05 | Batch Normalization 실험 |
| CNN-06 | Dropout 실험 |
| CNN-07 | Data Augmentation 실험 |
| CNN-08 | Learning Rate 실험 |
| CNN-09 | Optimizer 비교 실험 |
| FINAL | 최종 CNN 모델 |

실제 실험 순서는 진행하면서 변경될 수 있습니다.

## 실험 기록하기

실험마다 Notebook을 복사하지 않고 **`CIFAR10_딥러닝실험.ipynb` 하나를 계속 수정**합니다. 그래프는 실행 출력에 남기고 별도 PNG 파일이나 실험별 결과 폴더는 만들지 않습니다. 결과 설명과 메모는 **`결과/실험결과.md`** 아래에 새 실험 항목으로 추가합니다.

각 실험을 마치면 Notebook 저장 → `실험기록.csv`와 `결과/실험결과.md` 갱신 → commit → tag → GitHub push 순으로 기록합니다. CSV 정확도는 0~100의 백분율 숫자로 쓰고, 아직 평가하지 않은 Test Accuracy는 빈칸으로 둡니다.

CNN-01은 기존 EXP-01과 같은 최초 실험입니다. 당시 코드·출력·그래프는 기존 [EXP-01 tag](https://github.com/Inhamon0108/cifar10-deeplearning-project/tree/EXP-01)에 그대로 보존되어 있으며, 기존 commit과 tag는 변경하지 않습니다. 앞으로의 실험은 CNN-02, CNN-03처럼 기록합니다.

Commit 메시지는 실험명과 실제 측정 결과가 바로 보이도록 짧게 씁니다. 다음 점수는 형식을 설명하는 **예시**이며 새로 측정한 결과가 아닙니다.

```text
CNN-01 초기 실습 모델 - accuracy 67.13%
CNN-02 기준 모델 - val accuracy 68.42%
CNN-03 Epoch 변경 - val accuracy 72.31%
```

예를 들어 CNN-02를 완료한 후 프로젝트 터미널에서:

```text
git add CIFAR10_딥러닝실험.ipynb 실험기록.csv 결과/실험결과.md
git commit -m "CNN-02 기준 모델 - val accuracy 68.42%"
git tag -a CNN-02 -m "CNN-02 기준 모델"
git push origin main
git push origin CNN-02
```

실제 점수로 바꿔 기록합니다. 이번 결과 기록 방식 정리는 실험이 아니므로 새 실험 tag를 만들지 않습니다. Python 코드의 변수명과 함수명은 영어로 유지합니다.

## 실행 환경과 저장 경로

현재 사용하는 Anaconda의 `jupyter-local` 환경을 그대로 사용합니다. `requirements.txt`의 TensorFlow, JupyterLab, NumPy, Matplotlib, pandas, scikit-learn 호환 범위를 유지했고, 이번 작업에서는 패키지를 업그레이드하거나 재설치하지 않았습니다. 현재 TensorFlow 의존성에는 Python 3.14용 RC 버전이 포함되어 있습니다.

Notebook을 실행할 작업 폴더는 `C:/Users/lhb29/Documents/JupyterProjects/cifar10-deeplearning-project`입니다. 데이터셋과 대용량 모델 weight는 `.gitignore`로 제외하고 로컬에만 보관합니다.

이번 정리에서는 Notebook 코드와 실행 출력을 그대로 보존했습니다. 다음 실험에서는 과거의 파일 저장 셀을 별도 이미지 저장 없이 그래프를 Notebook 출력에 남기는 방식으로 정리합니다.