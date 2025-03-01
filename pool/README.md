# Pool

## Static
A static pool of workers designed for handling tasks concurrently. The worker routines remain active until the
pool is closed, making it suitable for continuous work. Submitting a task after the pool has been closed is not allowed.
However, if the context has been cancelled, any subsequent submissions will be ignored and return immediately.

### Example
```go
package main

import (
	"context"
	"fmt"
	"github.com/gavraz/pool"
)

func main() {
	ctx, cancel := context.WithCancel(context.Background())
	defer cancel()

	// Start a new static pool with 3 workers
	p := pool.StartNewStatic(ctx, 3)

	for i := 0; i < 10; i++ {
		n := i
		p.Submit(func(ctx context.Context) {
			fmt.Printf("Task %d is being processed\n", n)
		})
	}

	wait := p.Close()
	wait() // optional: wait for all routines and tasks to complete
	fmt.Println("All tasks completed.")
}

```