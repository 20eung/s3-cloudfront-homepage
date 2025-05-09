# AWS S3 버킷과 CloudFront로 HTTPS 홈페이지 만들기

## 1. SSL 인증서 ACM(AWS Certificate Manager)에 등록하기

**1. PEM 파일 준비하기**
- cert.pem: Certificate
- private.key: Private Key
- cert-chain.pem: Certificate Chain(Optional)

**2. 팁**
- 만약 cert.pem 파일에 인증서가 여러 개 등록되어 있는 경우 (Certificate Chain인 경우)
- 첫 번째 인증서 블록을 복사하여 cert.pem 파일로 저장
- 나머지 인증서 블록을 복사하여 cert-chain.pem 파일로 저장
  
**3. Certificate Manager에 등록하기**
- AWS Certificate Manager (ACM) 이동
- Import certificate
- Certificate body: cert.pem 내용 붙여넣기
- Certificate private key: private.key 내용 붙여넣기
- Certificate chain - optional: cert-chain.pem 내용 붙여넣기
![/Files/aws-acm-certificate-import.png](/Files/aws-acm-certificate-import.png)

**4. AWS CLI 명령어로 인증서 업로드하는 방법**
- AWS CloudShell 실행
- 인증서 파일 업로드
![/Files/aws-acm-certificate-upload-cloudshell.png](/Files/aws-acm-certificate-upload-cloudshell.png)
- 아래 명령어 실행
```
aws acm import-certificate \
--certificate fileb://cert.pem \
--private-key fileb://private.key \
--certificate-chain fileb://cert-chain.pem \
--region us-east-1
```

## 2. S3 버킷 생성

**1. S3 버팃을 정적 웹사이트로 사용 시 주의사항**
- S3 버킷 이름이 도메인과 정확히 일치해야 함. 예를 들어 www.abc.com 도메인용 웹사이트를 이용하려면 S3 버킷 이름도 www.abc.com 이어야 함.
- 정적 웹사이트 호스팅 활성화 및 index.html 파일을 업로드
- 버킷 퍼블릭 접근 및 정책 설정에 관하여
  - 기본적으로 HTTP(TCP 80)만 지원함
  - HTTPS(TCP 443)을 이용하는 방법 중 CloudFront를 사용할 수 있음
  - CloudFront 사용 시 **퍼블릭 접근을 제한**해야 함
  - CloudFront에서만 접근 가능한 정책 설정 필요

**2. S3 버킷 생성 완료 화면**

![/Files/aws-s3-buckets.png](/Files/aws-s3-buckets.png)

**3. S3 버킷 Object 업로드 완료 화면**

![/Files/aws-s3-buckets-objects.png](/Files/aws-s3-buckets-objects.png)

**4. S3 버킷 Properties 화면**

![/Files/aws-s3-buckets-properties1.png](/Files/aws-s3-buckets-properties1.png)

![/Files/aws-s3-buckets-properties2.png](/Files/aws-s3-buckets-properties2.png)

![/Files/aws-s3-buckets-properties3.png](/Files/aws-s3-buckets-properties3.png)

**5. S3 버킷으로 Static Website Hosting 설정 화면**

![/Files/aws-s3-buckets-properties-static-website-hosting.png](/Files/aws-s3-buckets-properties-static-website-hosting.png)

**6. S3 버킷 Permissions 화면**

![/Files/aws-s3-buckets-permissions.png](/Files/aws-s3-buckets-permissions.png)

**7. CloudFront Distribution 생성 화면**

![/Files/aws-cloudfront-distributions-create1.png](/Files/aws-cloudfront-distributions-create1.png)

![/Files/aws-cloudfront-distributions-create2.png](/Files/aws-cloudfront-distributions-create2.png)

**8. CloudFront Distribution General 화면**

![/Files/aws-cloudfront-distributions-general.png](/Files/aws-cloudfront-distributions-general.png)

**9. CloudFront Distribution Security 화면**

![/Files/aws-cloudfront-distributions-security.png](/Files/aws-cloudfront-distributions-security.png)

**10. CloudFront Distribution Origin 화면**

![/Files/aws-cloudfront-distributions-origins.png](/Files/aws-cloudfront-distributions-origins.png)

**11. CloudFront Distribution Behaviors 화면**

![/Files/aws-cloudfront-distributions-behaviors.png](/Files/aws-cloudfront-distributions-behaviors.png)

![/Files/aws-cloudfront-distributions-behaviors-edit.png](/Files/aws-cloudfront-distributions-behaviors-eidt.png)

**12. CloudFront Distribution Invalidations 화면**

![/Files/aws-cloudfront-distributions-create-invalidation-cloudshell.png](/Files/aws-cloudfront-distributions-create-invalidation-cloudshell.png)

