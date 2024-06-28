# Node.js

# 6.3. Node.js

이 문서에서는 Ubuntu 서버에서 Node.js 개발 환경을 구성하고, Node.js를 활용한 웹 애플리케이션 및 백엔드 서비스 개발 방법에 대해 자세히 설명합니다. Node.js는 Chrome V8 JavaScript 엔진을 기반으로 하는 서버 사이드 JavaScript 런타임으로, 비동기 I/O와 이벤트 기반 아키텍처를 통해 확장성 있는 네트워크 애플리케이션을 개발할 수 있습니다.

## 6.3.1. Node.js 설치

### 6.3.1.1. Node.js 버전 선택

Node.js는 LTS(Long-Term Support) 버전과 Current 버전으로 나뉩니다. 안정성과 장기 지원이 필요한 프로덕션 환경에서는 LTS 버전을 사용하는 것이 좋습니다.

### 6.3.1.2. Node.js 소스 리스트 추가

- Node.js 소스 리스트 추가:
    
    ```bash
    curl -sL <https://deb.nodesource.com/setup_14.x> | sudo -E bash -
    
    ```
    

위 명령어는 Node.js 14.x LTS 버전의 소스 리스트를 추가합니다. 다른 버전을 원할 경우 `setup_14.x` 부분을 원하는 버전으로 변경하세요.

### 6.3.1.3. Node.js 패키지 설치

- Node.js 패키지 설치:
    
    ```bash
    sudo apt-get install -y nodejs
    
    ```
    

위 명령어를 사용하여 Node.js 패키지를 설치합니다. 이 명령어는 Node.js 런타임과 npm(Node Package Manager)을 함께 설치합니다.

## 6.3.2. npm을 활용한 패키지 관리

### 6.3.2.1. 패키지 설치

- 프로젝트 디렉토리로 이동:
    
    ```bash
    cd /path/to/your/project
    
    ```
    
- 패키지 설치:
    
    ```bash
    npm install 패키지명
    
    ```
    
- 예시:
    
    ```bash
    npm install express
    
    ```
    

npm을 사용하여 Node.js 프로젝트에 필요한 패키지를 설치할 수 있습니다. 프로젝트 디렉토리로 이동한 후, `npm install` 명령어 뒤에 설치하고자 하는 패키지 이름을 입력하세요.

### 6.3.2.2. package.json 파일 생성

- package.json 파일 생성:
    
    ```bash
    npm init
    
    ```
    

`npm init` 명령어를 사용하여 대화형으로 package.json 파일을 생성할 수 있습니다. package.json 파일은 프로젝트의 메타데이터, 의존성 패키지 목록, 스크립트 등을 포함합니다.

### 6.3.2.3. 개발 의존성 패키지 설치

- 개발 의존성 패키지 설치:
    
    ```bash
    npm install 패키지명 --save-dev
    
    ```
    
- 예시:
    
    ```bash
    npm install nodemon --save-dev
    
    ```
    

개발 의존성 패키지는 프로덕션 환경에서는 필요하지 않고 개발 과정에서만 사용되는 패키지입니다. `--save-dev` 옵션을 사용하여 package.json 파일의 `devDependencies` 섹션에 패키지를 추가할 수 있습니다.

## 6.3.3. Express.js를 활용한 웹 애플리케이션 개발

### 6.3.3.1. Express.js 설치

- Express.js 설치:
    
    ```bash
    npm install express
    
    ```
    

Express.js는 Node.js를 위한 빠르고 간결한 웹 프레임워크입니다. `npm install express` 명령어를 사용하여 Express.js 패키지를 설치할 수 있습니다.

### 6.3.3.2. Express.js 애플리케이션 구조

- 기본 디렉토리 구조:
    
    ```
    project/
    ├── node_modules/
    ├── public/
    │   ├── css/
    │   ├── js/
    │   └── images/
    ├── routes/
    │   └── index.js
    ├── views/
    │   └── index.ejs
    ├── app.js
    └── package.json
    
    ```
    

Express.js 애플리케이션의 기본 디렉토리 구조입니다. `node_modules`는 설치된 패키지들이 저장되는 디렉토리이며, `public`은 정적 파일(CSS, JavaScript, 이미지 등)이 위치하는 디렉토리입니다. `routes`는 라우팅 로직을 포함하는 디렉토리이고, `views`는 템플릿 파일이 위치하는 디렉토리입니다. `app.js`는 Express.js 애플리케이션의 메인 파일이며, `package.json`은 프로젝트의 메타데이터와 의존성 정보를 포함합니다.

