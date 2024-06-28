# PHP

# 6.4. PHP

이 문서에서는 Ubuntu 서버에서 PHP 개발 환경을 구성하고, PHP를 활용한 웹 애플리케이션 개발 방법에 대해 자세히 설명합니다. PHP는 서버 사이드 스크립팅 언어로, 동적 웹 페이지 및 웹 애플리케이션 개발에 널리 사용됩니다. 이 문서에서는 PHP 설치, PHP 버전 관리, Laravel 프레임워크를 활용한 웹 애플리케이션 개발, Composer를 활용한 패키지 관리 등의 주제를 다룹니다.

## 6.4.1. PHP 설치

### 6.4.1.1. PHP 패키지 설치

- PHP 패키지 설치:
    
    ```bash
    sudo apt update
    sudo apt install php
    
    ```
    

Ubuntu 서버에서 `apt` 명령어를 사용하여 PHP 패키지를 설치할 수 있습니다. 기본적으로 최신 안정 버전의 PHP가 설치됩니다.

### 6.4.1.2. PHP 모듈 설치

- 일반적으로 사용되는 PHP 모듈 설치:
    
    ```bash
    sudo apt install php-cli php-fpm php-json php-common php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath
    
    ```
    

PHP 애플리케이션 개발에 필요한 다양한 모듈을 설치할 수 있습니다. 위 명령어는 일반적으로 사용되는 모듈들을 설치합니다.

### 6.4.1.3. PHP 버전 확인

- PHP 버전 확인:
    
    ```bash
    php -v
    
    ```
    

`php -v` 명령어를 사용하여 설치된 PHP의 버전을 확인할 수 있습니다.

## 6.4.2. PHP 버전 관리

### 6.4.2.1. 여러 PHP 버전 설치

- PPA 저장소 추가:
    
    ```bash
    sudo add-apt-repository ppa:ondrej/php
    sudo apt update
    
    ```
    
- 특정 PHP 버전 설치:
    
    ```bash
    sudo apt install php7.4 php7.4-cli php7.4-fpm
    
    ```
    

웹 애플리케이션에 따라 다양한 PHP 버전이 요구될 수 있습니다. Ondřej Surý의 PPA 저장소를 추가하면 여러 PHP 버전을 설치할 수 있습니다.

### 6.4.2.2. PHP 버전 전환

- update-alternatives를 사용하여 PHP 버전 전환:
    
    ```bash
    sudo update-alternatives --config php
    
    ```
    

`update-alternatives` 명령어를 사용하여 시스템에서 사용할 기본 PHP 버전을 선택할 수 있습니다. 설치된 PHP 버전 목록이 표시되며, 원하는 버전의 번호를 입력하여 선택합니다.

## 6.4.3. Laravel 프레임워크

### 6.4.3.1. Composer 설치

- Composer 설치:
    
    ```bash
    curl -sS <https://getcomposer.org/installer> | php
    sudo mv composer.phar /usr/local/bin/composer
    
    ```
    

Laravel은 PHP의 대표적인 웹 프레임워크 중 하나입니다. Laravel을 사용하기 위해서는 먼저 Composer(PHP 패키지 관리자)를 설치해야 합니다.

### 6.4.3.2. Laravel 프로젝트 생성

- 새로운 Laravel 프로젝트 생성:
    
    ```bash
    composer create-project --prefer-dist laravel/laravel 프로젝트명
    
    ```
    

`composer create-project` 명령어를 사용하여 새로운 Laravel 프로젝트를 생성할 수 있습니다. 프로젝트명 부분에 원하는 프로젝트 이름을 입력하세요.

### 6.4.3.3. Laravel 개발 서버 실행

- Laravel 개발 서버 실행:
    
    ```bash
    cd 프로젝트명
    php artisan serve --host=0.0.0.0 --port=8000
    
    ```
    

Laravel 프로젝트 디렉토리로 이동한 후, `php artisan serve` 명령어를 사용하여 개발 서버를 실행할 수 있습니다. `--host` 옵션으로 서버의 IP 주소를 지정하고, `--port` 옵션으로 포트 번호를 설정합니다.

