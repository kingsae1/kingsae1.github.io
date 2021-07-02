---
title: Text Sentiment Analysis (텍스트 감정 분석)
date: 2020-10-06 23:30:09
layout: post
category: data analysis
tags:
  - pandas
  - deep learning
  - machine learning
  - Text Sentiment
paginate: true
author: kingsae
image: https://emrahmete.files.wordpress.com/2018/11/nlp-833x474.png?w=404&h=230
---

지난번 Web Crawling으로 얻어온 영화 리뷰 데이터를 바탕으로 머신러닝을 수행해보도록 하자
영화 리뷰 100개의 데이터로 머신러닝을 한다라는 건 매우 적은 데이터이기에 실제 서비스에 사용하는데에는 무리가 있다. 지금 하는 예제는 텍스트를 어떤식으로 분석하고 이를 학습시키는지 관련 플로우를 배우고자 함에 있기에 추후 서비스를 기획한다면, 더많은 빅데이터 기반으로 정확도가 높은 알고리즘으로 구현하길 바란다

# Python을 이용한 텍스트 감정 분석

## 데이터 처리 Flow

데이터를 처리하기 위해 아래와 같은 Flow를 이용하여 학습한다

1.  데이터 수집
    -> 데이터 확인 및 탐색
    -> 데이터 전처리 / 정제
2.  데이터 모델링 / 학습
3.  데이터 평가
4.  배포

배포 후 다시 1.데이터 수집부터 시작하여 연속적인 데이터들에 대해서 학습된다

## Import 라이브러리

```python
import scipy as sp
import pandas as pd
import numpy as np

from konlpy.tag import Okt; okt = Okt()
import pickle
import math

from time import time
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import Pipeline
from sklearn.metrics import accuracy_score
from sklearn.model_selection import train_test_split
```

## 데이터 전처리 과정

데이터를 좀 더 잘이용하기 위해서는 전처리 즉, 불필요한 부분을 제거하고 사용할 데이터들을 정제하는 과정이 필요하다
Step1에서는 크롤링한 데이터를 이용해 Null값을 제거한 후, 실제 Train데이터와 Test데이터를 Load하여 Return해주는 역할을 한다

```python
def star_preprocessing (value) :
    print("Value = ",value)
    if value >= 3 :
        return '0'
    else :
        return '1'

def remove_stopword (text):
    stopword = ['의','가','은','수','들','더','는','잘','강', '이', '을', '를',
            '과','를','으로','에게','자','에','와','한','하다', '로', '으로',
            '.','..', '...', '?','\n', '^', '에서', '!']


def step1_data_preprocessing () :
    df = pd.read_excel('data/review.xlsx')
    df.loc[df.content.isnull()]

    _data = df.dropna(how='any')
    _star = []

    for score in _data.score :
        _star.append(star_preprocessing(score))

    _star = pd.DataFrame({'score' : _star})
    _length = math.floor(len(_data)/2)

    text_train = _data.content.iloc[:_length].values
    text_test = _data.content.iloc[_length:].values
    star_train = _star.score.iloc[:_length].values
    star_test = _star.score.iloc[_length:].values

    return text_train, text_test, star_train, star_test
```

## 데이터 학습 과정

Step2에서는 위에서 얻은 데이터를 가지고 데이터를 학습시키고 모델링 작업을 진행한다.

- 형태소 : 형태소 (morpheme)는 언어학에서 (일반적인 정의를 따르면) 일정한 의미가 있는 가장 작은 말의 단위
- dropna : column내에 NaN이 존재하면 삭제

