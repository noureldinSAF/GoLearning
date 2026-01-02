Resources: https://gobyexample.com/

These are the new concepts: 

1. Switch
```go
    i := 2
    fmt.Print("Write ", i, " as ")
    switch i {
    case 1:
        fmt.Println("one")
    case 2:
        fmt.Println("two")
    case 3:
        fmt.Println("three")

output: 
Write 2 as two
```

2- closures 
A function that returns another function, and saving the value in memory after it finishes. if you call it again, it will depend on the last returned value.
ex:
```go
package main

import "fmt"

func intSeq() func() int {
    i := 1
    return func() int {
        i = i * 2 
        return i
    }
}

func main() {

    nextInt := intSeq()

    
    fmt.Println(nextInt())
    fmt.Println(nextInt())
	fmt.Println(nextInt())
	fmt.Println(nextInt())

    newInts := intSeq()
    fmt.Println(newInts())
}

PS F:\Red teaming\automation course> go run "f:\Red teaming\automation course\main.go"
2
4
8
16
2
```
3- pointers => pass the variable by reference not by value, so you can edit its original value 
To get the reference of variable i use &i, to get the value of this reference in function use *i 
ex: 
```go
package main

import "fmt"

func zeroval(ival int) {
    ival = 0
}

func zeroptr(iptr *int) {
    *iptr = 0
}

func main() {
    i := 1
    fmt.Println("initial:", i)

    zeroval(i)
    fmt.Println("zeroval:", i)

    zeroptr(&i)
    fmt.Println("zeroptr:", i)

    fmt.Println("pointer:", &i)
}

$ go run pointers.go
initial: 1
zeroval: 1
zeroptr: 0
pointer: 0x42131100
```
4- Struct 
Uses a variable that has a collection of different variable types. 
```go
package main

import "fmt"

type person struct {
    name string
    age  int
}

func main() {

    p := person{

		name: "Alice",
		age: 30,
	}

	fmt.Println("Name:", p.name)
	fmt.Println("Age:", p.age)
}

PS F:\Red teaming\automation course> go run "f:\Red teaming\automation course\main.go"
Name: Alice
Age: 30
```
5- Constructor - A method to assign values to struct aoutamatically 
```go
package main

import "fmt"

type person struct {
    name string
    age  int
}

func constructPerson(name string, age int) *person {  // Here is the Constructor
	return &person{
		name: name,
		age:  age,
	}
}

func main() {

    p := constructPerson("Alice", 30)

	fmt.Println("Name:", p.name)
	fmt.Println("Age:", p.age)
}
```
6- Here is the updated past code with getter method to get the name, 
struct function (reciever ) methodName() reutrnType {}
```go
package main

import "fmt"

type person struct {
    name string
    age  int
}

func  constructPerson(name string, age int) *person {
	return &person{
		name: name,
		age:  age,
	}
}

func (p *person) GetPersonName() string {  //Here is the getter
	return p.name
}

func main() {

    p := constructPerson("Alice", 30)

	fmt.Println("Name:", p.GetPersonName())
	fmt.Println("Age:", p.age)
}
```
7- Interface with polymorephism. Interface put rules ( methods' signatures )  for every type (struct ) to have all the method declared in Interface. 
Polymorephism meaning many forms. So you can use many types using one inteface object. 
```go
package main

import (
    "fmt"
    "math"
)

type geometry interface {
    area() float64
    perim() float64
}

type rect struct {
    width, height float64
}
type circle struct {
    radius float64
}

func (r rect) area() float64 {
    return r.width * r.height
}
func (r rect) perim() float64 {
    return 2*r.width + 2*r.height
}

func (c circle) area() float64 {
    return math.Pi * c.radius * c.radius
}
func (c circle) perim() float64 {
    return 2 * math.Pi * c.radius
}

func measure(g geometry) {
    fmt.Println(g)
    fmt.Println(g.area())
    fmt.Println(g.perim())
}

func detectCircle(g geometry) {
    if n, ok := g.(rect); ok {
        fmt.Println("rect with width", n.width, "height", n.height)
    }
}

func main() {
    r := rect{width: 3, height: 4}
    c := circle{radius: 5}

    measure(r)
    measure(c)

    detectCircle(r)
    detectCircle(c)
}

PS F:\Red teaming\automation course> go run "f:\Red teaming\automation course\main.go"
{3 4}
12
14
{5}
78.53981633974483
31.41592653589793
rect with width 3 height 4
```
8- Channels

```go
package main

import "fmt"

func main() {

    req := make(chan string) 

	go func() {
		req <- "Hello, World!"
		}	()

	msg := <-req		
	fmt.Println(msg)
	}

PS F:\Red teaming\automation course> go run "f:\Red teaming\automation course\main.go"
Hello, World!
```

9- Channel buffering 
```go
package main

import "fmt"

func main() {

    messages := make(chan string, 2)

    messages <- "buffered"
    messages <- "channel"

    fmt.Println(<-messages)
    fmt.Println(<-messages)
}

PS F:\Red teaming\automation course> go run "f:\Red teaming\automation course\main.go"
buffered
channel

```
10- Read Files 
```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

func readFile(filename string) ([]string, error) {
	lines := []string{}

	file, err := os.Open(filename)
	if err != nil {
		fmt.Println("Error opening file:", err)
		return []string{}, err
	}
	defer file.Close()
	// Scanner to read the file line by line
	scanner := bufio.NewScanner(file)
	for scanner.Scan() {
		lines = append(lines, scanner.Text())
	}

	if scanner.Err() != nil {
		fmt.Println("Error reading file")
		return []string{}, scanner.Err()
	}

	return lines, scanner.Err()
}

func main() {
	lines, err := readFile("subdomainsx.txt")
	if err != nil {
		fmt.Println("Failed to read file:", err)
		return
	}
	for _, line := range lines {
		fmt.Println(line)
	}
	fmt.Println(lines)
}


PS F:\Red teaming\automation course> go run "f:\Red teaming\automation course\main.go"
bounty.conceptboard.com
www.vimeo.com
www.google.com
www.zerocopter.com
[bounty.conceptboard.com www.vimeo.com www.google.com www.zerocopter.com]
```

11- Create files 
```go
package main

import (
	"fmt"
	"os"
)

func writeToFile(filename string, data []string) error {
	file, err := os.Create(filename)
	if err != nil {
		fmt.Println("Error creating file:", err)
		return err
	}
	defer file.Close()

	for _, sub := range data {
		file.WriteString("https://" + sub + "\n")
	}
	fmt.Println("Subdomains written to subdomains2.txt")
	return nil
}

func main() {
	subs := []string{"sub1.example.com", "sub2.example.com", "sub3.example.com", "sub4.example.com", "sub5.example.com"}
    file := "subdomains3.txt"
	writeToFile(file, subs)
}

```

12- HTTP Get request, get the response, get the response with json format 
```go
package main

import (
	"encoding/json"
	"fmt"
	"io"
	"net/http"
)

type Post struct {
	UserID int    `json:"userId"`
	ID     int    `json:"id"`
	Title  string `json:"title"`
	Body   string `json:"body"`
}

func main() {

	resp, err := http.Get("https://jsonplaceholder.typicode.com/posts/1")
	if err != nil {
		fmt.Println("Error fetching URL:", err)
		return
	}
	defer resp.Body.Close()

	body, err :=io.ReadAll(resp.Body)
	if err != nil {
		fmt.Println("Error reading response body:", err)
		return
	}
    
	//print the hole body
	fmt.Println(string(body))

	//Unmarshal the JSON response into a Post struct
	var schema *Post
	if err := json.Unmarshal(body, &schema); err != nil {
		fmt.Println("Error unmarshaling JSON:", err)
		return
	}

	fmt.Printf("The title is: %s\nThe id is %d", schema.Title, schema.ID)

}
```

13- HTTP with any request type 
```go
package main

import (
	//"encoding/json" use this package if you want to unmarshal the response body to json
	"fmt"
	"io"
	"net/http"
	"strings"
)

// Post represents the structure of the post data, used for unmarshaling JSON response
type Post struct {
	UserID int    `json:"userId"`
	ID     int    `json:"id"`
	Title  string `json:"title"`
	Body   string `json:"body"`
}

func main() {

	body := `{
	  "userId": 1,
	  "username": "Noureldin",
	  "title": "foo",
	  "body": "bar",
	  "id": 112
	}
	`
	// convert body to io.Reader type, so you can pass it to http.NewRequest
	reqBody := strings.NewReader(body)
    
	// create a new POST request
	req, err := http.NewRequest("POST", "https://jsonplaceholder.typicode.com/posts", reqBody)
	if err != nil {
		fmt.Println("Error creating request:", err)
		return
	}

	// set the appropriate headers
	req.Header.Set("Content-Type", "application/json; charset=UTF-8")

	// send the request using http.DefaultClient
	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		fmt.Println("Error making request:", err)
		return
	}
	// ensure the response body is closed after reading
	defer resp.Body.Close()
    
	// read the response body
	respBody, err := io.ReadAll(resp.Body)
	if err != nil {package main

import (
	//"encoding/json" use this package if you want to unmarshal the response body to json
	"fmt"
	"io"
	"net/http"
	"strings"
)

// Post represents the structure of the post data, used for unmarshaling JSON response
type Post struct {
	UserID int    `json:"userId"`
	ID     int    `json:"id"`
	Title  string `json:"title"`
	Body   string `json:"body"`
}

func main() {

	body := `{
	  "userId": 1,
	  "username": "Noureldin",
	  "title": "foo",
	  "body": "bar",
	  "id": 112
	}
	`
	// convert body to io.Reader type, so you can pass it to http.NewRequest
	reqBody := strings.NewReader(body)
    
	// create a new POST request
	req, err := http.NewRequest("POST", "https://jsonplaceholder.typicode.com/posts", reqBody)
	if err != nil {
		fmt.Println("Error creating request:", err)
		return
	}

	// set the appropriate headers
	req.Header.Set("Content-Type", "application/json; charset=UTF-8")

	// send the request using http.DefaultClient
	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		fmt.Println("Error making request:", err)
		return
	}
	// ensure the response body is closed after reading
	defer resp.Body.Close()
    
	// read the response body
	respBody, err := io.ReadAll(resp.Body)
	if err != nil {
		fmt.Println("Error reading response body:", err)
		return
	}
	// print the response status, body, and headers
	fmt.Println("Response Status:", resp.Status)
	fmt.Println("Response Body:", string(respBody))
	fmt.Println("Response Headers:", resp.Header)

	// Example of accessing specific headers
	fmt.Println("The Cache-Control is:", resp.Header.Get("Cache-Control"))
	fmt.Println("The reporting Endpoint is:", resp.Header.Get("Reporting-Endpoints"))
	
}
		fmt.Println("Error reading response body:", err)
		return
	}
	// print the response status, body, and headers
	fmt.Println("Response Status:", resp.Status)
	fmt.Println("Response Body:", string(respBody))
	fmt.Println("Response Headers:", resp.Header)

	// Example of accessing specific headers
	fmt.Println("The Cache-Control is:", resp.Header.Get("Cache-Control"))
	fmt.Println("The reporting Endpoint is:", resp.Header.Get("Reporting-Endpoints"))
	
}
```
