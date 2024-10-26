Find the N-th element of a FibDigits sequence (in which each element is the sum of the separate digits of the two previous elements).
### Task description

Let's consider the following infinite sequence:

0, 1, 1, 2, 3, 5, 8, 13, 12, 7, 10, 8, 9, ...

The 0th element is 0 and the 1st element is 1. The successive elements are defined recursively. Each of them is the sum of the separate **digits** of the two previous elements.

Write a function:

> class Solution { public int solution(int N); }

that, given an integer N, returns the N-th element of the above sequence.

**Examples:**

1. Given N = 2, the function should return 1.

2. Given N = 6, the function should return 8.

3. Given N = 10, the function should return 10.

Write an ***efficient*** algorithm for the following assumptions:

> - N is an integer within the range [0..1,000,000,000].


```c#
class Solution 
{ 
	public int solution(int N) 
	{ 
		switch (N
		{ 
			case 0: 
				return 0; 
			case 1: 
				return 1; 
			default : 
				int cycle=2; 
				int n1= N<28?N:N%24; 
				n1= n1<4&&N>=28?n1+24:n1; 
				return test (n1,0,1,ref cycle); 
		} 
	} 
	
	public int test (int N,int n1, int n2, ref int Cycle) 
	{ 
		if (N==Cycle) 
			return n1+n2; 
		else 
		{ 
			Cycle++; 
			return test (N,n2,(n1+n2)/10+(n1+n2)%10,ref Cycle); 
		} 
	}
}
`

