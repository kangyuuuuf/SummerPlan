# Introduction to Algorithms

## 2 Getting Started

### 2.1 Insertion sort

Insertion sort is efficient for sorting a small number of elements. It takes two parameters: an array A containing the vale to be sorted and the number $n$ of values of sort.

```pseudocode
Instertion sort(A,n)
	for i = 2 to n
		key = A[i]
		j = i - 1
		while j > 0 and A[j] > key
			A[j+1] = A[j]
			j = j-1
		A[j+1] = key

```

This pseudocode includes two loops. The first for loop is just the simple traverse of A. Since we need to check **A[i]** and **A[i-1]**, it must start with $i= 2$. Then, we call the current element that is ready to sort as **key**. The second while loop is to sort the current element using **j**. The loop will iterate all the elements before index **i**. It will stop if all the elements are checked or the current element is large than **key**. In each while loop, **A[i+1]** will equal to **A[i],** or we can say the elements will shift one index to the right. Finally, we find the correct position and set the **A[j]** as key.

Here is the example of Insertion sort.

![image-20220525232225425](Algorithm.assets/image-20220525232225425.png)

#### Loop Invariants

It can help us to understand why an algorithm is correct. It can be divided into three parts:

**Initialization:**	It is true prior to the first iteration of the loop

**Maintenance:**	If it is true before an iteration of the loop, it remains true before the next iteration.

**Termination:** 	The loop terminates, and when it terminates, the invariant–usually along with the reason that the loop terminated–gives us a useful property that helps show that the algorithm is correct. 

### 2.2 Analyzing algorithms

#### Random-access Machine

RAM is a model of the technology that it runs on, including the resources of that technology and a way to express their costs. In and RAM model, instructions execute one after another, with no concurrent operations. It assumes that each instruction takes the same amount of time as any other instruction and that each data access takes the same amount of time as any other data access.

#### Analysis of Insertion Sort

The running time of an algorithm on a particular input is the number of instructions and data accesses exectued. 