---
title: "일반버스·도시철도 O/D 데이터 활용 - 전처리부터 kepler.gl 시각화까지"
excerpt: "안녕하세요!  이번 포스트는 셀레니움으로 다운받은 데이터를 전처리하고 시각화해 보려고 합니다!"

categories:
  - Blog
tags:
  - [kepler.gl, python, visualization]

toc: true
toc_sticky: true

date: 2021-11-17
last_modified_at: 2021-11-17
---

안녕하세요! 이번 포스트는 셀레니움으로 다운받은 데이터를 ([파이썬 셀레니움(Selenium)을 이용하여 데이터 자동 수집](https://github.com/everlast0430/cityBusRailOD-python-selenium/blob/main/202101.csv)) 전처리하고 시각화해 보려고 합니다!

아래 테이블에서 보이듯이 다운받은 데이터는 간단한 기초통계가 가능합니다.. 하지만 아쉽게도 시도 및 시군구 코드가 없어 활용에 어려움이 있습니다.

![image](https://user-images.githubusercontent.com/43924464/141873219-8a5f89d8-1e11-45a7-85d0-1e09edeb8210.png)

## 체크 사항

- Geopandas 설치

## 라이브러리 import

```python
import fiona
import pandas as pd
import geopandas as gpd
import time

import warnings
warnings.filterwarnings('ignore')
```

## 1. 데이터 추출

### 1.1. 파일 로드 및 null값 처리

먼저 파일을 로드하고 출발지가 특정되지 않은 곳을 제거한다.

```python
# 21년 01월 파일 로드 및 null값 제거

df = pd.read_csv('./data/202101.csv', encoding='CP949')
idx_remove = df[(df['시도(출발)'] == '-') | (df['시군구(출발)'] == '-')].index
new_df = df.drop(idx_remove)
new_df.head()
```

로드된 데이터 ⬇️

![image](https://user-images.githubusercontent.com/43924464/141878247-56780021-bf75-4eca-80c6-7e0ccad68f6a.png)

### 1.2. 시도 단위 추출

데이터 자체가 지도상에 표현을 하려고하니 애매한점이 많았다. 물론 다른 데이터를 쓰면 되겠지만 다른 사람들이 활용하지 않았던 데이터를 이용하는 것에 의미를 두고 싶어서 였다.

그래서 편의상 시도단위로 추출하고 평균을 구했다

코드는 다음과 같다.

```python
sig_df = new_df.groupby(['일자' ,'시도(출발)', '시군구(출발)', '시도(도착)', '시군구(도착)'], as_index=False).mean()
sig_seoul_df = sig_df[(sig_df['시도(출발)'].str.contains('서울')) &
                      (sig_df['시도(도착)'].str.contains('서울'))]
sig_seoul_df
```

로드된 서울내 OD 데이터 ⬇️

![image](https://user-images.githubusercontent.com/43924464/142096638-82c2bb68-4e0f-42ea-a428-c9593deca86f.png)

## 2. 데이터 polygon 만들기

좌표계 설정 포스트에서 다뤘던 내용대로 [여기](http://www.gisdeveloper.co.kr/?p=2332)에서 시군구 데이터를 다운받아 polygon을 만들려고 한다!

### 2.1. 시도 shp 로드

기본적인 설정을 한 후, 시도 shp파일을 로드한다. 파일은 [여기](http://www.gisdeveloper.co.kr/?p=2332)서 받을 수 있다.

코드는 다음과 같다.

```python
c = fiona.open('../data/CTPRVN_202101/TL_SCCO_CTPRVN.shp', encoding='euc-kr')
polycode_df = gpd.GeoDataFrame.from_features(c, crs=c.crs).to_crs('epsg:4326')
polycode_df.head()
```

shp 파일 ⬇️

![image](https://user-images.githubusercontent.com/43924464/141880410-ae7028ac-3605-49a0-8d05-17bce673adad.png)

### 2.2. 시도 unique 체크

통행량정보가 있는 테이블과 shp 테이블간의 key값에 중복이 없는지 체크하기위해 다음과 같이 코드를 만들었다. 0 이상또는 이하면 중복된 값이 있다고 보면된다.

```python
print(len(set(sido_df['시도(출발)'].unique().tolist()))-len(set(polycode_df['CTP_KOR_NM'].unique().tolist())))
print(len(set(sido_df['시도(도착)'].unique().tolist()))-len(set(polycode_df['CTP_KOR_NM'].unique().tolist())))
```

## 3. 최종 테이블 만들기

### 3.1. 두 테이블 합치기

```python
final_df = pd.merge(sido_df ,polycode_df[['CTP_KOR_NM', 'geometry']], left_on='시도(출발)', right_on='CTP_KOR_NM', how='left')
final_df = final_df.drop(columns=['CTP_KOR_NM'])
final_df.head()
```

합친 결과 ⬇️

![image](https://user-images.githubusercontent.com/43924464/142098702-8802df33-42d0-4b1d-9ffd-aaea3346fd0c.png)

### 3.2. 일자 수정

kepler.gl 에서는 다음과같은 형식의 날짜 데이터를 인식한다.

![image](https://user-images.githubusercontent.com/43924464/141870912-7dc88d33-fa8c-4a1d-908d-a49a01467b85.png)

형식을 바꿔주자!

```python
# 날짜 데이터 형식 수정
final_df['일자'] = final_df['일자'].str.slice(start=0, stop=-3)
final_df['일자'] = pd.to_datetime(final_df['일자'])
final_df['일자'] = final_df['일자'].dt.strftime("%Y-%m-%d %ss")
final_df.head()
```

최종 데이터 테이블 ⬇️

![image](https://user-images.githubusercontent.com/43924464/142100300-8b7dae6c-0f91-4f28-a41c-2a025ddac509.png)

### 3.3. 데이터 저장

로컬파일을 이용하는 경우에 250MB를 초과할 수 없다. 그래서 일주일치만 저장하였다.

```python
final_df[:119].to_csv('202101_sido_final.csv', encoding='utf-8', index=False)
```

## 4. 시각화

웹으로 간단하게 local에서 파일을 업로드하여 시각화해 볼 수 있다. [https://kepler.gl/demo/](https://kepler.gl/demo/)에서 할 수 있다.

파일을 업로드하고 layers에서 아래그림과 같이 설정하였다.
또한 깔대기 모양의 filters에서 time('일자')로 timestamp를 만들었고 Base map에서 약간의 설정을 하였다.

![image](https://user-images.githubusercontent.com/43924464/142110286-1f3f8999-4554-48f0-a424-46d0c93b32dd.png)
