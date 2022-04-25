---
title: "About"
permalink: /about/
layout: single

author_profile: true
toc: true
toc_sticky: true

date: 2021-07-08 15:00 +0900
last_modified_at: 2022-04-25 12:20 +0900

---
# 1. Introduce

한국해양대학교 전기전자공학부에서 전자통신공학을 전공한 대한민국 표준형 공돌이입니다 (아쉽게도 남녀공학 중/고등학교 졸업).<br/><br/>
학부생 시절엔 __Java__ , __Python__ 등의 언어를 사용하며 꾸준히 알고리즘을 개발해 왔고, 2회의 논문, 3회의 경진대회 출전 후 3회의 수상 경험이 있습니다.<br/><br/>
뒤이어, 졸업 후 __Backend__ 와 __DB__, __DevOps__ 의 넓은 분야에 흥미를 갖고 계속해서 공부를 이어나가는 경력 ~1년 차 Backend Junior 개발자입니다!<br/><br/>
현재 다니엘 프로젝트 플랫폼팀에서 __Django Rest Framework__ 를 활용해 플랫폼 개발, 관리, 통합을 진행하고 있고, 지속적인 성능 향상을 위해 매일 코드 리뷰를 진행하며 nginx 튜닝, db 트렌젝션을 도입하고 있습니다.<br/><br/>
2022년 3월을 기준으로 AWS SA, SQLD 자격증을 공부 중이며, 취득 후 서버/DB 분야의 전문가, 더 나아가 Full-Stack의 목표를 갖고 바쁘게 살아가고 있습니다.
<br/><br/>

# 2. Interests

## 1. Back-end
 - Python, Java
 - Django, Django Rest Framework
 - Celery, RabbitMQ, Redis

## 2. Web Server / Hosting / OS
 - nginx, Gunicorn
 - Docker, compose
 - AWS (EC2, ELB, Route53, RDS, S3, ElastiCache, AmazonMQ)
 - Ubuntu 16.04, CentOS 7

## 3. DB
 - MySQL
 - Redis

## 4. Dev-Ops, CI/CD
 - AWS (Lambda, CloudWatch)
 - Slack
 - Github Webhook, Jenkins

## 5. Front-end
 - HTML, CSS, JS
 - AWS (CloudFront, Amplify, S3)
 - Firebase

## 6. ETC
 - Git, Github
 - OpenAPI (Swagger, redoc)

<br/><br/>

# 3. Projects

## 1. "야, 너도 드럼 칠 수 있어". - "어나더레벨" 조 (한국해양대학교)

입력된 음성 소스를 FFT를 통해 주파수 분석, 악보를 출력하는 SW 개발 [(Source)](https://github.com/bye0nys/drum-final)
 - FFT, Sliding Window 기법 융합, 처리 속도 최적화 로직 개발
 - 드럼 연주를 시각화, 가시성 향상에 기여 (LilyPond 등)
 - Numpy, Sci-Py를 활용한 신호 처리 최적화

## 2. "Wi-Fi FTM를 활용한 실내 위치 측위" - "테라헤르츠" 조 (한국해양대학교)

Wi-Fi FTM을 활용하여 실내 사용자의 위치를 3차원 좌표로 추정하는 알고리즘 개발 [(Source)](https://github.com/bye0nys/WiFi-ML)
 - IMU 센서 (자이로스코프, 지자계, 가속도), Wi-Fi FTM 프로토콜 활용 3차원 좌표 추정 애플리케이션 개발 (배포 X)
 - 선형 회귀, 가우스-뉴턴법 구현, 평균 오차율을 3.6m -> 1.1m 수준으로 감소
 - PDR 측위를 융합, 최종 오차율을 0.6m 수준으로 감소
<br/><br/>

## 3. "EnSound Lite API 개발 프로젝트" - "다니엘프로젝트"

음악 교육 플랫폼 "인사운드 라이트" RESTful API 개발, Serverless 호스팅 구현 [인사운드 라이트 API(비공개)](https://lite.ensound.net/)
 - DB 모델링, ERD 설계, DB 마이그레이션 (MySQL -> MySQL) 진행
 - AWS (EC2, RDS, S3), Docker (Compose), nginx 도입, 서버 호스팅 설계/구현
 - Celery, AWS ElastiCache, AmazonMQ 도입, 비동기 작업 처리, 캐시 백엔드 구현
 - OpenAPI (Swagger) 도입 후 API 문서화, JWT, OAuth2 도입 후 인증/미들웨어 설계/구현
 - AWS CloudWatch, TDD (Unit Test) 도입, HTTP 499, 50x 에러 대응 로직 설계/구현
 - CI/CD (Jenkins, Github Webhook), Slack API 도입, 에러 모니터링 채널 구현

<br/><br/>


# 4. Career


## 2021.08 ~
### 다니엘 프로젝트 (서울특별시 서초구)
- Backend 개발 __매니저__

<br/>

## 2015.03 ~ 2021.02
### 한국해양대학교 (부산광역시 영도구)
- 전기전자공학부 전자통신공학전공 학사 __졸업__

<br/>

## 2012.03 ~ 2015.02
### 백운고등학교 (경기도 의왕시)
- 과학중점계열 졸업

<br/><br/>

# 5. Others

## 2019.06 ~ 2019.09
 - "야, 너도 드럼 칠 수 있어" 프로젝트
### 2019.09 __최우수상__
2019 KMOU 다학제융합 캡스톤디자인 경진대회, 한국해양대학교 공학교육혁신센터

<br/>

## 2019.09 ~ 2020.06
 - "Wi-Fi FTM를 활용한 실내 위치 측위" 프로젝트
### 2020.07 __우수논문발표상__
 - 2020 ICT 대학생 논문 경진대회, (사)대한임베디드공학회
### 2020.11 __장려상__
 - 2020년도 하계종합학술발표회 학부 논문경진대회, 한국통신학회
 - 박지훈, 이세진, 강주리 외 4명(2020), [*Wi-Fi FTM과 스마트폰 IMU 센서를 활용한 실내 측위 연구*](https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE10499055), 한국통신학회, 한국통신학회 학술대회논문집, 1329 - 1330 (2 페이지)
<br/>

<br/>

## 2020.08
 - __무선설비기사__ 자격증 취득

<br/>