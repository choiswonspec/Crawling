# 기본 코드

```python
import requests

from bs4 import BeautifulSoup

url = '<https://kin.naver.com/search/list.nhn?query=%ED%8C%8C%EC%9D%B4%EC%8D%AC>'

response = requests.get(url)

html = response.text # 또는 content
soup = BeautifulSoup(html, 'html.parser')
print(soup)

soup.find()

get_text()
```

# 원하는 정보가 있는 위치 찾기

https://m.blog.naver.com/kiddwannabe/221177292446

- soup.select('원하는 정보') # select('원하는 정보') --> 단 하나만 있더라도, 복수 가능한 형태로 되어있음
- soup.select('태그명')
- soup.select('.클래스명')
- soup.select('상위태그명 > 하위태그명 > 하위태그명')
- soup.select('상위태그명.클래스명 > 하위태그명.클래스명') # 바로 아래의(자식) 태그를 선택시에는 > 기호를 사용
- soup.select('상위태그명.클래스명 하~위태그명') # 아래의(자손) 태그를 선택시에는 띄어쓰기 사용
- soup.select('상위태그명 > 바로아래태그명 하~위태그명')
- soup.select('.클래스명')
- soup.select('#아이디명') # 태그는 여러개에 사용 가능하나 아이디는 한번만 사용 가능함! ==> 선택하기 좋음
- soup.select('태그명.클래스명)
- soup.select('#아이디명 > 태그명.클래스명)
- soup.select('태그명[속성1=값1]')

# Selenium

## 기본코드

```python
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager # webdrv-manager 패키지 다운로드
from bs4 import BeautifulSoup
import time
from selenium.webdriver.common.keys import Keys
import numpy as np
import pandas as pd
import urllib.request
import requests
from tqdm import tqdm
import os
from datetime import datetime

# 해당 사이트 접속
driver = webdriver.Chrome(ChromeDriverManager().install())
driver.get('<https://www.pinterest.co.kr>')
driver.implicitly_wait(1)

# 로그인 하기
driver.find_element_by_xpath('//*[@id=\\"__PWS_ROOT__\\"]/div[1]/div/div/div/div[1]/div[1]/div[2]/div[2]/button/div').click()
driver.implicitly_wait(1)
time.sleep(1)
my_id = 'csw31915@daum.net'
my_password = 'welcome2019!'
driver.find_element_by_xpath('//*[@id=\\"email\\"]').send_keys(my_id)
driver.find_element_by_xpath('//*[@id=\\"password\\"]').send_keys(my_password)
driver.find_element_by_xpath('//*[@id=\\"__PWS_ROOT__\\"]/div[1]/div/div/div/div[1]/div[2]/div[2]/div/div/div/div[1]/div/div/div/div[3]/form/div[5]/button/div').click()
time.sleep(2)
```

## 검색 코드

```python
# 검색어에 입력후 검색
a = driver.find_element_by_xpath('//*[@id="searchBoxContainer"]/div/div/div[2]/input') # 검색창 xpath 따오기
a.send_keys(word)
a.send_keys(Keys.ENTER)
time.sleep(1.5)
```

## 스크롤 내리기

```python
from selenium.webdriver.common.keys import Keys

# 스크롤 내리기... pinterest 사이트 특성상 스크롤을 내릴 때마다 html 수집하여 이미지 url을 수집해야한다.. 
b = driver.find_element_by_tag_name('body')

b.send_keys(Keys.END)
time.sleep(1.5)
```

## 이미지 가져오기

```python
import os
import requests

# 먼저 이미지 url을 수집하여 list로 만든 후,
count = 0
for url in url_list:
    try:
        res = requests.get(url, verify=False, stream=True)
        rawdata = res.raw.read()
        direct = f'/Users/choiswonspec/pinterest/{파일이름변수1}/{파일이름변수2}' # 저장할 경로 이름
        with open(os.path.join(direct, str(count) + '.jpg'), 'wb') as f:
            f.write(rawdata)
            time.sleep(0.1)
				count += 1
                
    except Exception as e:
        print('Failed to write rawdata.')
        print(e)
		if count == 1000:
                break

time.sleep(1)
```