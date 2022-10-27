# GO Guide for Beginners
![logo](https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/Go_Logo_Blue.svg/1200px-Go_Logo_Blue.svg.png)

This is a guide for beginners to learn Go. It is a work in progress. If you have any suggestions, please open an issue or a pull request.

## Table of Contents
- [GO Guide for Beginners](#go-guide-for-beginners)
  - [Table of Contents](#table-of-contents)
  - [Initialize-the-Project](#initialize-the-project)
  - [Build-and-Run](#build-and-run)
  - [Simple-Code](#simple-code)
  - [Variables](#variables)
  - [Input](#input)
  - [Arrays-and-Slices](#arrays-and-slices)
  - [Maps](#maps)
  - [Structs](#structs)
  - [Loops](#loops)
  - [Conditionals](#conditionals)
  - [Functions](#functions)
  - [Packages](#packages)
    - [Example 1(Different files in the same package)](#example-1different-files-in-the-same-package)
    - [Example 2(Different files in different packages)](#example-2different-files-in-different-packages)
  - [Go Routines](#go-routines)
  - [Channels](#channels)
  - [Mutex](#mutex)

## Initialize-the-Project
```bash
go mod init <projectname>
# OR, a repository name
go mod init github.com/<username>/<projectname>
```
This command, creates a go.mod file, which is the go module file. This file contains the dependencies of the project.

## Build-and-Run
We build the project and run the executable file.
```bash
go build main.go
./main
```
We can also compile and run the main.go file in one command.
```bash
go run main.go
```


## Simple-Code
```go
package main
Print("Hello World")
```
In GO, everything is a package. The main package is the entry point of the program. The main package is the only package that can be executed.
Actually, we should define a function called main, which is the entry point of the program.
```go
package main
func main() {
    Print("Hello World")
}
```
The Print function is defined in the fmt package which stands for the Format package. So, we need to import the fmt package.
```go
package main
import "fmt"
func main() {
    fmt.Print("Hello World")
}
```
Note that, we can import multiple packages in one line and we should whatever package we have imported.
```go
package main
import "fmt"
func main() {
    fmt.Println("Hello World")
}
```

## Variables

To define a variable, we use the `var` keyword. You don't need to specify the type of the variable. The compiler will infer the type of the variable.
Note that, you will get an error if you defined a var without using it. 
```go
package main
import "fmt"
func main() {
	// part 1
	var name string = "Mostafa"
	fmt.Println(name)
	// you can also use it like
	fmt.Println("Hello", name, "!")

	// part 2
	// const constAge int = 20           // this value can't be changed
	var age int = 20                  // this value can be changed
	fmt.Printf("My age is %v\n", age) // use Printf and a format specifier to format your output

	// part 3
	// if you didn't specify a default value, you should specify the type
	var isCool bool
	isCool = true

	// part 4
	// you can print the type of a variable like this:
	fmt.Printf("isCool is of type: %T\n", isCool)

	// part 5
	// you can use short hand declaration to declare a variable
	size := 2.3 // use this, not that it doesn't work with types
	// var size float32 = 2.3 // instead of this
}
```

## Input

```go
package main
import "fmt"
func main() {
    var name string // you should define the variable and its type first
    fmt.Print("Enter your name: ")
    fmt.Scan(&name)
    fmt.Println("Hello", name, "!")
}
```

## Arrays-and-Slices 

```go
package main
import "fmt"
func main() {
    // part 1
    // arrays
    var fruits [2]string // you should define the size of the array
    fruits[0] = "Apple"
    fruits[1] = "Orange"
    fmt.Println(fruits) // prints [Apple Orange], the whole array
    fmt.Println(len(fruits)) // prints 2, the length of the array

    // part 2
    // slices, just arrays without a size
    var vegetables = []string{"Tomato", "Potato"} // we can add a default value if we want, just like arrays
    fmt.Println(vegetables) // prints [Tomato Potato], the whole array
    fmt.Println(vegetables[1]) // prints Potato, the second element of the array

    // part 3
    // slices are dynamic
    vegetables = append(vegetables, "Cucumber")
    fmt.Println(vegetables) // prints [Tomato Potato Cucumber], the whole array

    // part 4
    // slices are reference types
    // so, if you change a slice, you will change the original slice
    // and if you change the original slice, you will change the other slices
    // that are referencing to the original slice
    var otherVegetables = vegetables // this is a reference to the original slice, otherVegetables is a pointer to vegetables
    otherVegetables[0] = "Onion" // equivalent to vegetables[0] = "Onion"
    fmt.Println(vegetables) // prints [Onion Potato Cucumber], the whole array
    fmt.Println(otherVegetables) // prints [Onion Potato Cucumber], the whole array
}
``` 

## Maps

```go
package main
import "fmt"
func main() {
    // part 1
    // maps
    var person = map[string]string{"name": "Mostafa", "age": "22"} // you should define the key and value types
    fmt.Println(person) // prints map[age:22 name:Mostafa], the whole map
    fmt.Println(person["name"]) // prints Mostafa, the value of the key "name"

    // part 2
    // maps are dynamic
    person["height"] = "172" // add a new key and value
    fmt.Println(person) // prints map[age:22 height:172 name:Mostafa], the whole map

    // part 3
    // maps are reference types
    // so, if you change a map, you will change the original map
    // and if you change the original map, you will change the other maps
    // that are referencing to the original map
    var otherPerson = person // this is a reference to the original map, otherPerson is a pointer to person
    otherPerson["name"] = "Wael" // equivalent to person["name"] = "Wael"
    fmt.Println(person) // prints map[age:22 height:172 name:Wael], the whole map
    fmt.Println(otherPerson) // prints map[age:22 height:172 name:Wael], the whole map

    // part 4
    // Create an empty map
    var emptyMap = make(map[string]string) // we use the make function to create an empty map
    fmt.Println(emptyMap) // prints map[]
    var emptyMap2 = map[string]string{} // we can also use this syntax
    fmt.Println(emptyMap2) // prints map[]
    var emptyMapDefaultSize = make(map[string]string, 10) // we can also specify the default size of the map
    fmt.Println(emptyMapDefaultSize) // prints map[]
    fmt.Println(len(emptyMapDefaultSize)) // prints 0, the length of the map

    // part 5
    // Delete a key from a map
    delete(person, "age") // delete the key "age" from the map person
    fmt.Println(person) // prints map[height:172 name:Wael], the whole map
}
```

## Structs
```go
package main
import "fmt"
func main() {
    // part 1
    // structs
    type Person struct {
        name string
        age int
    }
    var person = Person{name: "Mostafa", age: 22} // you should define the fields and their types
    fmt.Println(person) // prints {Mostafa 22}, the whole struct
    fmt.Println(person.name) // prints Mostafa, the value of the field "name"

    // part 2
    // structs are dynamic
    person.age = 23 // change the value of the field "age"
    fmt.Println(person) // prints {Mostafa 23}, the whole struct

    // part 3
    // structs are value types
    // so, if you change a struct, you will change the original struct
    // and if you change the original struct, you won't change the other structs
    // that are referencing to the original struct
    var otherPerson = person // this is a copy of the original struct, otherPerson is a pointer to person
    otherPerson.name = "Wael" // equivalent to person.name = "Wael"
    fmt.Println(person) // prints {Mostafa 23}, the whole struct
    fmt.Println(otherPerson) // prints {Wael 23}, the whole struct

    // part 4
    // Create an empty struct
    var emptyStruct = Person{} // we can use this syntax
    fmt.Println(emptyStruct) // prints { 0}, the whole struct
    var emptyStruct2 = Person{name: "Mostafa"} // we can also use this syntax
    fmt.Println(emptyStruct2) // prints {Mostafa 0}, the whole struct
}
```


## Loops
    
```go
package main
import "fmt"
func main() {
    // part 1
    // for loop
    for i := 0; i < 5; i++ {
        fmt.Println(i)
    }

    // part 2
    // while loop
    var i int = 0
    for i < 5 {
        fmt.Println(i)
        i++
    }

    // part 3
    // infinite loop
    for {
        fmt.Println("Hello World")
    }

    // part 4
    // range
    var fruits = []string{"Apple", "Orange", "Banana"}
    for index, fruit := range fruits {
        fmt.Println(index, fruit)
    }
}
```

## Conditionals

```go
package main
import "fmt"
func main() {
    // part 1
    // if statement
    var age int = 20
    if age > 18 {
        fmt.Println("You are an adult")
    } else if age > 12 {
        fmt.Println("You are a teenager")
    } else {
        fmt.Println("You are a child")
    }

    // part 2
    // switch statement
    switch age {
    case 18:
        fmt.Println("You are 18")
    case 20:
        fmt.Println("You are 20")
    default:
        fmt.Println("You are not 18 or 20")
    }
}
```

## Functions

```go
package main
import "fmt"
func main() {
    // part 1
    // function with no parameters and no return value
    sayHello()

    // part 2
    // function with parameters and no return value
    sayHelloTo("Mostafa")

    // part 3
    // function with parameters and return value
    var result int = sum(2, 3)
    fmt.Println(result)

    // part 4
    // function with multiple return values
    var sumResult, subResult int = sumAndSub(2, 3)
    fmt.Println(sumResult, subResult)
}
func sayHello() {
    fmt.Println("Hello World")
}
func sayHelloTo(name string) {
    fmt.Println("Hello", name)
}
func sum(a int, b int) int {
    return a + b
}
func sumAndSub(a int, b int) (int, int) {
    return a + b, a - b
}
```

## Packages
In Go, everything is a package. You can import a package and use its functions. You can also create your own package and use it in your code. 
### Example 1(Different files in the same package)
```go
// helper.go
package main
import "fmt"
func SayHello() {
    fmt.Println("Hello World")
}
```
```go
// main.go
package main
import "fmt"
func main() {
    sayHelloTo("Mostafa")
}
```
Since the `helper.go` and `main.go` files are in the same package, we can use the `SayHello` function in the `main.go` file directly.
You can run the code using `go run main.go helper.go` or `go run .` if you are in the same directory as the files.
### Example 2(Different files in different packages)
When using different packages, you should separate the files in different directories.
```go
// helper/helper.go
package helper
import "fmt"
// This function now, acts as a global-public- function
func SayHello() {
    fmt.Println("Hello World")
}
```
```go
// main.go
package main
import(
    "fmt"
    "guide/helper" // we got this from the module name in go.mod
)
// or 
// import "fmt"
// import "guide/helper"
func main() {
    helper.SayHello() // we should use the package name as a prefix
}
```
Note, that we had to capitalize the first letter of the `SayHello` function in the `helper.go` file. This is because the function is exported and can be used in other packages. If we didn't capitalize the first letter, the function would be private and we couldn't use it in other packages. The same applies for variables and structs.
Now, you can relate why all the functions in the `fmt` package started with a capital letter. 

You can also share your package with others. You can find more information about packages [here](https://golang.org/doc/code.html).    

## Go Routines
Go Routines are lightweight threads. They are used to run multiple functions at the same time. You can use them to run multiple functions at the same time. You can also use them to run a function in the background. 
```go
package main
import (
	"fmt"
	"time"
)
func main() {
    // part 1
    // run a function in the background
    go sayHello()
    time.Sleep(1 * time.Second)

    // part 2
    // run multiple functions at the same time
    go sayHello()
    go sayHelloTo("Mostafa")
    time.Sleep(1 * time.Second)

    // part 3
    // run a function in the background and wait for it
    go func() {
        fmt.Println("Hello World")
    }()
    time.Sleep(1 * time.Second)

    // part 4
    // run multiple functions at the same time and wait for them
    go func() {
        fmt.Println("Hello World")
    }()
    go func(name string) {
        fmt.Println("Hello", name)
    }("Mostafa")
    time.Sleep(1 * time.Second)
}
func sayHello() {
    fmt.Println("Hello World")
}
func sayHelloTo(name string) {
    fmt.Println("Hello", name)
}

```

## Channels
Channels are used to communicate between Go Routines. You can send and receive data from a channel. 
```go
package main
import (
    "fmt"
    "time"
)
func main() {
    // part 1
    // create a channel
    var channel chan string = make(chan string)

    // part 2
    // send data to a channel
    go func() {
        channel <- "Hello World"
    }()

    // part 3
    // receive data from a channel
    var message string = <-channel
    fmt.Println(message)

    // part 4
    // create a buffered channel
    var bufferedChannel chan string = make(chan string, 2)
    bufferedChannel <- "Hello"
    bufferedChannel <- "World"
    fmt.Println(<-bufferedChannel)
    fmt.Println(<-bufferedChannel)

    // part 5
    // create a channel with a timeout
    var timeoutChannel chan string = make(chan string, 1)
    go func() {
        time.Sleep(2 * time.Second)
        timeoutChannel <- "Hello World"
    }()
    select {
    case message := <-timeoutChannel:
        fmt.Println(message)
    case <-time.After(1 * time.Second):
        fmt.Println("Timeout")
    }
}
```

## Mutex
Mutex is used to lock a variable or a function. Only one Go Routine can lock a variable or a function at a time. If another Go Routine tries to lock the same variable or function, it will be blocked until the first Go Routine unlocks the variable or function. 
```go
package main
import (
    "fmt"
    "sync"
    "time"
)
func main() {
    var counter int = 0
    var wg sync.WaitGroup // used to wait for all the Go Routines to finish
    var mutex sync.Mutex // used to lock the counter variable
    for i := 0; i < 10; i++ {
        wg.Add(1) // acts as a counter
        go func() {
            mutex.Lock() // lock the counter variable
            counter++
            fmt.Println(counter)
            mutex.Unlock() // unlock the counter variable
            wg.Done() // to decrement the counter
        }()
    }
    wg.Wait() // wait for all the Go Routines to finish before exiting the program
    fmt.Println(counter)
}
```
>> If you have any suggestions, please open an issue or a pull request.
