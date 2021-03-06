# 메모 - https://github.com/practice-golang
준비. 일단은 두서없이 쓰기...  

목표는 프론트: Electron, 백: golang을 사용하여 간단하게 CRUD만 되는 PC용 앱 제작
- [Rest API 본 뜰 대상](http://www.insiderclub.org/module/go/SlimFrameworkAPI){: target="_blank" }  
- 목표 달성: [Rest Client](https://github.com/practice-golang/rest-electron){: target="_blank" }, [Rest Server(MySQL)](https://github.com/practice-golang/rest-ql-crud){: target="_blank" }, [Rest Server(PostgreSQL)](https://github.com/practice-golang/rest-pgsql-crud){: target="_blank" }  

사용할 에디터는 [윈도우용 vscode 개조본](http://enjoytools.net/xe/board_vngc57/8978)  
* vscode에 go extension용 util(delve, gocode 등)과 gocache 격리, git, mingw(cgo), dep 관련 배치파일, upx 등을 포터블 형태로 짬뽕시켜놨다. ㅋ
* vscode 최신버전으로 업데이트 방법: tools/vscode 폴더에서 아래 항목들 빼고 모두 삭제 > 최신 파일로 덮어씌움
  * data 폴더
  * run_main.bat
  * run_vscode.bat
  * RunHiddenConsole.exe

## 강좌
* 고투어 국문 - <https://go-tour-kr.appspot.com>{: target="_blank" }
* 고투어 영문
  * <https://tour.golang.org>{: target="_blank" }
  * go가 설치되어 있다면 셸 또는 명령프롬프트에서 go tool tour

* Let's Go - <https://www.slideshare.net/songaal/lets-go-45867246>{: target="_blank" }
* 효과적인 Go 프로그래밍 국문 번역 - <https://gosudaweb.gitbooks.io/effective-go-in-korean/>{: target="_blank" }
* Go 프로그래밍 소개 국문 번역 - <http://codingnuri.com/golang-book/>{: target="_blank" }
* Go by example 국문 번역 - <https://mingrammer.com/gobyexample/>{: target="_blank" }
* Go 언어 웹 프로그래밍 철저 입문 - <https://thebook.io/006806/>{: target="_blank" }
* 예제로 배우는 GO 프로그래밍 - <http://golang.site/>{: target="_blank" }
* 가장빨리만나는Go - <http://pyrasis.com/go.html>{: target="_blank" }
* 언제나 휴일 - <http://ehpub.co.kr/%EA%B5%AC%EA%B8%80-go-%ED%95%98%EC%9E%90-1-%EA%B5%AC%EA%B8%80-go-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%96%B8%EC%96%B4-%EC%86%8C%EA%B0%9C/>{: target="_blank" }
* 30분 Go - <https://programmers.co.kr/learn/courses/13>{: target="_blank" }
* LibQA - <http://libqa.com/wiki/826>{: target="_blank" }
  
* Go 코드를 작성하는 방법 - <https://github.com/golang-kr/golang-doc/wiki/Go-코드를-작성하는-방법>{: target="_blank" }
  * 2017년 중순에 이렇게 한 번 시도했다가 포기한 적이 있다.

아래는 전부 영문
* Go Web Example - <https://gowebexamples.com/>{: target="_blank" }  
* Go Cookbook (Sumatra PDF 오쏘 / `예제로 배우는 Go 언어 활용법` 아님) - <https://blog.kowalczyk.info/book/go-cookbook.html>{: target="_blank" }  
* Essential Go (Sumatra PDF 오쏘) - <https://www.programming-books.io/essential/go/>{: target="_blank" }  


## 진짜로 메모
### 빌드
`go 1.11`부터 `module`이란게 생겼고, 쌔가 빠지게 `GOPATH`에 적응했는데 이제 다 필요없다.  
폴더에 소스 작성 후 아래와 같이 하면 끝이다.
```sh
$ go mod init
$ go build

# go mod tidy는 바뀐 의존성 적용할 때만 쓰면 된다.
```

### 빌드 (=< go 1.10 & dep)
윈도우용 VScode, go extention의 inferGopath 설정을 true로 해서 쓰고 ~~있다~~있었다.  
근데, 위 설정이 통합터미널에서는 안먹혀서 스크립트로 GOPATH=현재workspace로 강제하게 했다.  
맞는 방법인지 모르겠지만 GOPATH 루트 즉, bin, pkg, src 트리 전체를 프로젝트 단위로 격리시켜 쓰고 있는데,  
이 형태로 소스트리를 구성하니 아래의 과정으로 빌드와 소스 관리를 쉽게 할 수 있었다.  
단, Git 같은 곳에 라이브러리 등의 형태로 배포 시에는 방법을 달리 해야 한다.
* 깃헙(또는 빗버킷이나 깃랩)소스 땡겨와서 빌드하기
  * 예시: 나의 [helloworld 레포지터리](https://github.com/practice-golang/helloworld){: target="_blank" }
  * **go get 명령 사용 안 함 - 의존성은 dep, 소스 땡겨오기는 git clone 사용**
  * dep init은 main이던 아니던, 패키지별로 모두 실행해야 함  

```powershell
# 명령프롬프트 또는 vscode 터미널에서 아래와 같이 실행
cd [vscode workspace 루트]
git clone https://github.com/practice-golang/helloworld.git

# dep init
# - 아래 과정이 불편해서 배치파일로 만들어 쓰고 있다.
# - Shift + F2 > dep init을 선택하면 현재 열린 go 소스가 저장된 패키지(=폴더) 기준으로 의존 패키지를 다운로드해준다.
%프로젝트루트%/src/hello>cd ../..
%프로젝트루트%>set GOPATH=%cd%;%GOPATH%
%프로젝트루트%>cd src/hello
%프로젝트루트%/src/hello>dep init
# 또는
%프로젝트루트%>dep init ./src/hello

# vscode에서 helloworld 폴더를 작업영역으로 열고나서, vscode 터미널에서 아래와 같이 실행
go build hello
# 또는
go install hello
# 또는
go run ./src/hello/hello.go
```

### 패키지
#### =< 1.10
* [helloworld](https://github.com/practice-golang/helloworld){: target="_blank" }
#### go mod
* [go111module-pkghello](https://github.com/practice-golang/go111module-pkghello){: target="_blank" }
* [go111module-localpkg](https://github.com/practice-golang/go111module-localpkg){: target="_blank" }
* [Just for Func](https://github.com/campoy/embedmd){: target="_blank" }
    * 동영상 - https://www.youtube.com/watch?v=H_4eRD8aegk


### 변수
#### Type 생략
```go
myName := "Practice Golang"
age := 1
```

### 배열류
Map은 Thread safe하지 않다고 하는데 난 꼭 써야된다.
#### 가변 배열 느낌의 Slice
```go
func main() {
  var animals string
  animal := "dog"
  animals = append(animals, animal)
  animal = "cat"
  other_animal := "bear"
  animals = append(animals, animal, other_animal)

  fmt.Printf("%+v\n", books)
}
```

#### 다른 Type을 섞어 사용
interface를 쓰면 되는데 사용할 때는 type assertion 필요
```go
// Book : Book 정보
type Book struct {
	Title  string
	Author string
}

func main() {
  // var books []Book
  var books []interface{}
  book := Book{Title: "My First Book", Author: "Human"}
  books = append(books, book)
  book = Book{"My Second Book", "Animal"}
  what := "The Heck"
  books = append(books, book, what)

  fmt.Println(books[0].(Book).Title)
  fmt.Println(books[2].(string))
}
```

#### PHP vardump 또는 print_r 같은 출력
%+v 또는 spew 패키지 사용. 아니면 그냥 fmt.Println

```go
import (
	"fmt"

	"github.com/davecgh/go-spew/spew"
)

// Book : Book 정보
type Book struct {
	Title  string
	Author string
}

func main() {
	// var books []Book
	var books []interface{}
	book := Book{Title: "My First Book", Author: "Human"}
	books = append(books, book)
	book = Book{Title: "My Second Book", Author: "Animal"}
	what := "The Heck"
	books = append(books, book, what)

	// Vardump source: https://stackoverflow.com/questions/24512112/how-to-print-struct-variables-in-console
	fmt.Printf("%+v\n", books)
	spew.Dump(books)
}
```

끝.
