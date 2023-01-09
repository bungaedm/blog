---
collapsible: false
date: "2022-01-08T10:09:56+09:00"
description: 유튜브 크롤링 Youtube Crawling
draft: false
title: 유튜브 크롤링 
weight: 1
---

# Youtube Crawling

## 1. 전체 함수
(유의사항) `2. 개별 함수`의 함수들을 순차적으로 정의한 후에 전체 함수(`youtube_crawling`)를 정의해야 실행이 된다.
```python
def youtube_crawling(channel_name, csv_name, export=True, verbose=True):
    # Selenium Option Setting
    options = set_selenium_option()
    
    # Extract Video Links by crawling
    today_video_link_list = youtube_video_list(channel_name, options)
    
    # Today & Yesterdat to string
    today_str, yesterday_str = make_date_str()
    
    # Load previously crawled data
    previous_df = previous_csv(csv_name, yesterday_str)
    
    # Check if today's crawling already exists
    if os.path.isfile('result\\' + csv_name + '_' + today_str + '.csv'):
        print('이미 오늘자 파일이 존재합니다.')
        return
    
    # Update crawling data
    df = update_video_info(previous_df, today_video_link_list)
    
    if export:
        df.to_csv('result/' + csv_name + '_' + today_str + '.csv', index=False)
    if verbose:
        new_video_links = set(today_video_link_list) - set(previous_df['video_url'])
        deleted_video_links = set(previous_df['video_url']) - set(today_video_link_list)
        print('오늘({}) 크롤링 영상 개수: {}개'.format(today_str, len(today_video_link_list)))
        print('어제({}) 크롤링 영상 개수: {}개 (전체 {}행)'.format(yesterday_str, len(np.unique(previous_df['title'])), previous_df.shape[0]))
        print('새로 업로드 된 영상 개수: {}개'.format(len(new_video_links)))
        print('삭제된 영상 개수: {}개'.format(len(deleted_video_links)))
        print(date.today(), '영상정보 저장 완료')
```

## 2. 개별 함수

### Step 1. Library Import
{{<expand "Code">}}
```python
import os
import time
import pandas as pd
import numpy as np
from datetime import date, datetime

from bs4 import BeautifulSoup
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By

from pytube import YouTube

import warnings
warnings.filterwarnings(action='ignore')
```
{{</expand>}}

### Step 2. Selenium Option
{{<expand "Code">}}
```python
def set_selenium_option():
    global options
    # Setting
    os.chdir('C:\\Users\\bunga\\Desktop\\python\\youtube')
    options = webdriver.ChromeOptions()
    user_agent = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.0.0 Safari/537.36'
    
    # Option Control
    options.add_argument('user-agent=' + user_agent)
    options.add_argument('headless') # 창을 띄우지 않습니다
    options.add_argument('window-size=1920x1080')
    options.add_argument('disable-gpu')
    options.add_argument('disable-infobars')
    options.add_argument('--disable-extensions')
    options.add_argument('--mute-audio')
    options.add_argument('--blink-settings=imagesEnabled=false') # 브라우저에서 이미지 로딩을 하지 않습니다.
    options.add_argument('incognito') # 시크릿모드의 브라우저가 실행됩니다.
    options.add_argument('--start-maximized')
    
    return options
```
{{</expand>}}

### Step 3. Youtube 영상 리스트 크롤링
{{<expand "Code">}}
```python
def scrollToEnd():
    prev_height = driver.execute_script("return document.documentElement.scrollHeight")
    # 웹페이지 맨 아래까지 무한 스크롤
    while True:
        # 스크롤을 화면 가장 아래로 내린다
        element = driver.find_element(By.TAG_NAME, 'body')
        element.send_keys(Keys.END)

        # 페이지 로딩 대기
        time.sleep(2)

        # 현재 문서 높이를 가져와서 저장
        curr_height = driver.execute_script("return document.documentElement.scrollHeight")

        if(curr_height == prev_height):
            break
        else:
            prev_height = driver.execute_script("return document.documentElement.scrollHeight")
```
{{</expand>}}


