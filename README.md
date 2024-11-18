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
