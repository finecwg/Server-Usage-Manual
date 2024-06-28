# Go

# 6.5. Go

이 문서에서는 Ubuntu 서버에서 Go 프로그래밍 언어 개발 환경을 구성하고, Go를 활용한 애플리케이션 및 서비스 개발 방법에 대해 자세히 설명합니다. Go는 Google에서 개발한 오픈소스 프로그래밍 언어로, 간결한 문법, 높은 성능, 강력한 동시성 처리 등의 장점을 가지고 있습니다. 이 문서에서는 Go 설치, 개발 환경 설정, 패키지 관리, Gin 웹 프레임워크를 활용한 웹 애플리케이션 개발 등의 주제를 다룹니다.

## 6.5.1. Go 설치

### 6.5.1.1. Go 바이너리 다운로드

- Go 공식 웹사이트에서 바이너리 다운로드:
    
    ```bash
    wget <https://golang.org/dl/go1.18.linux-amd64.tar.gz>
    
    ```
    

Go 공식 웹사이트([https://golang.org](https://golang.org/))에서 최신 버전의 Go 바이너리 아카이브를 다운로드합니다. 위 예시에서는 Go 1.18 버전을 사용하였습니다.

### 6.5.1.2. Go 바이너리 설치

- 다운로드한 아카이브 파일 압축 해제:
    
    ```bash
    sudo tar -C /usr/local -xzf go1.18.linux-amd64.tar.gz
    
    ```
    
- PATH 환경 변수 설정:
    
    ```bash
    echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.bashrc
    source ~/.bashrc
    
    ```
    

다운로드한 Go 바이너리 아카이브 파일의 압축을 해제하고, `/usr/local` 디렉토리에 설치합니다. 그리고 `PATH` 환경 변수에 Go 바이너리 경로를 추가하여 시스템 전역에서 Go 명령어를 사용할 수 있도록 합니다.

### 6.5.1.3. Go 버전 확인

- Go 버전 확인:
    
    ```bash
    go version
    
    ```
    

설치된 Go의 버전을 확인하려면 `go version` 명령어를 실행합니다.

## 6.5.2. Go 개발 환경 설정

### 6.5.2.1. GOPATH 설정

- GOPATH 환경 변수 설정:
    
    ```bash
    echo "export GOPATH=$HOME/go" >> ~/.bashrc
    echo "export PATH=$PATH:$GOPATH/bin" >> ~/.bashrc
    source ~/.bashrc
    
    ```
    

Go는 `GOPATH` 환경 변수를 사용하여 프로젝트의 위치를 지정합니다. `GOPATH` 환경 변수를 설정하고, `PATH`에 `$GOPATH/bin`을 추가하여 Go 프로젝트에서 빌드된 실행 파일을 시스템 전역에서 사용할 수 있도록 합니다.

### 6.5.2.2. Go 작업 디렉토리 생성

- 작업 디렉토리 생성:
    
    ```bash
    mkdir -p $GOPATH/src/github.com/yourusername/yourproject
    
    ```
    

Go 프로젝트를 위한 작업 디렉토리를 생성합니다. 일반적으로 `$GOPATH/src` 아래에 깃허브 사용자 이름과 프로젝트 이름을 조합하여 디렉토리를 생성합니다.

## 6.5.3. Go 패키지 관리

### 6.5.3.1. Go 모듈 사용

- Go 모듈 초기화:
    
    ```bash
    cd $GOPATH/src/github.com/yourusername/yourproject
    go mod init github.com/yourusername/yourproject
    
    ```
    

Go 1.11 버전부터 도입된 Go 모듈을 사용하여 패키지 종속성을 관리할 수 있습니다. 프로젝트 디렉토리로 이동한 후, `go mod init` 명령어를 실행하여 모듈을 초기화합니다.

### 6.5.3.2. 패키지 import 및 다운로드

- 패키지 import:
    
    ```go
    package main
    
    import (
        "fmt"
        "github.com/gin-gonic/gin"
    )
    
    ```
    
- 패키지 다운로드:
    
    ```bash
    go get -u github.com/gin-gonic/gin
    
    ```
    

Go 소스 코드에서 사용할 패키지를 import하고, `go get` 명령어를 사용하여 해당 패키지를 다운로드할 수 있습니다. 위 예시에서는 Gin 웹 프레임워크 패키지를 import하고 다운로드하는 과정을 보여줍니다.

## 6.5.4. Gin 웹 프레임워크

### 6.5.4.1. Gin 설치

- Gin 패키지 다운로드:
    
    ```bash
    go get -u github.com/gin-gonic/gin
    
    ```
    

Gin은 Go로 작성된 인기 있는 웹 프레임워크 중 하나입니다. `go get` 명령어를 사용하여 Gin 패키지를 다운로드하고 설치할 수 있습니다.

### 6.5.4.2. Gin 웹 서버 예제

- Gin 웹 서버 예제 코드 (main.go):
    
    ```go
    package main
    
    import (
        "net/http"
        "github.com/gin-gonic/gin"
    )
    
    func main() {
        router := gin.Default()
    
        router.GET("/", func(c *gin.Context) {
            c.JSON(http.StatusOK, gin.H{
                "message": "Hello, World!",
            })
        })
    
        router.Run(":8080")
    }
    
    ```
    

위의 예제 코드는 Gin을 사용하여 간단한 웹 서버를 구현한 것입니다. `/` 경로로 GET 요청이 오면 JSON 형식으로 "Hello, World!" 메시지를 응답합니다.

### 6.5.4.3. 웹 서버 실행

- 웹 서버 실행:
    
    ```bash
    go run main.go
    
    ```
    

`go run` 명령어를 사용하여 작성한 Go 코드를 실행할 수 있습니다. 위 예제에서는 `main.go` 파일을 실행하여 웹 서버를 시작합니다.

### 6.5.4.4. 라우팅 및 핸들러

- 라우팅 및 핸들러 예제 코드 (main.go):
    
    ```go
    package main
    
    import (
        "net/http"
        "github.com/gin-gonic/gin"
    )
    
    func main() {
        router := gin.Default()
    
        router.GET("/hello/:name", func(c *gin.Context) {
            name := c.Param("name")
            c.String(http.StatusOK, "Hello, %s!", name)
        })
    
        router.POST("/data", func(c *gin.Context) {
            data := c.PostForm("data")
            c.JSON(http.StatusOK, gin.H{
                "received": data,
            })
        })
    
        router.Run(":8080")
    }
    
    ```
    

Gin에서는 `router.GET()`, `router.POST()` 등의 메서드를 사용하여 라우팅을 정의하고, 해당 경로에 대한 핸들러 함수를 등록할 수 있습니다. 위 예제에서는 `/hello/:name` 경로로 GET 요청이 오면 URL 파라미터를 사용하여 인사말을 반환하고, `/data` 경로로 POST 요청이 오면 폼 데이터를 받아 JSON 형식으로 응답합니다.

이 문서에서는 Ubuntu 서버에서 Go 개발 환경을 구성하고, Go를 활용한 애플리케이션 및 서비스 개발 방법에 대해 자세히 다루었습니다. Go 설치, 개발 환경 설정, 패키지 관리, Gin 웹 프레임워크를 활용한 웹 애플리케이션 개발 등의 주제를 포함하고 있습니다. 이 문서를 참조하여 Go 기반의 효율적이고 확장 가능한 애플리케이션을 개발할 수 있을 것입니다.