---
title: "파이썬 셀레니움(Selenium)을 이용하여 데이터 자동 수집"
excerpt: "안녕하세요! 이번 포스트에서는 파이썬 셀레니움을 이용하여 교통카드 빅데이터 통합정보시스템 홈페이지에서 **일반버스·도시철도 O/D 데이터를 자동으로 수집**하려고 합니다!"

categories:
  - Blog
tags:
  - [selenium, python, data]

toc: true
toc_sticky: true

date: 2021-11-10
last_modified_at: 2021-11-11
---

안녕하세요! 이번 포스트에서는 파이썬 셀레니움을 이용하여 교통카드 빅데이터 통합정보시스템 홈페이지에서 **일반버스·도시철도 O/D 데이터를 자동으로 수집**하려고 합니다!

## 셀레니움 소개

셀레니움은 크롬, 엣지, 파이어폭스 등 **브라우저를 자동화하여 컨트롤** 할 수 있게 해줍니다. 보통 크롤링이랑 같이쓰면 더 좋은 시너지를 발휘할 수 있습니다.

데이터를 수집하려는데 일일이 클릭하여 수집하기보단 자동화하여 매크로 돌리듯이 수집할 수 있게 도와줍니다.

## 교통카드 빅데이터 통합정보시스템 소개

교통카드 빅데이터 통합정보시스템 홈페이지 [LINK](https://stcis.go.kr/wps/main.do) : 전국단위 **교통카드데이터 기반**의 통계자료를 제공하고 있습니다. 정부 3.0 시대에 맞춰 교통과 관련하여 다양한 통계정보를 제공하고 있습니다.

![image](https://user-images.githubusercontent.com/43924464/140259864-18ad52db-2be9-4e18-9d65-98a7b2fbbc83.png)

하지만 아직 초기 시스템이라 그런지 나중에보면 알겠지만 하루단위씩 끊어서 데이터를 수집해야하는 등 데이터 수집의 어려움과 번거로움이 있습니다. 😢

## 기본환경

다음과 같은 환경에서 실행하였다.

- Windows 10
- Python 3.8
- Chrome 95.xx 버전대

## 1. Chromedriver.exe 다운로드 및 설치

셀레니움을 활용하려면 Chromedriver가 필요하다. 먼저 Chromedriver를 다운로드 받기전에 나의 현재 크롬버전을 확인하자.

크롬 우측 상단의 점3개를 클릭하여 설정으로 들어간다음, 좌측하단에 있는 Chrome 정보를 누르면 다음과 같은 화면이 뜰 것이다.

![image](https://user-images.githubusercontent.com/43924464/140260758-ad43e546-52d3-4357-8e25-c804f5f01647.png)

위 화면을 보면 버전이 95로 시작됨을 볼 수 있다.

버전을 확인하고 [https://chromedriver.chromium.org/downloads](https://chromedriver.chromium.org/downloads) 으로 접속하자!

해당 링크로 접속하면 다음과같은 화면이 뜰 것이고 내 버전에 맞는 Chromedriver를 다운받았다. 만약 매칭되는 버전이 없다면 상위버전을 받으면 된다.

![image](https://user-images.githubusercontent.com/43924464/140261060-2f8b9c18-90e1-4fea-b489-98aaebcbf9ef.png)

나의 크롬 버전은 95.0.4638.54(공식 빌드) (64비트)과 같아서 "ChromeDriver 95.0.4638.54"를 클릭하였고 다음과 같은 화면에서 "chromedriver_win32.zip"을 다운로드 받았다.

![image](https://user-images.githubusercontent.com/43924464/140261548-41410cb2-60c8-43e0-98a1-4b5871bf3c14.png)

다운로드 받고 압축파일을 풀어서 실행시킬 스크립트 파일과 같은 경로에 두면 된다!

![image](https://user-images.githubusercontent.com/43924464/140273383-7c84267f-11b3-41da-a603-268cbd455015.png)

## 2. 수집 데이터 정의

수집하려는 **지표**와 데이터의**공간적·시간적 범위**는 다음과 같다.

- 지표 : 일반버스·도시철도 이용 O/D
- 공간적 범위 : 전국, 시군구단위
- 시간적 범위 : 2021년 1월 (한달)

## 3. 홈페이지 파악

홈페이지 들어가서, 지표조회를 보면 통행량, 통행거리, 정류장별 이용량 등 다양한 통계정보를 제공해줌을 볼 수 있다.

우리는 여기서 "**일반버스·도시철도 이용 O/D**"를 클릭하여 다음과같은 화면으로 들어온다.

![image](https://user-images.githubusercontent.com/43924464/140275568-54e75f51-bcb9-4a84-92de-43d8c0aeb4aa.png)

여기서 줄 옵션 및 고려사항은 다음과 같다.

- 지표 : 자동으로 선택
- 일자 : 하루단위씩 선택
- 공간선택 :
  -- 출발지 : 시/군/구 단위 선택 가능, 전체선택 가능
  -- 도착지 : 시/군/구 단위 선택 가능, 각 시도를 돌면서 시/군/구 선택해야됨 (전체선택 불가능)

## 4. 파이썬 셀레니움 코드 (1) - 일자/시도/시군구 경로 만들기

### 4-1. 필요한 라이브러리들을 import

```python
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.common.alert import Alert
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By

import time
```

webdriver는 "chromedriver.exe"를 실행시키기 위해서 필요하다. 나머지는 위에서 설명했던 내용과 같다.

팝업창이 나타나는 경우, 이를 제어해주는 **Alert**가 필요하다.

검색버튼을 눌렀을 때 데이터 로딩시간이 길어지는 경우, 이를 제어하기위한 **WebDriverWait**가 필요하다.

마지막으로 **WebDriverWait**를 통해서 기다린 결과를 이용해 최종적인 다운로드 버튼을 클릭할 수 있게 연결해주는
**element_to_be_clickable**와 **By**가 필요해보인다.

### 4-2. 홈페이지 접속

```python
start = time.time()

driver = webdriver.Chrome()

# 접속 사이트 불러오기
url = 'https://www.stcis.go.kr/gov/loginUser.do'
driver.get(url)
```

### 4-3. 지표선택

메인화면에서 해당 지표를 선택하면 된다.

```python
# 일반버스·도시철도이용 O/D 클릭
xpath = """/html/body/div[2]/div/div[2]/div[1]/ul/li[9]/a/div[1]/img"""
driver.find_element_by_xpath(xpath).click()
```

### 4-4. 달력 초기화 (일자 경로 얻기 위함)

달력은 1월 1일로 만들어줘야 1월달에 대한 **일자 경로**들을 쭉 뽑을 수 있기 때문에 초기화 해줬다.

```python
xpath = "/html/body/div[2]/form/div[1]/div[2]/div[2]/div/div[2]/div[2]/li/img"
driver.find_element_by_xpath(xpath).click()
time.sleep(0.5)
xpath = "/html/body/div[8]/div/div/select[2]/option[1]"
driver.find_element_by_xpath(xpath).click()
xpath = "/html/body/div[8]/table/tbody/tr[1]/td[6]/a"
driver.find_element_by_xpath(xpath).click()
```

### 4-5. 시도 xpath 경로 리스트

```python
sido_xpaths = ["""//*[@id="searchAlocZoneSd"]/option[{}]""".format(i) for i in range(2, 19)]
```

### 4-6. 시군구 xpath경로 리스트

```python
sigungus_path = []
sidos = driver.find_element_by_xpath("/html/body/div[2]/form/div[1]/div[2]/div[2]/div/div[4]/div/div[2]/div/ul[2]/li/table/tbody/tr[3]/td/select[1]")
sidos = sidos.find_elements(By.TAG_NAME, "option")

for i in range(1, len(sidos)):
    sidos[i].click()
    time.sleep(1)
    sigungus = driver.find_element_by_xpath("/html/body/div[2]/form/div[1]/div[2]/div[2]/div/div[4]/div/div[2]/div/ul[2]/li/table/tbody/tr[3]/td/select[2]")
    sigungus = sigungus.find_elements(By.TAG_NAME, "option")
    temp = []
    for j in range(1, len(sigungus)):
        temp.append(j+1)
    sigungus_path.append(temp)

sigungu_xpaths=[]

for sigungu in sigungus_path:
    temp = []
    for index in sigungu:
        temp.append("""//*[@id="searchAlocZoneSgg"]/option[{}]""".format(index))
    sigungu_xpaths.append(temp)
```

### 4-7. 일자 xpath 경로 리스트

```python
january = []

table = driver.find_element_by_xpath("/html/body/div[8]/table") #달력 <table>
tbody = table.find_element(By.TAG_NAME, "tbody") # table 내의 모든 tbody
trs = tbody.find_elements(By.TAG_NAME, "tr") # 모든 tbody의 tr

for i, tr in enumerate(trs): # 1주차부터 마지막주차까지 돌면서 클릭 가능한 일자 얻기
    tds = tr.find_elements(By.TAG_NAME, "td")
    time.sleep(0.5)
    for j, td in enumerate(tds):
        a_s = td.find_elements(By.TAG_NAME, "a")
        time.sleep(0.5)
        for a in a_s:
            january.append([i+1, j+1])



january_xpaths = ["""//*[@id="ui-datepicker-div"]/table/tbody/tr[{}]/td[{}]/a""".format(path[0], path[1]) for path in january]
january_xpaths
```

## 5. 파이썬 셀레니움 코드 (2) - 데이터 추출 작업

만들어진 각각의 경로들을 가지고 데이터 추출 작업을 시작하려고 한다.

### 5-1. 홈페이지 접속 및 기본설정

먼저 다시 사이트로 접속하여 해당 지표를 클릭하고 공간선택 중에 계속 고정이되는 부분을 기본으로 선택한다.

```python
start = time.time()
driver = webdriver.Chrome()

# 접속 사이트 불러오기
url = 'https://stcis.go.kr/wps/main.do'
driver.get(url)

# 일반버스·도시철도이용 O/D 클릭
xpath = """/html/body/div[2]/div/div[2]/div[1]/ul/li[9]/a/div[1]/img"""
driver.find_element_by_xpath(xpath).click()

# 공간선택 기본설정
xpath = "/html/body/div[2]/form/div[1]/div[2]/div[2]/div/div[4]/div/div[1]/div/input[2]" # 출발지 - 시군구
driver.find_element_by_xpath(xpath).click()
xpath = "/html/body/div[2]/form/div[1]/div[2]/div[2]/div/div[4]/div/div[2]/div/ul[1]/li/table/tbody/tr[2]/td/input[2]" # 출발지 - 전체
driver.find_element_by_xpath(xpath).click()
xpath = "/html/body/div[2]/form/div[1]/div[2]/div[2]/div/div[4]/div/div[2]/div/div/div/input[2]" # 도착지 - 시군구
driver.find_element_by_xpath(xpath).click()
xpath = "/html/body/div[2]/form/div[1]/div[2]/div[2]/div/div[4]/div/div[2]/div/ul[2]/li/table/tbody/tr[2]/td/input[1]" # 도착지 - 선택
driver.find_element_by_xpath(xpath).click()
```

### 5-2. 데이터 추출 작업

만들었던 경로 리스트를 이용하여 일자별 시도별 시군구별 for문을 이용해 데이터 추출 작업을 수행한다.

```python

# 1월 1일부터 30일까지 모든 시도의 시군구 데이터 추출작업
for month_index in range(len(january_xpaths)):

    january_xpath = january_xpaths[month_index]

    xpath = "/html/body/div[2]/form/div[1]/div[2]/div[2]/div/div[2]/div[2]/li/img"
    driver.find_element_by_xpath(xpath).click()
    time.sleep(0.2)

    xpath = "/html/body/div[8]/div/div/select[2]/option[1]"
    driver.find_element_by_xpath(xpath).click()

    driver.find_element_by_xpath(january_xpath).click()
    time.sleep(0.2)

	# 시도
    for index, sido_xpath in enumerate(sido_xpaths):

            driver.find_element_by_xpath(sido_xpath).click()
            time.sleep(0.2)

	        # 시군구
            for sigungu_xpath in sigungu_xpaths[index]:
                driver.find_element_by_xpath(sigungu_xpath).click()
                time.sleep(0.2)

                # 검색버튼 클릭
                xpath = "/html/body/div[2]/form/div[1]/div[2]/div[2]/div/div[8]/button"
                driver.find_element_by_xpath(xpath).click()
                time.sleep(0.2)

                # 팝업창 클릭 (검색버튼 클릭시 생기는 팝업창)
                popUp = Alert(driver)
                popUp.accept()
                time.sleep(0.2)

                # 다운로드
                wait = WebDriverWait(driver, 120) # 120초까지 응답이 안된다면 해당 명령 취소하고 중지
                xpath = """/html/body/div[2]/form/div[2]/div[3]/div/h2/p/span[1]/a"""
                element = wait.until(EC.element_to_be_clickable((By.XPATH, xpath)))
                driver.find_element_by_xpath(xpath).click()
                time.sleep(0.2)

                # 팝업창 클릭 (데이터가 없는경우 생기는 팝업창)
                try:
                    popUp = Alert(driver)
                    popUp.accept()
                    time.sleep(0.5)
                except:
                    continue
                    time.sleep(0.5)
```

## 6. 전체 코드 및 데이터 샘플

소스코드 : [https://github.com/everlast0430/cityBusRailOD-python-selenium](https://github.com/everlast0430/cityBusRailOD-python-selenium)

데이터 샘플 :
![image](https://user-images.githubusercontent.com/43924464/141049020-585190ef-52e1-4e3e-9351-d796aa5ae107.png)

## 7. 주의사항

브라우저 또는 서버 오류로 인하여 중간에 자주 멈추게된다...

```python
for month_index in range(len(january_xpaths)):

    january_xpath = january_xpaths[month_index]
```

그럴경우 month_index 부분을 수정하면 된다.

만약 24일에서 뭠췄다면, 다음과같이 수정하면 될 것이다.

```python
range(23, len(january_xpaths))
```

## 8. 마무리

이상으로 파이썬 셀레니움을 활용해서 교통카드 빅데이터 통합정보시스템 홈페이지에서 일반버스·도시철도 이용 O/D 데이터를 추출해보았습니다. 혹시 문제가 있거나 간단한 방법이 있다면 피드백 부탁드립니다! 감사합니다. 😄