### 6.3.3.3. Express.js 라우팅

- 기본 라우팅 예시 (app.js):
    
    ```jsx
    const express = require('express');
    const app = express();
    
    app.get('/', (req, res) => {
      res.send('Hello, World!');
    });
    
    app.listen(3000, () => {
      console.log('Server is running on port 3000');
    });
    
    ```
    

Express.js의 라우팅은 HTTP 메서드(GET, POST, PUT, DELETE 등)와 URL 경로를 기반으로 이루어집니다. 위 예시에서는 루트 경로('/')에 대한 GET 요청에 대해 'Hello, World!'를 응답으로 전송합니다. `app.listen()` 메서드를 사용하여 지정된 포트(여기서는 3000)에서 서버를 실행합니다.

### 6.3.3.4. 미들웨어

- 미들웨어 예시 (app.js):
    
    ```jsx
    const express = require('express');
    const app = express();
    
    app.use((req, res, next) => {
      console.log(`Request received: ${req.method} ${req.url}`);
      next();
    });
    
    app.get('/', (req, res) => {
      res.send('Hello, World!');
    });
    
    app.listen(3000, () => {
      console.log('Server is running on port 3000');
    });
    
    ```
    

미들웨어는 요청과 응답 사이에서 실행되는 함수로, 요청 객체(req)와 응답 객체(res)에 접근할 수 있습니다. 위 예시에서는 `app.use()` 메서드를 사용하여 모든 요청에 대해 실행되는 미들웨어를 등록하고, 요청 정보를 로깅합니다. `next()` 함수를 호출하여 다음 미들웨어나 라우트 핸들러로 제어를 전달합니다.

## 6.3.4. PM2를 활용한 프로세스 관리

### 6.3.4.1. PM2 설치

- PM2 설치:
    
    ```bash
    npm install pm2 -g
    
    ```
    

PM2는 Node.js 애플리케이션을 관리하고 모니터링하는 데 사용되는 프로세스 관리자입니다. `-g` 옵션을 사용하여 PM2를 전역으로 설치할 수 있습니다.

### 6.3.4.2. 애플리케이션 실행

- 애플리케이션 실행:
    
    ```bash
    pm2 start app.js
    
    ```
    

`pm2 start` 명령어를 사용하여 Node.js 애플리케이션을 실행할 수 있습니다. PM2는 애플리케이션을 백그라운드에서 실행하고 프로세스를 관리합니다.

### 6.3.4.3. 애플리케이션 모니터링

- 실행 중인 애플리케이션 목록 확인:
    
    ```bash
    pm2 list
    
    ```
    
- 애플리케이션 상태 모니터링:
    
    ```bash
    pm2 monit
    
    ```
    

`pm2 list` 명령어를 사용하여 실행 중인 애플리케이션의 목록을 확인할 수 있습니다. `pm2 monit` 명령어를 사용하여 실시간으로 애플리케이션의 상태를 모니터링할 수 있습니다.

### 6.3.4.4. 애플리케이션 관리

- 애플리케이션 중지:
    
    ```bash
    pm2 stop app_name_or_id
    
    ```
    
- 애플리케이션 재시작:
    
    ```bash
    pm2 restart app_name_or_id
    
    ```
    
- 애플리케이션 삭제:
    
    ```bash
    pm2 delete app_name_or_id
    
    ```
    

PM2를 사용하여 애플리케이션을 중지, 재시작 또는 삭제할 수 있습니다. 애플리케이션 이름이나 ID를 사용하여 해당 작업을 수행합니다.

이 문서에서는 Ubuntu 서버에서 Node.js 개발 환경을 구성하고, Node.js를 활용한 웹 애플리케이션 및 백엔드 서비스 개발 방법에 대해 자세히 다루었습니다. Node.js 설치, npm을 활용한 패키지 관리, Express.js 프레임워크를 활용한 웹 애플리케이션 개발, PM2를 활용한 프로세스 관리 등의 주제를 포함하고 있습니다. 이 문서를 참조하여 Node.js 기반의 확장성 있고 효율적인 웹 애플리케이션과 서비스를 개발할 수 있을 것입니다.