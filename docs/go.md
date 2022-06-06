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


## aws-sdk-s3.go

### Example

```go
package main

import (
	"fmt"

	"github.com/aws/aws-sdk-go/aws/session"
	"github.com/aws/aws-sdk-go/service/s3"
)

func GetS3Client() *s3.S3 {
	sess := session.Must(session.NewSessionWithOptions(session.Options{
		SharedConfigState: session.SharedConfigEnable,
	}))

	svc := s3.New(sess)

	return svc
}

/*
	S3ServiceInterface created for the purpose of testing
	When testing AwsSdkCountObjectsInBucket, I need to mock the `svc` parameter
	If I defined `svc` of type *s3.S3, all *s3.S3 functions need to be mocked
	By creating an interface, I can only define functions I use
	Thus, only the functions I use have to be mocked
	And an argument of type *s3.S3 would satisfy the interface
*/
type S3ServiceInterface interface {
	ListObjectsV2(input *s3.ListObjectsV2Input) (*s3.ListObjectsV2Output, error)
}

func AwsSdkCountObjectsInBucket(svc S3ServiceInterface, bucket string) (int, error) {

	resp, err := svc.ListObjectsV2(&s3.ListObjectsV2Input{Bucket: &bucket})

	if err != nil {
		fmt.Printf("Error Listing Objects: %v\n", err)
		return -1, err
	}

	fmt.Printf("len(resp.Contents): %v\n", len(resp.Contents))
	return len(resp.Contents), nil
}

func main() {
	svc := GetS3Client()
	bucket := "garretts-s3-bucket-name"
	AwsSdkCountObjectsInBucket(svc, bucket)
}
```


## aws-sdk-s3_test.go

### Example

```go
package main

import (
	"testing"

	"github.com/aws/aws-sdk-go/service/s3"
)

type mockS3Svc struct{}

func (m *mockS3Svc) ListObjectsV2(input *s3.ListObjectsV2Input) (*s3.ListObjectsV2Output, error) {
	name := "someBucket"
	return &s3.ListObjectsV2Output{
		Name: &name,
	}, nil
}

func TestAwsSdkCountObjectsInBucket(t *testing.T) {
	// Arrange
	// Create mockS3Svc & ListObjectsV2
	want := 1

	// Act
	mockSvc := mockS3Svc{}
	got, err := AwsSdkCountObjectsInBucket(&mockSvc, "someBucket")

	// Assert
	if err != nil {
		t.Error("Error returned")
	}

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
	got := ConcatStrings(s1, s2)
	want := "Concatenation"

	if got != want {
		t.Errorf("got %q want %q", got, want)
	}
}
```


## diff-library.go

### Example

```go
package main

import (
	"fmt"

	"github.com/r3labs/diff"
)

func DiffLibrary() diff.Changelog {
	from := make(map[string]map[string]string)
	item_a_from := map[string]string{"hello": "world"}
	from["a"] = item_a_from
	to := make(map[string]map[string]string)
	item_a_to := map[string]string{"hello": "world1"}
	to["a"] = item_a_to

	changelog, err := diff.Diff(from, to)
	if err != nil {
		fmt.Printf("err: %v\n", err)
	}
	return changelog
}

// func main() {
// 	result := DiffLibrary()
// 	fmt.Printf("result: %v\n", result)
// 	b, err := json.Marshal(result)
// 	if err != nil {
// 		fmt.Println(err)
// 		return
// 	}
// 	fmt.Printf("result as json: %v\n", result)
// 	fmt.Println(string(b))
// }
```


## hello-world.go

### Example

```go
package main

import "fmt"

func Hello() string {
	fmt.Printf("Hello, Garrett")
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
```


## write-to-file.txt

### Example

```go
I can write to a file
```


