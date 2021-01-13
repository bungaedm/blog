---
collapsible: false
date: "2021-01-13T10:09:56+09:00"
description: 데이터의 이해와 데이터베이스
draft: false
title: Python & MySQL
weight: 7
---

## Part 4. 데이터의 이해와 데이터베이스
본 포스팅은 패스트캠퍼스(FastCampus)의 **데이터 엔지니어링 올인원 패키지 Online**을 참고하였습니다.

### 1. Pymysql 패키지
```python
import sys
import requests
import base64
import json
import logging
import pymysql #New library

client_id = '' #직접 입력
client_secret = '' # 직접 입력

host = '' #host
port = 3306
username = '' #user
database = '' #db
password = '' #passwd

def main():
    try:
        conn = pymysql.connect(host=host, user=username, passwd=password, db=database, port=port, use_unicode=True, charset='utf8')
        cursor = conn.cursor()
    except:
        logging.error('could not connect to RDS')
        sys.exit(1)

    cursor.execute('SHOW TABLES')
    print(cursor.fetchall())

    print('success')
    sys.exit(0)

if __name__ == '__main__':
    main()
```

### 2. INSERT, UPDATE, REPLACE, INSERT IGNORE
```sql
CREATE TABLE artists (id VARCHAR(255), name VARCHAR(255), followers INT, popularity INT, url VARCHAR(255), image_url VARCHAR(255), PRIMARY KEY(id)) ENGINE=InnoDB DEFAULT CHARSET='utf8';
CREATE TABLE artist_genres (artist_id VARCHAR(255), genre VARCHAR(255)) ENGINE=InnoDB DEFAULT CHARSET='utf8';
SHOW CREATE TABLE artists;

-- INSERT 
INSERT INTO artist_genres (artist_id, genre) VALUES ('1234', 'pop');
DELETE FROM artist_genres; --제거
DROP TABLE artist_genres; --제거
CREATE TABLE artist_genres (artist_id VARCHAR(255), genre VARCHAR(255), UNIQUE KEY(artist_id, genre)) ENGINE=InnoDB DEFAULT CHARSET='utf8'; --unique key 생성
INSERT INTO artist_genres (artist_id, genre) VALUES ('1234', 'pop');
INSERT INTO artist_genres (artist_id, genre) VALUES ('1234', 'pop'); --두 번 하면 ERROR

-- UPDATE
UPDATE artist_genres SET genre='pop' WHERE artist_id ='1234';
ALTER TABLE artist_genres ADD COLUMN country VARCHAR(255); 
ALTER TABLE artist_genres ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON 
UPDATE CURRENT_TIMESTAMP; --업데이트되는 시각대 자동추가
INSERT INTO artist_genres (artist_id, genre, country) VALUES ('1234','pop','UK'); -- 오류 발생함

-- REPLACE
REPLACE INTO artist_genres (artist_id, genre, country) VALUES ('1234','pop','UK');
/* 문제점(1) 지우고 업데이트하기 때문에 2번의 과정을 거쳐서 퍼포먼스적으로 문제 생길 수가 있다.
문제점(2) primary key가 auto_increment인 경우 새로운 숫자로 바뀌게 된다. */

-- INSERT IGNORE
INSERT IGNORE INTO artist_genres (artist_id, genre, country) VALUES ('1234','rock','UK');
/* 문제점(1) 이미 값이 있으면 추가하지 않게 된다.*/

-- INSERT ... ON DUPLICATE KEY UPDATE
INSERT INTO artist_genres (artist_id, genre, country) VALUES ('1234','rock','UK') ON DUPLICATE KEY UPDATE artist_id='1234', genre='rock', country='FR'; --UK를 FR로 바꾼다.

-- ETC
ALTER TABLE artist_genres DROP COLUMN country; --불필요한 칼럼 지우기
```
*MySQL에서 진행하였다.

###### 특이사항
- data type을 INTEGER로 하니까 안 되고, INT로 하니까 됐다.

### 3. _, .format()
```python
def main():
    try:
        conn = pymysql.connect(host=host, user=username, passwd=password, db=database, port=port, use_unicode=True, charset='utf8')
        cursor = conn.cursor()
    except:
        logging.error('could not connect to RDS')
        sys.exit(1)

    cursor.execute('SHOW TABLES')
    print(cursor.fetchall())

    query = "INSERT INTO artist_genres (artist_id, genre) VALUES ('{0}', '{1}')".format('2345','hip-hop')
    cursor.execute(query)
    conn.commit()
    sys.exit(0)

if __name__ == '__main__':
    main()
```

### 4. Dictionary와 JSON Package
```python
def main():
    try:
        conn = pymysql.connect(host=host, user=username, passwd=password, db=database, port=port, use_unicode=True, charset='utf8')
        cursor = conn.cursor()
    except:
        logging.error('could not connect to RDS')
        sys.exit(1)

    headers = get_headers(client_id, client_secret)

    ## Spotify Search api
    params = {
        'q': 'BTS',
        'type': 'artist',
        'limit': '5'
    }

    r = requests.get('https://api.spotify.com/v1/search', params=params, headers=headers)
    raw = json.loads(r.text)

    print(raw['artists'].keys)

def get_headers(client_id, client_secret):
    endpoint = 'https://accounts.spotify.com/api/token'
    encoded = base64.b64encode("{}:{}".format(client_id, client_secret).encode('utf-8')).decode('ascii')

    headers = {'Authorization': 'Basic {}'.format(encoded)}
    payload = {'grant_type': 'client_credentials'}

    r = requests.post(endpoint, data=payload, headers=headers)
    access_token = json.loads(r.text)['access_token']
    headers = {'Authorization': "Bearer {}".format(access_token)}

    return headers

if __name__ == '__main__':
    main()
```

