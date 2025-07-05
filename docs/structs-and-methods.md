---
title: Structs, Pointers, and Methods in Go
---

# Structs, Pointers, and Methods in Go

---

## 📦 Why Use Pointers to Structs?

- **Efficiency**: Passing a pointer to a struct (e.g., `*Person`) avoids copying the entire struct — faster and more memory-efficient.
- **Mutability**: You can modify the original data when passing by pointer.

---

## 🧠 Struct Memory Layout Tips

Structs are stored **contiguously in memory**, field by field.  
To avoid **wasted memory** from padding:

✅ **Preferred layout:**
```go
type stats struct {
	Reach    uint16
	NumPosts uint8
	NumLikes uint8
} // 4 bytes used efficiently
```

❌ Bad layout (2 bytes wasted):
```go 
type stats struct {
	NumPosts uint8
	Reach    uint16
	NumLikes uint8
} // 6 bytes, with 2 bytes padding
```
## 🕳️ Empty Structs in Go

Empty structs take up zero bytes of memory!

```go
// anonymous empty struct
empty := struct{}{}

// named empty struct
type emptyStruct struct{}
empty := emptyStruct{}
```
### Used often in:

   - `[string]struct{}`

   - `chan struct{}`

## 🚗 Example Struct with Nested Struct
``` go
type Car struct {
	model        string
	dateAcquired string
	miles        int
	cylinders    int
	computer     struct {
		CPU     string
		CORES   int
		GPU     string
		Wattage int
		isOn    bool
		age     int
	}
}
```
## 🔁 Updating Structs via Pointer
```go
func updateCarModelStruct(pointer *Car) {
	pointer.model = "Honda"
}
```
Using a pointer allows direct modification of the original struct instance.

## 📦 Embedded Structs (Pseudo-Inheritance)

There’s no real inheritance in Go, but you can embed structs:
```go
type car struct {
  brand string
  model string
}

type truck struct {
  car      // embedded
  bedSize int
}
```
You can access lanesTruck.brand or lanesTruck.model directly.

## 🧰 Methods in Go

Go supports "methods" — functions with receivers:
```go
type rect struct {
	width  int
	height int
}

func (r rect) area() int {
	return r.width * r.height
}
```
Usage:
```go
r := rect{width: 5, height: 10}
fmt.Println(r.area())  // 50
```

## 🧪 Main Function Example
```go
func main() {
	Mycar := Car{
		model:        "Toyota",
		dateAcquired: "22/06/2014",
		miles:        78212,
		cylinders:    6,
		computer: computer{
			CPU:     "INTEL",
			CORES:   8,
			GPU:     "NVIDIA",
			Wattage: 750,
			isOn:    true,
			age:     2,
		},
	}

	Mycarpointer := &Mycar
	updateCarModelStruct(Mycarpointer)
	fmt.Println(Mycar)

	lanesTruck := truck{
		bedSize: 10,
		car: car{
			brand: "Toyota",
			model: "Tundra",
		},
	}

	fmt.Println(lanesTruck.brand) // Toyota
	fmt.Println(lanesTruck.model) // Tundra

	fmt.Println(r.area()) // 50
}
```
## ✅ Key Takeaways

- ### Use pointers to structs for performance and mutability
- ### Struct field order matters for memory efficiency
- ### Use embedded structs for composition
- ### Go supports methods with receivers
- ### Empty structs are useful for zero-size placeholders