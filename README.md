# 배포 파이프라인

## 다이어그램

![배포 파이프라인](https://i.postimg.cc/3r0j2FYg/deployment-diagram.png)

## Github Actions

### 1. 트리거 조건

- main 브랜치에 push 하면 배포 파이프라인이 실행된다.

### 2. 환경 설정

- Ubuntu 최신 버전에서 실행한다.

### 3. 배포 단계

- 코드 체크아웃

- 의존성 설치: `npm ci`

- 빌드: `npm run build`

- AWS 자격 증명 설정
  - AWS_ACCESS_KEY_ID
  - AWS_SECRET_ACCESS_KEY
  - AWS_REGION

### 4. AWS 배포

- S3 버킷에 배포

- CloudFront 캐시 무효화 : 새로운 버전을 적용하기 위해 캐시를 무효화한다.

## 보안

- AWS 자격 증명은 Github Secrets에 저장된다.

- 필요한 시크릿 키는 다음과 같다.
  - AWS_ACCESS_KEY_ID
  - AWS_SECRET_ACCESS_KEY
  - AWS_REGION
  - S3_BUCKET_NAME
  - CLOUDFRONT_DISTRIBUTION_ID

## 주요 링크

- [S3 버킷 웹사이트 엔드포인트](http://hanghae-taylor.s3-website-ap-southeast-2.amazonaws.com)

- [CloudFront 배포 도메인](https://dtmh4286aohmb.cloudfront.net)

## 주요 개념

- GitHub Actions과 CI/CD 도구
  : 코드 변경사항을 자동으로 감지하여 빌드, 테스트, 배포하는 자동화 프로세스를 제공한다. Github에서 제공하는 CI/CD 플랫폼이다.

- S3와 스토리지
  : 정적 웹사이트를 저장하고 제공하는 데 사용되는 객체 스토리지 서비스이다.

- CloudFront와 CDN
  : Amazon의 CDN(Content Delivery Network) 서비스이다. 콘텐츠를 캐싱하고 더 빠르게 제공하는 데 사용된다. 사용자와 가까운 곳에 캐시를 저장하여 더 빠른 콘텐츠 전송을 제공한다.

- 캐시 무효화(Cache Invalidation)
  : CloudFront의 캐시를 무효화하여 새로고침하는 프로세스이다. 새로운 버전을 적용하기 위해 캐시를 무효화한다.

- Repository secret과 환경변수
  : Github 저장소에 안전하게 저장되는 암호화된 환경변수이다. 민감한 정보를 코드에 노출하지 않고 안전하게 관리할 수 있다.

# CDN과 성능최적화

## 성능 측정

- 측정 도구 : Chrome DevTools Network tab, Lighthouse

- 측정 항목 : 로딩 시간, 네트워크 성능, 웹 바이탈

### S3 버킷 웹사이트

#### 네트워크 탭

![S3 버킷 웹사이트 네트워크 탭](https://i.postimg.cc/50nz10wS/Pasted-Graphic-4.png)

#### 라이트하우스

![S3 버킷 웹사이트 라이트하우스](https://i.postimg.cc/htT7vMLj/Pasted-Graphic-2.png)

### CloudFront 배포

#### 네트워크 탭

![CloudFront 배포 네트워크 탭](https://i.postimg.cc/PqHvwZyM/Pasted-Graphic-5.png)

#### 라이트하우스

![CloudFront 배포 라이트하우스](https://i.postimg.cc/SsNXZhw2/Pasted-Graphic-3.png)

## 성능 비교 분석

### 라이트하우스 성능 비교

| 측정 지표                | S3 직접 호스팅 | CloudFront | 개선율 |
| ------------------------ | -------------- | ---------- | ------ |
| Performance              | 84             | 100        | +19.0% |
| First Contentful Paint   | 0.8s           | 1.1s       | -37.5% |
| Speed Index              | 4.7s           | 1.3s       | +72.3% |
| Largest Contentful Paint | 4.0s           | 1.2s       | +70.0% |

## 개선 사항

### 응답 시간 개선

- cloudfront를 통해 받은 컨텐츠의 사이즈가 더 작고, 응답속도가 더 빠름.

### 사용자 경험 개선

- 전반적으로 First Contentful Paint를 제외한 모든 지표에서 큰 폭의 성능 향상.

## 결론

- CloudFront를 사용하여 콘텐츠 전송 속도를 높이고 사용자 경험을 개선할 수 있다.

- 캐싱 전략을 최적화하여 네트워크 부하를 줄이고 응답 시간을 개선할 수 있다.
