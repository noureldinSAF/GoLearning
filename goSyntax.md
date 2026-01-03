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
13- Send a reqeust using the Full request fromat as a String : 
```go
package main

import (
	"bufio"
	"fmt"
	"io"
	"net/http"
	"strings"
)

func main() {
	// 1. PASTE RAW REQUEST
	rawReq := `POST /fetch HTTP/1.1
Host: tm91cmvszglu.playat.flagyard.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:146.0) Gecko/20100101 Firefox/146.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 24
Origin: http://tm91cmvszglu.playat.flagyard.com
Connection: keep-alive
Cookie: _ga_YD2ZMTYJ5D=GS2.1.s1767278403$o3$g1$t1767278515$j24$l0$h0; _ga=GA1.1.1432435242.1766847934
Priority: u=0

url=fi{l}e:/app/flag.txt`

	// 2. FIX NEWLINES & PARSE
	rawReq = strings.ReplaceAll(rawReq, "\n", "\r\n")
	reader := bufio.NewReader(strings.NewReader(rawReq))
	req, err := http.ReadRequest(reader)
	if err != nil {
		panic(err)
	}

	// 3. CONFIGURE CLIENT SETTINGS
	req.RequestURI = ""
	req.URL.Scheme = "http"
	req.URL.Host = req.Host

	// 4. SEND REQUEST
	client := &http.Client{}
	resp, err := client.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	// 5. PRINT STATUS
	fmt.Printf("HTTP/1.1/2 %s\n", resp.Status)

	// 6. PRINT HEADERS (Updated Part)
	for key, values := range resp.Header {
		// We join values with comma in case there are multiple (e.g., multiple Set-Cookie)
		fmt.Printf("%s: %s\n", key, strings.Join(values, ", "))
	}

	// 7. PRINT BODY
	fmt.Println()
	body, _ := io.ReadAll(resp.Body)
	fmt.Println(string(body))
}
```

14- demo without concurrency 
```go
package main

import (
	"fmt"
	"time"
)

func scan(url string) error {
	fmt.Println("Scanning URL:", url)
	time.Sleep(10 * time.Second) 
	return nil
}

func main() {

	urls := []string{
		"http://example.com",
		"http://test.com",
		"http://sample.com",
		"http://demo.com",
		"http://website.com",
		"http://mysite.com",
		"http://anothersite.com",
		"http://onemoresite.com",
		"http://lastsite.com",
		"http://finalsite.com",
	}
    
	startTime := time.Now()
	for _, url := range urls {
		err := scan(url)
		if err != nil {
			fmt.Println("Error scanning URL:", err)
		}
	}

	elapsed := time.Since(startTime)
	fmt.Println("Total scanning time:", elapsed)
}

PS F:\Red teaming\automation course> go run "f:\Red teaming\automation course\main.go"
Scanning URL: http://example.com
Scanning URL: http://test.com
Scanning URL: http://sample.com
Scanning URL: http://demo.com
Scanning URL: http://website.com
Scanning URL: http://mysite.com
Scanning URL: http://anothersite.com
Scanning URL: http://onemoresite.com
Scanning URL: http://lastsite.com
Scanning URL: http://finalsite.com
Total scanning time: 1m40.0102196s
```
15- demo with concurrency; using The WaitGroup method, this method run All the urls at once, useful for small urls, but it may be crash the memory if you used it in large urls
```go
package main
// Tells the compiler this is an executable program, not a library.

import (
    "fmt"   // For printing text to the console
    "sync"  // For the WaitGroup (the counter)
    "time"  // For sleeping and tracking time
)

// This is the "Job" function. It takes 10 seconds to finish.
func scan(url string) error {
    fmt.Println("Scanning URL:", url) // Print that we started
    time.Sleep(10 * time.Second)      // Pause this specific thread for 10s
    return nil                        // Return "no error"
}

func main() {
    // 1. Create a slice (list) of websites to scan
    urls := []string{
		"http://example.com",
		"http://test.com",
		"http://sample.com",
		"http://demo.com",
		"http://website.com",
		"http://mysite.com",
		"http://anothersite.com",
		"http://onemoresite.com",
		"http://lastsite.com",
		"http://finalsite.com",
    }
    
    // 2. Record the current time so we can calculate duration later
    startTime := time.Now()

    // 3. Create the "Manager" (WaitGroup). 
    // This is a counter that prevents the program from quitting too early.
    var wg sync.WaitGroup

    // 4. Set the counter to the number of jobs (12 URLs)
    // The manager now knows it must wait for 12 "Done" signals.
    wg.Add(len(urls))

    // 5. Loop through every URL in the list
    // _ is the index (0, 1, 2...) which we ignore.
    // url is the value ("http://example.com"...)
    for _, url := range urls {
        
        // 6. Launch a background worker (Goroutine)
        // 'go' means: "Start this function NOW, but don't wait for it to finish.
        // Move immediately to the next line of code."
        go func(u string) {
            
            // 7. Schedule the "Done" signal
            // 'defer' means: "Run this line at the very end of this function,
            // just before it exits." It subtracts 1 from the wg counter.
            defer wg.Done()
            
            // 8. Do the actual work
            // This calls the scan function using the local copy 'u'
            scan(u)

        }(url) // <--- 9. PASS THE DATA IN
        // This takes the current 'url' from the loop and passes it 
        // into the 'u' variable of the function above.
    }

    // 10. The Block
    // The main program stops here. It pauses until the wg counter hits 0.
    // Without this, the program would exit instantly before scans finish.
    wg.Wait()

    // 11. Calculate how long it took
    elapsed := time.Since(startTime)
    
    // 12. Print the result
    fmt.Println("Total scanning time:", elapsed)
}

PS F:\Red teaming\automation course> go run "f:\Red teaming\automation course\main.go"
Scanning URL: http://test.com
Scanning URL: http://finalsite.com
Scanning URL: http://demo.com
Scanning URL: http://website.com
Scanning URL: http://mysite.com
Scanning URL: http://anothersite.com
Scanning URL: http://onemoresite.com
Scanning URL: http://lastsite.com
Scanning URL: http://sample.com
Scanning URL: http://example.com
Total scanning time: 10.0023271s
```

