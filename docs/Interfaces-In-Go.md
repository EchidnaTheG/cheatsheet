

## 🧠 What Are Interfaces?

- Interfaces define **behavior**, not structure.
- A type *implements* an interface implicitly by defining all its required methods.
- Interfaces let you write **flexible**, **decoupled**, and **reusable** code.


## 🧩 Interface Example: Shapes

```go
type shape interface {
  area() float64
  perimeter() float64
}
```

Types that match these method signatures implement the interface:

```go
type rect struct {
	width, height float64
}

func (r rect) area() float64 {
	return r.width * r.height
}

func (r rect) perimeter() float64 {
	return 2*r.width + 2*r.height
}


-------------------------------------------------------------------------------------------------


type circle struct {
	radius float64
}

func (c circle) area() float64 {
	return math.Pi * c.radius * c.radius
}

func (c circle) perimeter() float64 {
	return 2 * math.Pi * c.radius
}

```

## 📤 Interface in Use

```go
func printShapeData(s shape) {
	fmt.Printf("Area: %v - Perimeter: %v\n", s.area(), s.perimeter())
}
```

## ❓ The Empty Interface: interface{}

Every type in Go satisfies the empty interface:
```go
var x interface{}
x = 42
x = "hello"
x = struct{}{}
```
Use with caution — no behavior is enforced.

## 📌 Implicit Implementation

In Go:

    No `implements` keyword

    If a type defines all required methods, it *automatically* implements the interface

    Example: `email` and `sms` implement the `expense` interface
    ```go
    type expense interface {
	cost() float64
}
    ```
## ✅ Example Types
```go
type email struct {
	isSubscribed bool
	body         string
	toAddress    string
}

type sms struct {
	isSubscribed  bool
	body          string
	toPhoneNumber string
}
```
#### Implementing the interface:
```go
func (e email) cost() float64 {
	if !e.isSubscribed {
		return float64(len(e.body)) * 0.05
	}
	return float64(len(e.body)) * 0.01
}

func (s sms) cost() float64 {
	if !s.isSubscribed {
		return float64(len(s.body)) * 0.1
	}
	return float64(len(s.body)) * 0.03
}
```
Once we do this, the structs have implemented the expense interface, no other keyword needed, its why *everything* satisfies the empty interface, since no functions or methods are needed in order to implement it.

## 🔍 Type Assertion

You can cast an interface to its concrete type:
```go
em, ok := e.(email)
if ok {
	return em.toAddress, em.cost()
}
```
## 🔁 Better: Type Switch
```go
func getExpenseReport(e expense) (string, float64) {
	switch v := e.(type) {
	case email:
		return v.toAddress, v.cost()
	case sms:
		return v.toPhoneNumber, v.cost()
	default:
		return "", 0.0
	}
}
```

## 🧠 Key Design Tips

1. ### Keep Interfaces Small
 Define only what's absolutely needed — think behavior, not structure.

2. ### Don’t Tie Interfaces to Concrete Types
 Let types come later. Interfaces describe what needs to be done, not who does it.

3. ### Interfaces Are Not Classes
 No fields, constructors, or inheritance — just behavior contracts.

## 🚨 Naming Your Interface Methods
Bad:
```go
type Copier interface {
	Copy(string, string) int
}
```
Good!:
```go
type Copier interface {
	Copy(sourceFile string, destinationFile string) (bytesCopied int)
}
```

## 🧪 Example: Notification System
```go
type notification interface {
	importance() int
}

type directMessage struct {
	senderUsername string
	messageContent string
	priorityLevel  int
	isUrgent       bool
}

type groupMessage struct {
	groupName      string
	messageContent string
	priorityLevel  int
}

type systemAlert struct {
	alertCode      string
	messageContent string
}
```

### Implementing the interface:
```go
func (dm directMessage) importance() int {
	if dm.isUrgent {
		return 50
	}
	return dm.priorityLevel
}

func (gp groupMessage) importance() int {
	return gp.priorityLevel
}

func (sy systemAlert) importance() int {
	return 100
}
```
### Type switch to process:
```go
func processNotification(n notification) (string, int) {
	switch v := n.(type) {
	case directMessage:
		return v.senderUsername, v.importance()
	case groupMessage:
		return v.groupName, v.importance()
	case systemAlert:
		return v.alertCode, v.importance()
	default:
		return "", 0
	}
}
```


### 📦 Example: Cargo & Manifest
```go
type Cargo interface {
	manifest() string
}

Types:

type parcel struct {
	location    string
	weight      string
	isFlammable bool
	isDelicate  bool
}

type container struct {
	location    string
	weight      string
	isFlammable bool
	isDelicate  bool
	size        float64
}
```
### Methods:
```go
func (p parcel) manifest() string {
	return fmt.Sprintf("Location: %v\nWeight: %v\nIsFlammable: %v\nIsDelicate: %v", p.location, p.weight, p.isFlammable, p.isDelicate)
}

func (c container) manifest() string {
	return fmt.Sprintf("Location: %v\nWeight: %v\nIsFlammable: %v\nIsDelicate: %v\nSize: %v", c.location, c.weight, c.isFlammable, c.isDelicate, c.size)
}
```

## Sending Decision:
```go
func send(C Cargo, destination string) string {
	switch v := C.(type) {
	case parcel:
		if (v.isFlammable || v.isDelicate) && destination == "Africa" {
			return "Denied!"
		}
		return "Granted!"
	case container:
		if (v.isFlammable && v.isDelicate) && destination == "Africa" {
			return "Denied!"
		}
		return "Granted"
	default:
		return "Unknown Detected"
	}
}
```
## ✅ Sample Main
```go
func main() {
	randomDM := directMessage{"robin", "wassup", 22, false}
	user, priority := processNotification(randomDM)
	fmt.Println(user, priority)

	if priority < 50 {
		fmt.Println("Seems like we are getting some spam today...")
	}

	randomContainer := container{"america", "2812 pounds", true, false, 31331.0}
	fmt.Println(randomContainer.manifest())
	fmt.Println(send(randomContainer, "Africa"))
}
```

## ✅ Summary
- ### Interfaces are implicitly implemented

- ### Use type switches to extract underlying types

- ### Keep interfaces small and focused

- ### Use clear method signatures

- ### Interfaces are about behavior, not structure