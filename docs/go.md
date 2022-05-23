# Go

This rst document is generated programatically from https://github.com/gsweene2/garrett-learns-go/blob/master/convert-to-md.py

## add-three-numbers.go

### Example

```go
package main

func AddThreeNumbers(n1 int, n2 int, n3 int) int {
    return n1 + n2 + n3
}

```


## add-three-numbers_test.go

### Example

```go
package main

import "testing"

func AddThreeNumbersTest(t *testing.T) {
    n1 := 3
    n2 := 4
    n3 := 5
    got := AddThreeNumbers(n1, n2, n3)
    want := 12

    if got != want {
        t.Errorf("got %q want %q", got, want)
    }
}

```


## concat-strings.go

### Example

```go
package main

func ConcatStrings(s1 string, s2 string) string {
    return s1 + s2
}

```


## concat-strings_test.go

### Example

```go
package main

import "testing"

func ConcatStringsTest(t *testing.T) {
    s1 := "Concat"
    s2 := "enation"
    got := ConcatStrings(s1,s2)
    want := "Concatenation"

    if got != want {
        t.Errorf("got %q want %q", got, want)
    }
}

```


## hello-world.go

### Example

```go
package main

import "fmt"

func Hello() string {
    return "Hello, Garrett"
}

```


## hello-world_test.go

### Example

```go
package main

import "testing"

func TestHello(t *testing.T) {
    got := Hello()
    want := "Hello, Garrett"

    if got != want {
        t.Errorf("got %q want %q", got, want)
    }
}

```


## write-to-file.go

### Example

```go
package main

import (
	"fmt"
	"os"
)

func check(e error) {
	if e != nil {
		panic(e)
	}
}

func WriteToFile() {
	filePath := "/Users/garrettsweeney/git/garrett-learns-go/write-to-file.txt"
	f, err := os.Create(filePath)
	check(err)

	defer f.Close()

	bytesWritten, err := f.WriteString("I can write to a file\n")
	check(err)
	fmt.Printf("Wrote %d bytes to %s\n", bytesWritten, filePath)

	f.Sync()
}

// func main() {
//     WriteToFile()
// }
```


## write-to-file.txt

### Example

```go
I can write to a file
```


