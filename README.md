# gobobos

List of things in Go that are uncomfortable to me:
* No overload
* C++ code for map, string, slice which is good because it is optimized. But to understand how they work one needs to make some additional effort (at least download go source code)
* List all values in an enum is not trivial
* Constant types are limited to numeric, string, bool
* Dances with `append` and underlying arrays:
```
package main

import (
	"fmt"
)

func main() {
	s := []int{1, 2, 3, 4, 5}
	resultSlice := [][]int{{}}

	for _, num := range s {
		newSubsets := make([][]int, 0)
		for _, subset := range resultSlice {
			if num == 5 && len(subset) == 3 && subset[0] == 1 && subset[1] == 2 && subset[2] == 3 {
				fmt.Printf("resultSlice before append=%v; last element=%v\n", resultSlice, resultSlice[len(resultSlice)-1])
				fmt.Printf("subset=%v append %d\n", subset, num)
			}
			newSubset := append(subset, num)
			if num == 5 && len(subset) == 3 && subset[0] == 1 && subset[1] == 2 && subset[2] == 3 {
				fmt.Printf("subset after append=%v\n", subset)
				fmt.Printf("resultSlice after append=%v; last element=%v\n", resultSlice, resultSlice[len(resultSlice)-1])
			}
			newSubsets = append(newSubsets, newSubset)
		}
		resultSlice = append(resultSlice, newSubsets...)
	}
}
Output:
resultSlice before append=[[] [1] [2] [1 2] [3] [1 3] [2 3] [1 2 3] [4] [1 4] [2 4] [1 2 4] [3 4] [1 3 4] [2 3 4] [1 2 3 4]]; last element=[1 2 3 4]
subset=[1 2 3] append 5
subset after append=[1 2 3]
resultSlice after append=[[] [1] [2] [1 2] [3] [1 3] [2 3] [1 2 3] [4] [1 4] [2 4] [1 2 4] [3 4] [1 3 4] [2 3 4] [1 2 3 5]]; last element=[1 2 3 5]
```