```python
def tokenizer(text) :
    okt = Okt()
    return okt.morphs(text)

def step2_learning (X_train, X_test, Y_train, Y_test) :
    tfidf = TfidfVectorizer(lowercase=False, tokenizer=tokenizer)

    logistic = LogisticRegression(C=10.0, penalty='l2', random_state=0)
    pipe = Pipeline([('vect', tfidf), ('clf', logistic)])

    stime = time()

    pipe.fit(X_train, Y_train)

    y_pred = pipe.predict(X_test)

    print('### 테스트 종료 : 소요시간 [%d]초' %(time()-stime))
    print('### 테스트 정확도 : %.3f' %accuracy_score(Y_test, y_pred))

    with open('data/pipe.txt','wb') as fp:
        pickle.dump(pipe, fp)

    print('### 저장 완료')
```

해당 포스트에서는 TFIDF 단어 빈도수-역 문서 빈도수(term frequency-inverse document frequency)라 불리는 기법을 이용한다. 간단하게 말해 TFIDF는 형태소로 구분된 단어들에게 빈도수에 맞게 구분되고 이를 이용하는 기법이다.
또한 학습알고리즘으로 LogisticRegression를 이용하였는데, 보통 범주형(이산형)의 형태로 데이터 모델이 존재하는 경우 LogisticRegression을 이용한다고 한다.

- penalty : 페널티를 부여할 때 사용할 기준을 결정('l1', 'l2')
- dual : Dual Formulation인지 Primal Formulation인지를 결정(True, False)
- tol : 중지 기준에 대한 허용 오차 값
- C : 규칙의 강도의 역수 값
- fit_intercept : 의사 결정 기능에 상수를 추가할 지 여부 결정(True, False)
- intercept_scaling :
- class_weight : 클래스에 대한 가중치들의 값
- random_state : 데이터를 섞을 때 사용하는 랜덤 번호 생성기의 시드 값
- solver : 최적화에 사용할 알고리즘 결정('newton-cg', 'lbfgs', 'liblinear', 'sag', 'saga')
- max_iter : solver가 수렴하게 만드는 최대 반복 횟수 값
- multi_class : ('ovr', 'multinomial')
- verbose :
- warm_start : 이전 호출에 사용했던 solution을 재사용 할지 여부 결정(True, False)
- n_jobs : 병렬처리 시 사용 할 CPU 코어의 수

> LinearRegression는 종속 변수와 독립 변수 사이의 관계를 설정하는 데 사용되며(이는 독립 변수가 변경되는 경우 결과 종속 변수를 추정하는 데 유용), 반면에 LogisticRegression는 사건의 확률을 확인하는 데 사용됩니다

## 데이터 평가

learning()을 실행 후, 데이터를 학습시킨다. 그 이후에 using() 을 이용해 추가로 들어오는 데이터들을 평가한다

```python
def step3_using_model():
    with open('data/pipe.txt', 'rb') as fp:
        pipe = pickle.load(fp)

    import numpy as np

    while True:
        text = input('리뷰를 작성해주세요: ')
        str = [text]

        if str != '' :
            # 예측 정확도
            r1 = np.max(pipe.predict_proba(str)* 100)
            # 예측 결과
            r2 = pipe.predict(str)[0]

            print ('### 입력된 리뷰 :' , str)

            if r2 == '1' :
                print('긍정적인 리뷰')
            else :
                print('부정적인 리뷰')

            print('### 정확도 : %.3f' %r1)


def learning():
    print('### 머신러닝 시작 ####')
    text_train, text_test, star_train, star_test = step1_data_preprocessing()
    print('### 머신러닝 종료 ####')
    return step2_learning(text_train, text_test, star_train, star_test)

def using():
    step3_using_model()

```

```
    ### 입력된 리뷰 : ['재미있다']
    긍정적인 리뷰
    ### 정확도 : 56.319
    ### 입력된 리뷰 : ['노잼']
    부정적인 리뷰
    ### 정확도 : 88.730
    ### 입력된 리뷰 : ['ㅋㅋㅋ']
    긍정적인 리뷰
    ### 정확도 : 65.737
    ### 입력된 리뷰 : ['별로다']
    부정적인 리뷰
    ### 정확도 : 88.186
```

> 공유 : https://github.com/kingsae1/python-textsentment-analysis