{{<expand "Code">}}
```python
def youtube_video_list(channel_name, selenium_option):
    # Channel URL
    channel_url = 'https://www.youtube.com/' + channel_name + '/videos'
    
    # 페이지 탐색
    global driver
    driver = webdriver.Chrome('chromedriver.exe', options=selenium_option)
    driver.get(channel_url)
    driver.switch_to.window(driver.window_handles[-1])
    scrollToEnd()
    
    # 페이지 정보 추출
    html = driver.page_source
    soup = BeautifulSoup(html, 'lxml')
    
    # Driver 닫기
    driver.close()
    
    # 영상 링크 추출
    sample_list = soup.find_all('a', class_='yt-simple-endpoint focus-on-expand style-scope ytd-rich-grid-media')
    video_link_list = []
    for sample in sample_list:
        video_link = sample.get_attribute_list('href')[0]
        video_link_list.append('https://youtube.com'+video_link)
        
    return video_link_list
```
{{</expand>}}

### Step 4. 날짜 변수 만들기
{{<expand "Code">}}
```python
def make_date_str():
    # 날짜변수 제작
    today = date.today().year*10000 + date.today().month*100 + date.today().day
    today_str = str(today)
    yesterday = today-1
    yesterday_str = str(yesterday)
    return today_str, yesterday_str
```
{{</expand>}}

### Step 5. 이전일자 csv 불러오기
{{<expand "Code">}}
```python
def previous_csv(csv_name, yesterday_str):
    # 어제자 csv 불러오기 (없으면 빈 데이터프레임 만들기)
    try: 
        df = pd.read_csv('result/' + csv_name + '_' + yesterday_str + '.csv')
    except:
        print('이전 파일이 없습니다.')
        df_columns=['crawl_datetime', 'channel', 'title', 'length', 'views', 'publish_date',
                    'video_url','thumbnail_url', 'keywords', 'description']
        df = pd.DataFrame(columns=df_columns)
    
    return df
```
{{</expand>}}

### Step 6. Pytube 활용하기
{{<expand "Code">}}
```python
def update_video_info(df, video_link_list, verbose=True) :
    print('--------------------------------------------------------------------------')
    today_datetime = datetime.today()
    for idx, url in enumerate(video_link_list):
        # 유튜브 정보 불러오기
        youtube = YouTube(url)

        # 전체 영상 수 
        if idx==0:
            print('{} / 영상: {}개'.format(youtube.author, len(video_link_list)))

        # 개별 영상 정보
        new = [today_datetime, youtube.author, youtube.title, youtube.length, youtube.views, youtube.publish_date.date(),
               youtube.watch_url, youtube.thumbnail_url, youtube.keywords, youtube.description]
        df_new = pd.DataFrame([new], index=[idx], columns=df.columns)
        df = pd.concat([df, df_new])

        # 진행상황 체크
        if verbose and (idx+1)%10==0:
            print('{}번째 영상정보 정리 완료!'.format(idx+1))
    print('\n')
    return df
```
{{</expand>}}

## 3. Example
```python
youtube_crawling(channel_name = '@junsooham', csv_name='hamjunsoo')
```

```result
이전 파일이 없습니다.
--------------------------------------------------------------------------
JUNSOO 함준수 / 영상: 343개
10번째 영상정보 정리 완료!
20번째 영상정보 정리 완료!
30번째 영상정보 정리 완료!
40번째 영상정보 정리 완료!
50번째 영상정보 정리 완료!
60번째 영상정보 정리 완료!
70번째 영상정보 정리 완료!
80번째 영상정보 정리 완료!
90번째 영상정보 정리 완료!
100번째 영상정보 정리 완료!
110번째 영상정보 정리 완료!
120번째 영상정보 정리 완료!
130번째 영상정보 정리 완료!
140번째 영상정보 정리 완료!
150번째 영상정보 정리 완료!
160번째 영상정보 정리 완료!
170번째 영상정보 정리 완료!
180번째 영상정보 정리 완료!
190번째 영상정보 정리 완료!
200번째 영상정보 정리 완료!
210번째 영상정보 정리 완료!
220번째 영상정보 정리 완료!
230번째 영상정보 정리 완료!
240번째 영상정보 정리 완료!
250번째 영상정보 정리 완료!
260번째 영상정보 정리 완료!
270번째 영상정보 정리 완료!
280번째 영상정보 정리 완료!
290번째 영상정보 정리 완료!
300번째 영상정보 정리 완료!
310번째 영상정보 정리 완료!
320번째 영상정보 정리 완료!
330번째 영상정보 정리 완료!
340번째 영상정보 정리 완료!


오늘(20230109) 크롤링 영상 개수: 343개
어제(20230108) 크롤링 영상 개수: 0개 (전체 0행)
새로 업로드 된 영상 개수: 343개
삭제된 영상 개수: 0개
2023-01-09 영상정보 저장 완료
```
