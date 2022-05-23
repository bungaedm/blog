---
title: JW Blog
date: "2020-01-26T04:15:05+09:00"
description: Hugo zzo, zdoc theme documentation home page
draft: false

# landing:
#   - type: typewriter
#     methods:
#       - typeString: Hello world!
#       - pauseFor: 2500
#       - deleteAll: true
#       - typeString: Strings can be removed
#       - pauseFor: 2500
#       - deleteChars: 7
#       - typeString: <strong>altered!</strong>
#       - pauseFor: 2500
#     options:
#       loop: true
#       autoStart: false
#     height: 190
#     paddingX: 50
#     align: center
#     fontSize: 44
#     fontColor: yellow
    
landing:
  # buttons:
  # - color: null
  #   link: posts
  #   text: View Posts
  # height: 500
  image: favicon/logo_small.png
  spaceBetweenTitleText: 5
  text:
  - Yonsei University
  textColor: null
  title:
  - Jiwoo Son
  titleColor: null

sections:
- bgcolor: '#ffbf00'
  body:
    color: white
    description:
      저는 **통계학**과 **심리학**을 공부하고 있는 대학원생입니다. <br>
      현재는 데이터분석에 관심이 생겨서 다양한 도전을 해보고 있습니다. <br> <br>
      **2021~NOW** | **연세대 통계데이터사이언스학과(대학원)** 재학 <br>
      **2020~2021** | ESC / 연세대학교 통계학회 학술부 및 총무 <br>
      **2017~2019** | 대한민국 공군 / RAPCON Radar Approach Control <br>
      **2016~2021** | 응용통계학과 이중전공 <br>
      **2015~2017** | KSCY 한국청소년학술대회 / 인문계열 컨퍼런스 총괄 및 헤드 퍼실리테이터 <br>
      **2015~2015** | 심리학 학술모임장 / 연세대 심리학 학술소모임 Psy-World 설립 및 운영 <br>
      **2013~2014** | 심리학 동아리장 / 하나고 LIOM Look Into Our Minds 운영 <br>
    image: images/section/brain3.png
    imagePosition: left
    subtitle: Who am I?
    subtitlePosition: left
  description: 간단자기소개 
  header:
    color: '#fff' 
    fontSize: 32
    hlcolor: '#8bc34a'
    title: Intro
    width: 140
  type: normal
  
- bgcolor: '#5a8734'
  cards:
  - button:
      bgcolor: '#ffbf00'
      color: white
      link: https://blog.naver.com/bungaedm
      name: Link
      size: large
      target: _blank
    color: white
    # description: 네이버 블로그
    image: images/section/naver.png
    subtitle: Naver Blog
    subtitlePosition: center
  - button:
      bgcolor: '#ffbf00'
      color: white
      link: https://www.instagram.com/5on_jiwoo
      name: Link
      size: large
      target: _blank
    color: white
    # description: 인스타그램
    image: images/section/instagram.png
    subtitle: Instagram
    subtitlePosition: center
  - button:
      bgcolor: '#ffbf00'
      color: white
      link: https://www.facebook.com/jiwoo.son.50/
      name: Link  
      size: large
      target: _blank
    color: white
    # description: 페이스북 
    image: images/section/facebook.png
    subtitle: Facebook
    subtitlePosition: center
  # description: Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce id eleifend
  #   erat. Integer eget mattis augue. Suspendisse semper laoreet tortor sed convallis.
  #   Nulla ac euismod lorem
  header:
    color: '#fff'
    fontSize: 32
    hlcolor: '#8bc34a'
    title: Profile
    width: 200
  type: card
  
- bgcolor: '#ffbf00'
  body:
    description:
      1. [행복지수 예측](/posts/project/202005_happiness_index/)
      
      2. [수소차 충전소 입지 추천](/posts/project/202006_hydrogen_car/)
      
      3. [NS Shop+ 홈쇼핑 매출 예측](/posts/project/202009_nsshop_bigcontest/) (빅콘테스트 챔피언스리그)
      
      4. [NH투자증권 Y&Z세대 투자자 프로파일링](/posts/project/202011_nh_yz/) (Dacon)
      
      5. [아파트 경매가격 예측](/posts/project/202105_apartment_auction/)
      
      6. [빅맥과 노동가치](/posts/project/202105_bigmac/)
      
      7. [수질오염총량관리제 시행에 대한 지역주민 인식 분석](/posts/project/202105_waterpollution/)
      
      8. [택배회사직원 지각시간](/posts/project/202106_delivery_lateness/)
      
      9. [큰돌고래 상호작용 네트워크분석](/posts/project/202112_dolphin_network/)
      
      10. [아파트 경매가격 예측2](/posts/project/202112_apartment_auction2/)
      
      11. [효돌 사용자 군집화](/posts/project/202112_hyodol/)
      
    image: images/section/keyboard.png
    imagePosition: left
    # subtitle: Projects
    # subtitlePosition: left
    #color: white
  description: null
  header:
    color: '#fff'
    fontSize: 32
    hlcolor: '#8bc34a'
    title: Projects
    width: 170
  type: normal
  
- bgcolor: '#5a8734'
  cards:
  - cards:
    color: white
    image: images/section/r.png
    subtitle: R
  - cards:
    color: white
    image: images/section/python.png
    subtitle: Python
  - cards:
    color: white
    image: images/section/sql.png
    subtitle: SQL
  - cards:
    color: white
    image: images/section/tableau.jpg
    subtitle: Tableau
  - cards:
    color: white
    image: images/section/spss.png
    subtitle: SPSS        
  header:
    color: '#fff'
    fontSize: 32
    hlcolor: '#8bc34a'
    title: Language Available
    width: 350
  type: card

footer:
  contents:
    align: left
    applySinglePageCss: false
    markdown: |
      ## Jiwoo Son
      Copyright © 2022. All rights reserved.
  # sections:
  # - links:
  #   - link: https://gohugo.io/
  #     title: Docs
  #   - link: https://gohugo.io/
  #     title: Learn
  #   - link: https://gohugo.io/
  #     title: Showcase
  #   - link: https://gohugo.io/
  #     title: Blog
  #   title: General
  # - links:
  #   - link: https://gohugo.io/
  #     title: GitHub
  #   - link: https://gohugo.io/
  #     title: Releases
  #   - link: https://gohugo.io/
  #     title: Spectrum
  #   - link: https://gohugo.io/
  #     title: Telemetry
  #   title: resources
  # - links:
  #   - link: https://gohugo.io/
  #     title: GitHub
  #   - link: https://gohugo.io/
  #     title: Releases
  #   - link: https://gohugo.io/
  #     title: Spectrum
  #   - link: https://gohugo.io/
  #     title: Telemetry
  #   title: Features
  
--- 