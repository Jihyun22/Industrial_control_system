## data scaling

데이터 셋에 맞는 스케일러 찾기



## **1. 스케일러 성능비교**

- Standard Scaler 

```
**%**time
MODEL.train()
BEST_MODEL, LOSS_HISTORY **=** train(HAI_DATASET_TRAIN, MODEL, BATCH_SIZE, 32)
Wall time: 0 ns
```

training: 100%

32/32 [48:47<49:38, 93.09s/it, loss: 9.257446]

- Robust Scaler

```
**%**time
MODEL.train()
BEST_MODEL, LOSS_HISTORY **=** train(HAI_DATASET_TRAIN, MODEL, BATCH_SIZE, 32)
Wall time: 0 ns
```

training: 100%

32/32 [48:24<00:00, 90.75s/it, loss: 26.189287]

- **MinMax Scaler** : 이상치를 공격이라 간주.. 따라서 train 셋의 min max를 기준으로 이를 벗어나는 테스트 데이터 셋을 검증할 것.

```
**%**time
MODEL.train()
BEST_MODEL, LOSS_HISTORY **=** train(HAI_DATASET_TRAIN, MODEL, BATCH_SIZE, 32)
Wall time: 0 ns
```

training: 100%

32/32 [48:27<00:00, 90.85s/it, loss: 0.087259]

- 사이킷런의 minmax scaler vs 베이스라인의 정규화 방법의 차이

- - 코드

```
from sklearn.preprocessing import MinMaxScaler
def scaled(df, fit_df):
  scaler=MinMaxScaler()
  scaler.fit(fit_df)
  scaled_df=scaler.transform(df)
  return scaled_df
TRAIN_DF_Mm=scaled(TRAIN_DF_RAW[VALID_COLUMNS_IN_TRAIN_DATASET], TRAIN_DF_RAW[VALID_COLUMNS_IN_TRAIN_DATASET])
def normalize(df):
  ndf = df.copy()
  for c in df.columns:
    if TAG_MIN[c] == TAG_MAX[c]:
      ndf[c] = df[c] - TAG_MIN[c]
    else:
      ndf[c] = (df[c] - TAG_MIN[c]) / (TAG_MAX[c] - TAG_MIN[c])
  return ndf
TRAIN_DF = normalize(TRAIN_DF_RAW[VALID_COLUMNS_IN_TRAIN_DATASET]).ewm(alpha=0.9).mean()
TRAIN_DF
```

- - ewm(alpha=0.9) : 지수 가중치 ([링크](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.ewm.html) 참고)
  - 가장 큰 차이점 : 값이 변하지 않는 칼럼을 0으로 바꾸느냐의 차이

*최종수정일: 20-08-31*