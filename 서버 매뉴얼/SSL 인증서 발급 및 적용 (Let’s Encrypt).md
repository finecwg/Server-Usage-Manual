# SSL 인증서 발급 및 적용 (Let’s Encrypt)

# 8.5. SSL 인증서 발급 및 적용 (Let's Encrypt)

웹 사이트의 보안을 강화하고 사용자의 신뢰를 얻기 위해서는 SSL/TLS 인증서를 적용하는 것이 필수적입니다. Let's Encrypt는 무료로 SSL/TLS 인증서를 발급해주는 인증 기관으로, 이 문서에서는 Let's Encrypt를 사용하여 인증서를 발급받고 웹 서버에 적용하는 방법에 대해 자세히 설명하겠습니다.

## Let's Encrypt 개요

Let's Encrypt는 비영리 인증 기관으로, 무료로 SSL/TLS 인증서를 발급하여 웹 사이트의 보안을 향상시키는 것을 목표로 합니다. 주요 특징은 다음과 같습니다:

- 무료: Let's Encrypt 인증서는 무료로 발급받을 수 있습니다.
- 자동화: 인증서 발급 및 갱신 과정을 자동화할 수 있습니다.
- 표준 준수: Let's Encrypt 인증서는 표준 브라우저와 호환됩니다.
- 단기 유효 기간: 인증서의 유효 기간은 90일로, 자주 갱신해야 합니다.

## Certbot 설치

Certbot은 Let's Encrypt 인증서를 발급받고 관리하기 위한 공식 클라이언트 도구입니다. 다음 단계에 따라 Certbot을 설치하세요:

1. Certbot 저장소 추가:
    
    ```
    sudo apt-get update
    sudo apt-get install software-properties-common
    sudo add-apt-repository universe
    sudo add-apt-repository ppa:certbot/certbot
    sudo apt-get update
    
    ```
    
2. Certbot 설치:
    
    ```
    sudo apt-get install certbot python3-certbot-apache
    
    ```
    

## SSL 인증서 발급

Certbot을 사용하여 Let's Encrypt SSL 인증서를 발급받는 방법은 다음과 같습니다:

1. 인증서 발급 명령 실행:
    
    ```
    sudo certbot --apache -d example.com -d www.example.com
    
    ```
    
    위 명령에서 `example.com`과 `www.example.com`을 실제 도메인으로 대체하세요.
    
2. 이메일 주소 입력:
    
    Certbot이 인증서 만료 및 갱신 관련 알림을 보낼 이메일 주소를 입력합니다.
    
3. 이용 약관 동의:
    
    Let's Encrypt 이용 약관에 동의해야 합니다.
    
4. HTTP 또는 HTTPS 선택:
    
    Certbot이 웹 서버 구성을 자동으로 수정하여 HTTPS를 사용하도록 할 것인지 선택합니다.
    
5. 인증서 발급 확인:
    
    인증서 발급이 완료되면 Certbot이 성공 메시지를 출력합니다.
    

## SSL 인증서 자동 갱신

Let's Encrypt 인증서는 90일 동안만 유효하므로, 인증서를 자동으로 갱신하도록 설정하는 것이 좋습니다.

1. 자동 갱신을 위한 Cron 작업 추가:
    
    ```
    sudo crontab -e
    
    ```
    
    파일의 끝에 다음 내용을 추가합니다:
    
    ```
    0 0 1 * * /usr/bin/certbot renew --quiet
    
    ```
    
    이 Cron 작업은 매월 1일 자정에 인증서 갱신을 시도합니다.
    
2. 갱신 테스트:
    
    ```
    sudo certbot renew --dry-run
    
    ```
    
    이 명령은 인증서 갱신 프로세스를 테스트하지만 실제로 인증서를 갱신하지는 않습니다.
    

## SSL 인증서 적용 확인

SSL 인증서가 올바르게 적용되었는지 확인하려면 다음 단계를 수행하세요:

1. 웹 브라우저에서 웹 사이트 열기:
    
    `https://`로 시작하는 URL을 사용하여 웹 사이트에 액세스합니다.
    
2. SSL 인증서 정보 확인:
    
    브라우저의 주소 표시줄에 자물쇠 아이콘이 표시되는지 확인합니다. 자물쇠 아이콘을 클릭하여 인증서 정보를 확인할 수 있습니다.
    
3. SSL 검사 도구 사용:
    
    SSL Labs ([https://www.ssllabs.com/ssltest/](https://www.ssllabs.com/ssltest/))와 같은 온라인 도구를 사용하여 SSL 인증서와 웹 서버 구성을 검사할 수 있습니다.
    

## 결론

이 문서에서는 Let's Encrypt를 사용하여 무료로 SSL/TLS 인증서를 발급받고 웹 서버에 적용하는 방법에 대해 설명했습니다. Certbot 클라이언트를 사용하면 인증서 발급 과정을 간편하게 자동화할 수 있습니다. 또한 인증서 갱신을 자동화하고 인증서 적용을 확인하는 방법도 알아보았습니다. SSL/TLS 인증서를 적용하면 웹 사이트의 보안을 강화하고 사용자의 신뢰를 얻을 수 있습니다. 이 문서의 내용을 참고하여 Let's Encrypt 인증서를 발급받고 웹 서버에 적용해보세요.