### 6.4.3.4. 라우팅 및 컨트롤러

- 라우트 정의 (routes/web.php):
    
    ```php
    Route::get('/', function () {
        return view('welcome');
    });
    
    Route::get('/hello', 'HelloController@index');
    
    ```
    
- 컨트롤러 생성 (app/Http/Controllers/HelloController.php):
    
    ```php
    <?php
    
    namespace App\\Http\\Controllers;
    
    use Illuminate\\Http\\Request;
    
    class HelloController extends Controller
    {
        public function index()
        {
            return 'Hello, World!';
        }
    }
    
    ```
    

Laravel에서 라우팅은 `routes/web.php` 파일에 정의됩니다. 클로저 함수를 사용하여 간단한 로직을 처리하거나, 컨트롤러의 메서드를 호출할 수 있습니다. 컨트롤러는 `app/Http/Controllers` 디렉토리에 위치하며, 비즈니스 로직을 구현합니다.

### 6.4.3.5. 뷰 및 블레이드 템플릿

- 뷰 생성 (resources/views/hello.blade.php):
    
    ```php
    <!DOCTYPE html>
    <html>
    <head>
        <title>Hello</title>
    </head>
    <body>
        <h1>{{ $message }}</h1>
    </body>
    </html>
    
    ```
    
- 컨트롤러에서 뷰 반환 (app/Http/Controllers/HelloController.php):
    
    ```php
    public function index()
    {
        $message = 'Hello, World!';
        return view('hello', ['message' => $message]);
    }
    
    ```
    

Laravel은 블레이드 템플릿 엔진을 사용하여 뷰를 구성합니다. 뷰 파일은 `resources/views` 디렉토리에 위치하며, `.blade.php` 확장자를 가집니다. 컨트롤러에서 `view()` 함수를 사용하여 뷰를 반환하고, 필요한 데이터를 전달할 수 있습니다.

## 6.4.4. Composer를 활용한 패키지 관리

### 6.4.4.1. 패키지 설치

- 프로젝트 디렉토리로 이동:
    
    ```bash
    cd /path/to/your/project
    
    ```
    
- 패키지 설치:
    
    ```bash
    composer require 패키지명
    
    ```
    
- 예시:
    
    ```bash
    composer require guzzlehttp/guzzle
    
    ```
    

Composer를 사용하여 PHP 프로젝트에 필요한 패키지를 설치할 수 있습니다. 프로젝트 디렉토리로 이동한 후, `composer require` 명령어 뒤에 설치하고자 하는 패키지 이름을 입력하세요.

### 6.4.4.2. 패키지 업데이트

- 패키지 업데이트:
    
    ```bash
    composer update
    
    ```
    

`composer update` 명령어를 사용하여 프로젝트의 의존성 패키지를 최신 버전으로 업데이트할 수 있습니다.

### 6.4.4.3. 패키지 자동로딩

- 패키지 자동로딩 설정 (composer.json):
    
    ```json
    {
        "autoload": {
            "psr-4": {
                "App\\\\": "app/"
            }
        }
    }
    
    ```
    
- 자동로딩 파일 생성:
    
    ```bash
    composer dump-autoload
    
    ```
    

Composer의 오토로딩 기능을 사용하면 클래스를 쉽게 로드할 수 있습니다. `composer.json` 파일의 `autoload` 섹션에 PSR-4 표준에 따른 네임스페이스와 디렉토리 매핑을 설정하고, `composer dump-autoload` 명령어로 자동로딩 파일을 생성합니다.

이 문서에서는 Ubuntu 서버에서 PHP 개발 환경을 구성하고, PHP를 활용한 웹 애플리케이션 개발 방법에 대해 자세히 다루었습니다. PHP 설치, 버전 관리, Laravel 프레임워크를 활용한 웹 애플리케이션 개발, Composer를 활용한 패키지 관리 등의 주제를 포함하고 있습니다. 이 문서를 참조하여 PHP 기반의 웹 애플리케이션을 효과적으로 개발하고 관리할 수 있을 것입니다.

```

```