---
title: "일반버스·도시철도 O/D 데이터 활용 (1) - 판다스를 활용한 데이터 전처리 (2021.11.16)"
excerpt: "안녕하세요!  이번 포스트는 셀레니움으로 다운받은 데이터를 전처리해보려고 합니다!"

categories:
  - Blog
tags:
  - [kepler.gl, python, visualization]

toc: true
toc_sticky: true

date: 2021-11-15
last_modified_at: 2021-11-15
---

안녕하세요! 이번 포스트는 셀레니움으로 다운받은 데이터를 ([파이썬 셀레니움(Selenium)을 이용하여 데이터 자동 수집](https://github.com/everlast0430/cityBusRailOD-python-selenium/blob/main/202101.csv)) 전처리해보려고 합니다!

아래 테이블에서 보이듯이 다운받은 데이터는 간단한 기초통계가 가능하겠지만, 여기선 지도상에 표현을 할 수 있는 데이터로 전처리 하고자합니다!!

![image](https://user-images.githubusercontent.com/43924464/141873219-8a5f89d8-1e11-45a7-85d0-1e09edeb8210.png)

## 체크 사항

- Geopandas 설치
- Jupyter Lab (설정에서는 필수사항은 아니지만 kepler.gl을 쓰려면 미리 써두는게 편하다.)

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

### 1.2. 출·도착이 서울인 곳만 추출

데이터 자체가 지도상에 표현을 하려고하니 애매한점이 많았다. 물론 다른 데이터를 쓰면 되겠지만 다른 사람들이 활용하지 않았던 데이터를 이용하는 것에 의미를 두고 싶어서 였다.

그래서 편의상 서울내에서 이동한 데이터만 이용하려고 한다..

이어지는 코드는 다음과 같다.

```python
sig_df = new_df.groupby(['일자' ,'시도(출발)', '시군구(출발)', '시도(도착)', '시군구(도착)'], as_index=False).mean()
sig_seoul_df = sig_df[(sig_df['시도(출발)'].str.contains('서울')) &
                      (sig_df['시도(도착)'].str.contains('서울'))]
sig_seoul_df
```

로드된 서울내 OD 데이터 ⬇️

![image](https://user-images.githubusercontent.com/43924464/141880200-85c0d179-2579-4784-bc68-6cc696e9886b.png)

## 2. 데이터 O/D 포인트 만들기

좌표계 설정 포스트에서 다뤘던 내용대로 [여기](http://www.gisdeveloper.co.kr/?p=2332)에서 시군구 데이터를 다운받아 O/D point를 만들려고 한다!

### 2.1. 시군구 shp 로드 및 중심점 생성

기본적인 설정을 한 후, 'centroid' 함수를 활용하여 각 구별 중심점을 구한다.

```python
c = fiona.open('../data/SIG_202101/TL_SCCO_SIG.shp', encoding='euc-kr')
polycode_df = gpd.GeoDataFrame.from_features(c, crs=c.crs).to_crs('epsg:4326')
polycode_df['centroid'] = polycode_df['geometry'].centroid
polycode_df.head()
```

shp 파일의 각 구별 중심점 생성 ⬇️

![image](https://user-images.githubusercontent.com/43924464/141880410-ae7028ac-3605-49a0-8d05-17bce673adad.png)

### 2.2. 서울인 곳만 추출

그리고나서 '시도코드'의 앞 두자리가 11인 곳만 추출하면 된다.

```python
polycode_df['시도코드'] = polycode_df['SIG_CD'].str.slice(start=0, stop=2)
polycode_seoul_df = polycode_df[polycode_df['시도코드'] == '11']
polycode_seoul_df.head()
```

shp 파일의 서울만 ⬇️

![image](https://user-images.githubusercontent.com/43924464/141880114-d700a35c-effe-453b-859e-d54832a3af84.png)

### Unique 값 확인

두 테이블을 매칭하기전 unique 값을 확인하려 한다. 원래 첫 OD테이블에 시군구코드가 있다면 이런 과정이 필요없었을 것이다.

위에서 말씀드린봐아 같이, 애매한 부분이 너무 많았다 😥 그래도 이 데이터를 지도상에 활용하고 싶었다..

다음과 같이하면 서로 유일값이 같은지 확인할 수 있다.

```python
# key값이 같은지 확인

print(len(set(sig_seoul_df['시군구(출발)'].unique().tolist())
	    - set(polycode_seoul_df['SIG_KOR_NM'].unique().tolist())))

print(len(set(sig_seoul_df['시군구(도착)'].unique().tolist())
        - set(polycode_seoul_df['SIG_KOR_NM'].unique().tolist())))
```

## 3. 최종 테이블 만들기

### 3.1. 두 테이블 합치기

```python
final_df = pd.merge(sig_seoul_df ,polycode_seoul_df[['SIG_KOR_NM', 'centroid']], left_on='시군구(출발)', right_on='SIG_KOR_NM', how='left')
final_df = pd.merge(final_df ,polycode_seoul_df[['SIG_KOR_NM', 'centroid']], left_on='시군구(도착)', right_on='SIG_KOR_NM', how='left')
final_df = final_df.drop(columns=['SIG_KOR_NM_x', 'SIG_KOR_NM_y'])
```

합친 결과 ⬇️

![image](https://user-images.githubusercontent.com/43924464/141881281-1ed21777-0833-4307-ab58-97cec1016ec4.png)

### 3.2. 일자 수정

kepler.gl 에서는 다음과같은 형식의 날짜 데이터를 인식한다.

![image](https://user-images.githubusercontent.com/43924464/141870912-7dc88d33-fa8c-4a1d-908d-a49a01467b85.png)

형식을 바꿔주자!

```python
# 날짜 데이터 형식 수정

final_df['일자'] = final_df['일자'].str.slice(start=0, stop=-3)
final_df.head()
```

최종 데이터 테이블 ⬇️

![image](https://user-images.githubusercontent.com/43924464/141881330-e5f5bddc-3021-40ac-80a0-8d8bc7589ce5.png)
