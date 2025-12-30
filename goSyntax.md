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





