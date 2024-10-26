### Task description

A retail store chain wants to expand into a new neighborhood. To make the number of clients as large as possible, the new branch should be at a distance of no more than K from all the houses in the neighborhood. A is a matrix of size N x M. It represents the neighborhood as a rectangular grid, in which each cell is an integer 0 (an empty plot) or 1 (a house). The distance between two cells is calculated as the minimum number of cell borders that one has to cross to move from the source cell to the target cell. It does not matter whether the cells on the way are empty or occupied but it does not allow for moving through corners. A store can be only built on an empty plot. How many suitable locations are there?

For example, given K = 2 and matrix A = [ [0, 0, 0, 0], [0, 0, 1, 0], [1, 0, 0, 1] ], houses are located in cells with coordinates (2, 3), (3, 1) and (3, 4). We can build a new store on two empty plots that are close enough to all the houses. The first possible empty plot is located at (3, 2). The distance to the first house at (2,3) is 2. The distance to the second house at (3, 1) is 1. The third house at (3, 4) is at a distance of 2. The second possible empty plot is located at (3, 3). The distances to the first, second and third houses are 1, 2 and 1, respectively.

![A[1] = 0000, A[2] = 0010, A[3] = 1001.
Cells at a distance of less than or equal to 2 from all houses are marked in yellow.](https://codility-frontend-prod.s3.amazonaws.com/media/task_static/building_store/1fee038e036616c686ae2dc4d4e00b78/static/images/example1.png)

Write a function:

> class Solution { public int solution(int K, int[][] A); }

that, given a positive integer K and matrix A of size N x M, returns the number of empty plots close enough to all the houses.

**Examples:**

1. Given K = 2 and A = [ [0, 0, 0, 0], [0, 0, 1, 0], [1, 0, 0, 1] ], the function should return 2, as explained above.

2. Given K = 1 and A = [ [0, 1], [0, 0] ], the function should return 2. We can build a store on empty plots at (1, 1) and (2, 2).

![A[1] = 01, A[2] = 00.
Cells at a distance of less than or equal to 1 from all houses are marked in yellow.](https://codility-frontend-prod.s3.amazonaws.com/media/task_static/building_store/e78edb253d3ee5d6997123309c39fbe8/static/images/example2.png)

3. Given K = 4 and A = [ [0, 0, 0, 1], [0, 1, 0, 0], [0, 0, 1, 0], [1, 0, 0, 0], [0, 0, 0, 0] ], the function should return 8. Stores can be built on the following plots: (1, 1), (1, 2), (2, 1), (2, 3), (3, 2), (3, 4), (4, 3) and (4, 4).

![A[1] = 0001, A[2] = 0100, A[3] = 0010, A[4] = 1000, A[5] = 0000.
Cells at distance of less than or equal to 4 from all houses are marked in yellow.](https://codility-frontend-prod.s3.amazonaws.com/media/task_static/building_store/38b37f436e6adf03b200154ac3204137/static/images/example3.png)

Write an ****efficient**** algorithm for the following assumptions:

> - K is an integer within the range [1..800];
> - N and M are integers within the range [2..400];
> - each element of matrix A is an integer within the range [0..1];
> - there is at least one house.


```c#
using System; 
class Solution 
{ 
	public int solution(int K, int[][] A) 
	{ 
		int nrow= A.Length; 
		int ncol = A[0].Length; 
		int[,] candidates = new int[nrow,ncol]; 
		int homes = 0,ncandidates=0; 
		
		for (int row = 0; row < nrow; row++) 
			for (int col = 0; col < ncol; col++) 
				if (A[row][col] > 0) 
				{ 
					homes++; 
					for (int k = row - K < 0 ? 0 : row - K; k <= row + K && k < nrow ; k++) 
					for (int l = col - K < 0 ? 0 : col - K; l <= col + K && l < ncol; l++) 
						if (Distance(row, col, k, l) <= K) 
							candidates[k, l]++; 
				} 
		for (int row = 0; row < nrow; row++) 
			for (int col = 0; col < ncol; col++) 
				if (candidates[row, col] == homes && A[row][col]==0) 
					ncandidates++; 
					
		return ncandidates; 
	} 
	
	public int Distance(int x0,int y0, int x1, int y1) 
	{ 
		return Math.Abs(x1-x0) + Math.Abs(y1-y0); 
	} 
}

```


```c#
using System; 
using System.Collections.Generic; 
using System.Linq; 

class point: IEquatable<point> 
{ 
	public int row; 
	public int col; 
	public bool valid; 
	
	public point(int Row,int Col,bool Valid) 
	{ 
		row = Row; col = Col;valid = Valid; 
	} 
	
	public bool Equals(point other) 
	{ 
		return this.row == other.row && this.col == other.col; 
	} 
} 
class Solution 
{ 
	public int solution(int K, int[][] A) 
	{ 
		int nrow= A.Length; 
		int ncol = A[0].Length; 
		List<point> list = new List<point>(); 
		List<point> candidates = new List<point>(); 
		for (int row = 0; row < nrow; row++) 
			for (int col = 0; col < ncol; col++) 
				if (A[row][col] > 0) 
					list.Add(new point(row, col,true)); 
					
		point home0 = list.First(); 
		for (int k = home0.row - K < 0 ? 0 : home0.row - K; k <= home0.row + K && k < nrow; k++) 
			for (int l = home0.col - K < 0 ? 0 : home0.col - K; l <= home0.col + K && l < ncol; l++) 
				if (Distance(home0.row, home0.col, k, l) <= K && (home0.row!=k ||home0.col!=l)) 
					candidates.Add(new point(k,l, true)); 
					
		foreach (point p in list) 
			foreach (point p2 in candidates.Where(x => x.valid)) 
				if (Distance(p2.row, p2.col, p.row, p.col) > K || p2.Equals(p)) p2.valid = false; ; 
		
		return candidates.Except(list).Count(x=>x.valid); 
	
	} 

	public int Distance(int x0,int y0, int x1, int y1) 
	{ 
		return Math.Abs(x1-x0) + Math.Abs(y1-y0);
	} 
}

```
