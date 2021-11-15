---
title: "좌표계 설정 - Kepler.gl, Python (2021.11.15)"
excerpt: "안녕하세요!  이번 포스트는 Kepler.gl에서 쓰이는 좌표계를 파이썬으로 설정하는 방법을 설명하려고 합니다."

categories:
  - Blog
tags:
  - [kepler.gl, python, visualization]

toc: true
toc_sticky: true

date: 2021-11-15
last_modified_at: 2021-11-15
---

## 체크 사항

- Geopandas 설치
- Jupyter Lab (설정에서는 필수사항은 아니지만 kepler.gl을 쓰려면 미리 써두는게 편하다.)

## Kepler.gl 에서 지원하는 좌표계

먼저 좌표계를 변환하기 전에,Kepler.gl 홈페이지에서 지원되는 좌표 시스템을 알아보니 WGS84 경위도로 표시되어야 한다고 나와있다.

![image](https://user-images.githubusercontent.com/43924464/141422517-d00636b7-5012-46c6-96be-b4f17321095c.png)

즉 다음과 같이 바꾸면 될 것 같다!

     WGS84(EPSG:4326): GPS가 사용하는 좌표계(경도와 위도)


## 좌표계 설정파일 다운로드

다운로드 주소 : [http://www.gisdeveloper.co.kr/?p=2332](http://www.gisdeveloper.co.kr/?p=2332)

나는 시군구로 작업을 할 예정이라서 시군구의 최신 업데이트 파일을 받았다.

## 좌표계 설정 방법

먼저, 일반적으로 shp파일을 불러오면 다음과 같다.

![image](https://user-images.githubusercontent.com/43924464/141706125-b2ec2575-9df7-4c55-93f7-42070332e153.png)

이를 이제 fiona로 좌표계 정보가 들어있는 **prj파일**과 **crs**를 이용해서 좌표계를 설정해준다.

좌표계 설정코드는 다음과 같다.

```python
c = fiona.open('../data/SIG_202101/TL_SCCO_SIG.shp', encoding='euc-kr') # 자동으로 prj파일을 읽어온다.
polycode_df = gpd.GeoDataFrame.from_features(c, crs=c.crs).to_crs('epsg:4326') # 좌표계 정보를 읽어서 'epsg: 4326'으로 설정
polycode_df
```

제대로 설정이 되었다면, 다음과같이 나온다!!

![image](https://user-images.githubusercontent.com/43924464/141706415-d66f7cd4-a722-4522-98e8-27b55a993355.png)
