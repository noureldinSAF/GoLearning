1- You can return more than one value in functions (  Multiple Return Values ) 
ex: 
```go
package main
import "fmt"

func vals() (int, int) {
    return 3, 7
}

func main() {

    a, b := vals()
    fmt.Println(a)
    fmt.Println(b)

    _, c := vals()
    fmt.Println(c)
}

$ go run multiple-return-values.go
3
7
7
```
2- You can use unknown number of arguments in a function ( Variadic Functions ), go deals with arguments in this case as a slice
```go
package main
import "fmt"

func sum(nums ...int) {
    fmt.Print(nums, " ")
    total := 0

    for _, num := range nums {
        total += num
    }
    fmt.Println(total)
}

func main() {

    sum(1, 2)
    sum(1, 2, 3)

    nums := []int{1, 2, 3, 4}
    sum(nums...)
}
```
3- when you use a for loop for slice, you need for index, value. if you want to ignore one of them replace it by _ 

4- Anonymous function : quick functions, it doesn't have a name, you assign it to a variable. use it when you are in another function
ex: 
func main() {
    fib := func(n int) int { ... } // OK: Assigning a value to a variable
}

5- In Go, if you want an anonymous function to call itself, you must declare the variable before you define the function
ex: 
```go
var fib func(n int) int

fib = func(n int) int {
    if n < 2 {
        return n
    }
    return fib(n-1) + fib(n-2)
}
```
## Important !!!
6- Looping over an array of 100 mg, creates another 100 mg. So it is more garbage in the memory.
instead, use a pointer to pass the variable by reference which is just 4 - 8 bytes.
if you have a struct called paerson and you will create an array of it. Don't use []person use []*person

7- comma ok adioms. 
Logic => If g object is rect type, assign it to n, and OK becomes true. else Ok becomes false
```go
value, ok := myMap[key]
if ok {
    // Key exists, use value
} else {
    // Key does not exist
}

```
8- If with a short statement 
```go
//  1. Setup       2. Separator    3. The Check
if  x := 10    ;   x > 5           {
    // Code runs if x > 5
}
```

9- nil  
nil in Go is the concept of "Zero" or "Nothing" for complex types.

If you are coming from Java, it is very similar to null, but with stricter rules.

In simple terms: nil means "This variable has been defined, but it doesn't point to anything real yet."

1. The "Zero Value" Rule
In Go, every variable is created with a default value. It is never "undefined."

If you create an int, the default is 0.

If you create a bool, the default is false.

If you create a string, the default is "" (Empty string).

If you create a Pointer, Map, Slice, Channel, or Interface, the default is nil.

10- You can use cobra library instead of flag library to make many modes of your tool ( sub commands ) 

11- Run these 3 lines in vs code to update the github repostry with the modification you made in the local code 
```go
git add .
git commit -m "Description of what I changed"
git push
```
12- Differences Between %v, %+v, %#v 
```go
func main() {

	type person struct {
		name string
		age  int
	}
	p := person{name: "Alice", age: 30}

	fmt.Printf("Person: %v\n", p) //Person: {Alice 30}
	fmt.Printf("Person: %+v\n", p) // Person: {name:Alice age:30}
	fmt.Printf("Person: %#v\n", p) // Person: main.person{name:"Alice", age:30}
	scanner.Scan()
}
```

13- fmt.Printf() 
```go
func main() {

	fmt.Printf("|%10s|\n", "test")    //|        test|
	fmt.Printf("|%-10s|\n", "test")   //|test        |
	fmt.Printf("|%*s|\n", 10, "test") //|        test|

	fmt.Printf("Number: %d\n", 42)        //Number: 42

	fmt.Printf("Pi: %.2f\n", 3.14159) //Pi: 3.14

	fmt.Printf("Hex: %#x\n", 255) //Hex: 0xff

	fmt.Printf("Percent: %.2f%%\n", 99.5) //Percent: 99.50%

	fmt.Printf("Scientific: %.2e\n", 12345.6789) //Scientific: 1.23e+04

	fmt.Printf("Binary: %b\n", 10) //Binary: 1010

	fmt.Printf("Unicode: %U\n", 'A') //Unicode: U+0041

	test := "example"
	fmt.Printf("Pointer: %p\n", &test) //Pointer: 0xc0000120b0


}
```

14- intialize a project connecting to github 
```bash
go mod init github.com/yourname/AutoRegister
go mod tidy
go run ./cmd/main.go
```

15- initialize and update the project to github 
```bash
# 1. Initialize the folder
git init -b main

# 2. Stage all files
git add .

# 3. Commit your files locally
git commit -m "Initial project setup"

# 4. Link to GitHub (Replace with your actual URL)
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git

# 5. Push to GitHub
git push -u origin main
```

16- pull 
```bash
# Downloads and merges the latest changes from GitHub
git pull origin main
```
17- If you want to Add a package online 
```bash
go get github.com/gen2brain/beeep
```

18- git remote and git pull while current remote 
```bash
git remote set-url origin https://github.com/noureldinSAF/AutoCourse.git
git pull origin main --allow-unrelated-histories
```

19- How to Install your code in another device ( vps ) 
```bash
root@BountyBox:/home/AutoHunting/AutoRegister# git init -b main
Initialized empty Git repository in /home/AutoHunting/AutoRegister/.git/
root@BountyBox:/home/AutoHunting/AutoRegister# git add .
root@BountyBox:/home/AutoHunting/AutoRegister# git remote add origin https://github.com/noureldinSAF/AutoRegister.git
root@BountyBox:/home/AutoHunting/AutoRegister# git pull origin main
Username for 'https://github.com': noureldinSAF
Password for 'https://noureldinSAF@github.com': <Paste-your-personal-access-token>
```



