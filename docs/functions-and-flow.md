# Functions and Control Flow in Go

---

## 🧠 Go Is Block Scoped

> Go is **block scoped**, not function scoped.  
Blocks are created by `{}` — including `main`, loops, if-statements, etc.

---

## 🔹 Logging Function

```go
func Logger(fname, lname, registerDate, planType string, isOnUpgradeList bool, numOfDevices, numOfRelatives int) string {
	logMessage := fmt.Sprintf(
		"\n Name: %v %v\n Date Registered: %v\n Subscription Type: %v\n OnUpgradeList: %v\n Number of Devices: %v\n Number of Relatives: %v\n",
		fname, lname, registerDate, planType, isOnUpgradeList, numOfDevices, numOfRelatives)
	return logMessage
}
```

## 🔁 Request Function with Conditional Logic

```go
func Request(httpCode int, serverNode string) int {
	if httpCode == 200 && serverNode != "" {
		return 0
	} else {
		return 1
	}
}
```
## Early Return Style
Go uses early returns instead of nested conditions:
```go
func example(x, y int) int {
	if x == 1 && y == 0 {
		return 10
	}
	// More checks here...
	return 0
}
```
## 🧩 First-Class Functions

You can pass functions as arguments:
```go
func applyAddFunction(sum func(x int) int, number int) int {
	return sum(number)
}
```
## 🔁 Returning Multiple Values
```go
func triple(x, y, z int) (int, int, int) {
	x *= 3
	y *= 3
	z *= 3
	return x, y, z
}
```
## 🕙 defer Keyword
Use defer to delay execution until the end of the function:
```go
func write_to_db() {
	defer fmt.Println("DB CLOSED") // Will be done once the function is over, returns something etc, 
	fmt.Println("Writing to DB...")
	// ... repeat
	fmt.Println("DB OPERATION DONE!") 
}
```
Multiple defer calls are stacked, last-in-first-out.
## 🔒 Closures
A closure is a function that remembers variables from its enclosing scope:
```go
func adder() func(int) int {
	sum := 0
	return func(num int) int {
		sum += num
		return sum
	}
}
```
## 🧠 Currying / Partial Application
```go
func selfMath(mathFunc func(int, int) int) func(int) int {
	return func(x int) int {
		return mathFunc(x, x)
	}
}

func multiply(x, y int) int {
	return x * y
}

func add(x, y int) int {
	return x + y
}

func MATTH() {
	squareFunc := selfMath(multiply)
	doubleFunc := selfMath(add)

	fmt.Println(squareFunc(5)) // 25
	fmt.Println(doubleFunc(5)) // 10
}
```
## 🧪 Main Function

Sample usage and testing:
```go
func main() {
	RESPONSEVALUE := Request(2010, "America: Miami")

	switch RESPONSEVALUE {
	case 0:
		fmt.Println(Logger("John", "Doe", "3/21/2023", "Deluxe", false, 6, 2))
	case 1:
		fmt.Println("NO DATA FOUND")
	}

	fmt.Println(triple(42, 12, 11))

	test1, test2, _ := triple(31, 32, 24323421)
	println(test1, test2)

	fmt.Println(applyAddFunction(func(x int) int {
		return x + x
	}, 2))

	write_to_db()

	myPerson := person{}
	myPerson.name = "Mike"
	fmt.Println(myPerson)
}
```
# 🏁 Summary
- ## Prefer early returns

- ## defer is useful for cleanup

- ## Functions are first-class in Go

- ## Support for closures and partial application