### 5. Duplicate Record 핸들링
```python
def main():
    try:
        conn = pymysql.connect(host=host, user=username, passwd=password, db=database, port=port, use_unicode=True, charset='utf8')
        cursor = conn.cursor()
    except:
        logging.error('could not connect to RDS')
        sys.exit(1)

    headers = get_headers(client_id, client_secret)

    ## Spotify Search api
    params = {
        'q': 'BTS',
        'type': 'artist',
        'limit': '1'
    }

    r = requests.get('https://api.spotify.com/v1/search', params=params, headers=headers)
    raw = json.loads(r.text)

    artist_raw = raw['artists']['items'][0]
    if artist_raw['name'] == params['q']:
        artist = {
            'id': artist_raw['id'],
            'name': artist_raw['name'],
            'followers': artist_raw['followers']['total'],
            'popularity': artist_raw['popularity'],
            'url': artist_raw['external_urls']['spotify'],
            'image_url': artist_raw['images'][0]['url']
        }

    query = """
        INSERT INTO artists (id, name, followers, popularity, url, image_url)
        VALUES ('{}', '{}', {}, {}, '{}', '{}')
        ON DUPLICATE KEY UPDATE id='{}', name='{}', followers={}, popularity={}, url='{}', image_url='{}'
    """.format(
        artist['id'],
        artist['name'],
        artist['followers'],
        artist['popularity'],
        artist['url'],
        artist['image_url'],
        artist['id'],
        artist['name'],
        artist['followers'],
        artist['popularity'],
        artist['url'],
        artist['image_url']
    )

    cursor.execute(query)
    conn.commit()
```

### 6. Duplicate Record 핸들링을 위한 파이썬 함수
###### 5와 다른 점을 눈여겨 보기 (5를 보다 간단하게 한 코드)
```python
def main():
    ## .... 여기까지는 위와 동일

    r = requests.get('https://api.spotify.com/v1/search', params=params, headers=headers)
    raw = json.loads(r.text)

    artist = {}
    artist_raw = raw['artists']['items'][0]
    if artist_raw['name'] == params['q']:
        artist.update({
            'id': artist_raw['id'],
            'name': artist_raw['name'],
            'followers': artist_raw['followers']['total'],
            'popularity': artist_raw['popularity'],
            'url': artist_raw['external_urls']['spotify'],
            'image_url': artist_raw['images'][0]['url']
        })

    insert_row(cursor, data=artist, table='artists')
    conn.commit()

def insert_row(cursor, data, table):
    placeholders = ', '.join(['%s'] * len(data))
    columns = ', '.join(data.keys())
    key_placeholders = ', '.join(['{0}=%s'.format(k) for k in data.keys()])
    sql = 'INSERT INTO %s ( %s ) VALUES ( %s ) ON DUPLICATE KEY UPDATE %s' % (table, columns, placeholders, key_placeholders)
    cursor.execute(sql, list(data.values())*2)
```    

### 7. Artist list 추출하기
- 패스트캠퍼스 강좌를 통해 제공된 artist_list.csv 파일을 활용하였다.
```python
def main():
    try:
        conn = pymysql.connect(host=host, user=username, passwd=password, db=database, port=port, use_unicode=True, charset='utf8')
        cursor = conn.cursor()
    except:
        logging.error('could not connect to RDS')
        sys.exit(1)

    headers = get_headers(client_id, client_secret)

    artists = []
    with open('../artist_list.csv', encoding="utf-8") as f:
        raw = csv.reader(f)
        for row in raw:
            artists.append(row[0])

    for a in artists:
        params = {
            'q': a,
            'type': 'artist',
            'limit': '1'
        }

        r = requests.get('https://api.spotify.com/v1/search', params=params, headers=headers)
        raw = json.loads(r.text)

        artist = {}
        try:
            if raw['artists']['items'][0]['name'] == params['q']:
                artist.update(
                    {
                        'id': raw['artists']['items'][0]['id'],
                        'name': raw['artists']['items'][0]['name'],
                        'followers': raw['artists']['items'][0]['followers']['total'],
                        'popularity': raw['artists']['items'][0]['popularity'],
                        'url': raw['artists']['items'][0]['external_urls']['spotify'],
                        'image_url': raw['artists']['items'][0]['images'][0]['url']
                    }
                )

                insert_row(cursor, artist, 'artists')
        except:
            logging.error('NO ITEMS FROM SEARCH API')
            continue

    conn.commit()
    # sys.exit(0)
```
`ERROR:root:NO ITEMS FROM SEARCH API`와 같은 에러가 여러 개가 나오게 된다.
말그대로 SERACH API를 통해서 ITEMS를 찾지 못하게 된 경우에 해당한다.

### 8. Batch 형식으로 데이터 요청
- 한번에 묶어서 API에 전달하는 방식이다.
- 모든 API가 제공하는 것은 아니긴 하다!
- Spotify는 ['Get Several Artists']((https://developer.spotify.com/console/get-several-artists/))하는 법을 제공하고 있다.


### 9. MySQL 테이블들로 데이터 저장