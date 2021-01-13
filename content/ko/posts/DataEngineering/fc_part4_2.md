---
collapsible: false
date: "2021-01-13T10:09:56+09:00"
description: 데이터의 이해와 데이터베이스
draft: false
title: Spotify Data
weight: 6
---

## Part 4. 데이터의 이해와 데이터베이스
본 포스팅은 패스트캠퍼스(FastCampus)의 **데이터 엔지니어링 올인원 패키지 Online**을 참고하였습니다.

### 1. Spotify 데이터 이해

[`Spotify Web API > get an artist`](https://developer.spotify.com/documentation/web-api/reference/artists/get-artist/)

###### artist object
```json
{
  "external_urls" : {
    "spotify" : "https://open.spotify.com/artist/0OdUWJ0sBjDrqHygGUXeCF"
  },
  "followers" : {
    "href" : null,
    "total" : 306565
  },
  "genres" : [ "indie folk", "indie pop" ],
  "href" : "https://api.spotify.com/v1/artists/0OdUWJ0sBjDrqHygGUXeCF",
  "id" : "0OdUWJ0sBjDrqHygGUXeCF",
  "images" : [ {
    "height" : 816,
    "url" : "https://i.scdn.co/image/eb266625dab075341e8c4378a177a27370f91903",
    "width" : 1000
  }, {
    "height" : 522,
    "url" : "https://i.scdn.co/image/2f91c3cace3c5a6a48f3d0e2fd21364d4911b332",
    "width" : 640
  }, {
    "height" : 163,
    "url" : "https://i.scdn.co/image/2efc93d7ee88435116093274980f04ebceb7b527",
    "width" : 200
  }, {
    "height" : 52,
    "url" : "https://i.scdn.co/image/4f25297750dfa4051195c36809a9049f6b841a23",
    "width" : 64
  } ],
  "name" : "Band of Horses",
  "popularity" : 59,
  "type" : "artist",
  "uri" : "spotify:artist:0OdUWJ0sBjDrqHygGUXeCF"
}
```

### 2. Spotify 데이터 모델
###### 예시
![Data Model](images/posts/data_engineering/spotifyERD.PNG)