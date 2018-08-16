# 참고

준비. 일단은 두서없이 쓰기...

## 강좌
* 고투어 국문 - <https://go-tour-kr.appspot.com>{: target="_blank" }
* 고투어 영문
  * <https://tour.golang.org>{: target="_blank" }
  * go가 설치되어 있다면 셸 또는 명령프롬프트에서 go tool tour

* Go 프로그래밍 소개 국문 번역 - <http://codingnuri.com/golang-book/>{: target="_blank" }
* Go by example 국문 번역 - <https://mingrammer.com/gobyexample/>{: target="_blank" }
* Go 언어 웹 프로그래밍 철저 입문 - <https://thebook.io/006806/>{: target="_blank" }
* 예제로 배우는 GO 프로그래밍 - <http://golang.site/>{: target="_blank" }
* 가장빨리만나는Go - <http://pyrasis.com/go.html>{: target="_blank" }
* 30분 Go - <https://programmers.co.kr/learn/courses/13>{: target="_blank" }

### 변수
#### Type 생략
```go
myName := "Practice Golang"
age := 1
```

### 배열류
Map은 Thread safe하지 않다고 하니 꼭 필요할 때만 사용
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

#### PHP vardump 또는 print_r 같은 일괄 출력
%+v 또는 spew 패키지 사용

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
