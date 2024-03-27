[[Go Lectures]] #Go-Basic #Go-Concept #Go-Core

- _goroutine_ is a lightweight thread of execution that works as **Asynchronous**. Help go runtime runs faster when executing multiply tasks when order doesn't matter but speed does.
- goroutine working with [[Go Channels]] for that connect concurrent goroutines.


```go
package main
import (
   "fmt"
   "time" // Import time มาเพื่อจับเวลาในการ Run
)
func buyGlassesAtSevenEleven() {
   time.Sleep(1 * time.Second);
   fmt.Println("ซื้อแว่น : ที่เซเว่น : เสร็จแล้ว");
}
func buyWatchAtCentral() {
   time.Sleep(1 * time.Second);
   fmt.Println("ซื้อนาฬิกา : ที่เซ็นทรัล : เสร็จแล้ว");
}
func buyFruitAtSiamParagon() {
   time.Sleep(1 * time.Second);
   fmt.Println("ซื้อผลไม้ : ที่สยามพารากอน : เสร็จแล้ว");
}
func buyCarAtToyota() {
   time.Sleep(1 * time.Second);
   fmt.Println("ซื้อรถ : ที่ศูนย์โตโยต้า : เสร็จแล้ว");
}
func main() {
   start := time.Now() // เริ่มจับเวลาในการ Run
   go buyGlassesAtSevenEleven();
   go buyWatchAtCentral();
   buyFruitAtSiamParagon();
   buyCarAtToyota();
  fmt.Println("ใช้เวลาในการ Run ทั้งสิ้น : ", time.Since(start), " วินาที") // แสดงเวลาที่ Run ทั้งหมด
```

![[Pasted image 20240325142636.png]]