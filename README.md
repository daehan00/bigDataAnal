# 제10회 빅데이터 분석기사 실기 공부

thanks to[퇴근후딴짓](https://github.com/lovedlim/BigDataCertificationCourses)님

### 공부 내용
 - 유형별 기출문제, 예상문제들 풀이
 - 다양한 예시 상황에서 응용 코드 정리
 - 데이터분석 라이브러리 용법 정리
 - 꼼수

### 구조
 - 기출
 - 예시
 - 꼼수
 - requirements.txt
   - 실제 환경에서 제공되는 라이브러리 목록(환경 구성용)
 - .gitignore

# 빅데이터분석기사 실기 10회 후기(2025.06.21)

시험시간이 딱 되어서 문제를 보니까 생각보다 많아서 좀 당황했습니다;; 총 3시간 중 1시간반 이후부터 퇴실할 수 있고, 알고있던거랑은 다르게 출력값을 복붙할 수가 없었습니다(?!!?!?!??) 귀찮긴 했지만 보통 3자리 반올림이라 네글자 그냥 타이핑해서 메모장에 넣고 그걸로 복붙했네요,,, 막히거나 이름이 잘 기억이 안날때 dir, help 프린트해서 찾아서 풀어도 1시간 30분이면 충분히 다 풀고 검토까지 가능할 듯 합니다(본인은 시험장 전체 금메달).

---

## 유형 1
- **핵심 연산**: `groupby`, `dropna`, `fillna`, 평균/분산 계산
- **자주 나오는 흐름**
  1. 유형별 갯수/총합 계산 (`groupby`) 및 순위 데이터 출력
  2. 결측치 평균 대체 / 결측 데이터 drop
  3. 평균값 기반 필터링 등 조건 처리

**예시 코드**
```python
total = df.groupby(df["유형"])["정답"].count()
right = df[df["정답"] == 1].groupby(df["유형"])["정답"].count()
result = right / total
result.sort_values()
print(result.iloc[len(result)-1]
```
## 유형 2: 회귀
 - **풀이 흐름**
   1. 범주형/수치형 변수 분리 및 전처리 
   2. StandardScaler로 수치형 변수 스케일링 
   3. get_dummies() 등으로 범주형 인코딩 
   4. train_test_split()로 학습/검증 분리 
   5. 모델 학습: AdaBoost, GradientBoosting, LGBM 등
   6. RMSE로 성능 평가
   7. 예측 결과 .to_csv()로 저장
- **특징**
  - 결측치 없었음
  - 대부분의 변수가 수치형이었음
  - 큰 특징이 없는것이 특징, 템플릿 활용하면 충분
  - 분류 아니면 회귀가 나오는데 이번에는 회귀(import시에 regressor냐 classifier냐 차이,,ㅎ)

## 유형 3: 회귀분석 (로지스틱 & 선형회귀)
- **로지스틱 회귀**
  1. 적합 후 회귀계수와 p-value 확인
  2. 오즈비 계산
  3. 새로운 관측치 예측
**예시 코드**
```python
from statsmodels.api import Logit
import pandas as pd
import numpy as np

formula = "종속변수 ~ 성별 + 나이 + 직업"
model = Logit.from_formula(formula, df).fit()
print(model.summary())

odds = np.exp(model.params["나이"]) 
print(round(odds, 3))

data = pd.DataFrame({"나이":[40], "성별":[0], "직업":["의사"]})
pred = model.predict(data)
print(round(pred, 3))
```

- **다중선형회귀**
  1. 적합 후 유효 변수 추출
  2. 유효한 변수로 재적합
  3. 새로운 관측치 예측