16- The Worker Pool (The "Sempahore") concurrency method, if you have 1000 request and you need to run 5 at once then 5, then 5 ...and so on
```go
package main

import (
    "fmt"
    "sync"
    "time"
)

// The job function (simulates a slow scan)
func scan(url string) error {
    fmt.Println("Scanning URL:", url)
    time.Sleep(10 * time.Second) 
    return nil
}

func main() {
    
    // 1. Set the Limit
    // We only want 5 simultaneous scans.
    cLevel := 5

    // 2. Build the "Parking Lot" (Buffered Channel)
    // We make a channel that fits exactly 5 items.
    // 'struct{}' is used because it takes 0 bytes of memory. We don't care about data, just counting.
    cLevelCh := make(chan struct{}, cLevel)

    urls := []string{
		"http://example.com",
		"http://test.com",
		"http://sample.com",
		"http://demo.com",
		"http://website.com",
		"http://mysite.com",
		"http://anothersite.com",
		"http://onemoresite.com",
		"http://lastsite.com",
		"http://finalsite.com",
	}
    
    startTime := time.Now()
    var wg sync.WaitGroup

    // 3. Loop through all 10 URLs
    for _, url := range urls {
        
        // 4. Add to WaitGroup
        // We tell the manager "expect 1 more worker to finish eventually".
        wg.Add(1)

        // 5. Launch the Worker (Goroutine)
        // CRITICAL: All 10 goroutines start ALMOST INSTANTLY here.
        // They are all "alive" in the background now.
        go func(u string) {
            
            // 9. When function finishes (at the very end), mark job as done.
            defer wg.Done()

            // 6. THE GATEKEEPER (Enter Parking Lot)
            // The code tries to push an empty item into the channel.
            // - If the channel has space (0-4 items): It succeeds, and code continues.
            // - If the channel is full (5 items): The code PAUSES (BLOCKS) right here.
            //   It waits until a slot opens up.
            cLevelCh <- struct{}{}

            // 7. THE EXIT (Leave Parking Lot)
            // We schedule this to run when the function exits.
            // It pulls an item OUT of the channel, freeing up a slot for a waiting worker.
            defer func() { <-cLevelCh }()
            
            // 8. The Actual Work
            // This only runs if step 6 succeeded.
            scan(u)

        }(url)
    }

    // 10. Wait for everyone
    wg.Wait()

    elapsed := time.Since(startTime)
    fmt.Println("Total scanning time:", elapsed)
}

PS F:\Red teaming\automation course> go run "f:\Red teaming\automation course\main.go"
Scanning URL: http://demo.com
Scanning URL: http://sample.com
Scanning URL: http://example.com
Scanning URL: http://website.com
Scanning URL: http://test.com
Scanning URL: http://anothersite.com
Scanning URL: http://mysite.com
Scanning URL: http://onemoresite.com
Scanning URL: http://lastsite.com
Scanning URL: http://finalsite.com
Total scanning time: 20.0038062s
```
