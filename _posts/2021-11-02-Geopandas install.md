---
title: "Geopandas 설치하는 방법 (2021.11.02)"
excerpt: "안녕하세요! 이번 포스트는 Geopandas 설치시 버전 에러가 계속나서, Windows 10 및 아나콘다 파이썬 3.8 환경에서 설치하는 방법을 한 번 정리해보았습니다."

categories:
  - Blog
tags:
  - [geopandas, python]

toc: true
toc_sticky: true

date: 2021-11-02
last_modified_at: 2021-11-10
---

안녕하세요! 이번 포스트는 Geopandas 설치시 버전 에러가 계속나서, Windows 10 및 아나콘다 파이썬 3.8 환경에서 설치하는 방법을 한 번 정리해보았습니다.

## 기본환경

- Windows 10
- Python 3.8
- +@ 파이썬 가상환경(geopandas가 버전 의존성이 심한 것 같아, geopandas용 가상환경을 만드는 것을 추천)

## 패키지 다운로드 리스트

먼저 관련 패키지를 다음과 같은 경로에 다운로드하였다.

> C:\Windows\System32\geopandas\

Python 버전이 3.8로 시작되기 때문에 각 whl 파일 이름에 38이 포함되어야 한다.

다음 그림과 같이 다운을 받고 저장하였다.

![image](https://user-images.githubusercontent.com/43924464/139789272-927d26f7-e4d3-49cc-ad1f-500484f2e3c2.png)

## 패키지 다운로드 URL

만약 다운로드하려는 패키지의 버전이 없다면 한 단계 높은 버전을 받아서 설치해보길 권장합니다!

- [https://www.lfd.uci.edu/~gohlke/pythonlibs/#shapely](https://www.lfd.uci.edu/~gohlke/pythonlibs/#shapely)
- [https://www.lfd.uci.edu/~gohlke/pythonlibs/#gdal](https://www.lfd.uci.edu/~gohlke/pythonlibs/#gdal)
- [https://www.lfd.uci.edu/~gohlke/pythonlibs/#fiona](https://www.lfd.uci.edu/~gohlke/pythonlibs/#fiona)
- [https://www.lfd.uci.edu/~gohlke/pythonlibs/](https://www.lfd.uci.edu/~gohlke/pythonlibs/) (geopandas 검색)
- [https://www.lfd.uci.edu/~gohlke/pythonlibs/#cartopy](https://www.lfd.uci.edu/~gohlke/pythonlibs/#cartopy)
- [https://www.lfd.uci.edu/~gohlke/pythonlibs/#rasterio](https://www.lfd.uci.edu/~gohlke/pythonlibs/#rasterio)

## Geopandas 관련 패키지 설치 순서

해당 명령어를 **순서대로** 실행하기전에 **관리자 권한**으로 아나콘다 프롬프트를 실행했는지 확인해야 된다.

    pip install pyproj

    pip install geopandas/Shapely-1.8.0-cp38-cp38-win_amd64.whl

    pip install geopandas/GDAL-3.3.3-cp38-cp38-win_amd64.whl

    pip install geopandas/Fiona-1.8.20-cp38-cp38-win_amd64.whl

    pip install geopandas/geopandas-0.10.2-py2.py3-none-any.whl

    pip install geopandas/Cartopy-0.20.1-cp38-cp38-win_amd64.whl

    pip install geopandas/rasterio-1.2.10-cp38-cp38-win_amd64.whl

    pip install contextily

## 정상적으로 설치되었는지 확인

파이썬 주피터 노트북에서 해당 가상환경 커널을 이용하여 다음과 같이 입력하여 잘 설치되었는지 확인한다.

![image](https://user-images.githubusercontent.com/43924464/139789332-a1fe2370-8c3d-4add-9782-0205571ad1be.png)

Geopandas 설치 성공! 😄
