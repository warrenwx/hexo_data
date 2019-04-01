---
title: Dynamic-Programming
date: 2018-08-22 20:41:35
tags:
---

Definition(DP):
dynamic programming is a method for solving a complex problem by breaking it down into a collection of simpler subproblems, solving each of those subproblems just once, and storing their solutions. The next time the same subproblem occurs, instead of recomputing its solution, one simply looks up the previously computed solution, thereby saving computation time at the expense of a modest expenditure in storage space.

母牛问题：
初始时间只有一头母牛，第二年开始每只成熟的母牛都会产下一头小母牛，一头小母牛话费3年成熟且可以再生下小母牛；求解第n年总母牛头数。
S(n) = S(n - 1) + S(n - 3)
S(1) = 1
S(2) = 2
S(3) = 3

Fibonacci数列问题：
从第三项开始每一项等于前两项的和。
S(n) = S(n - 1) + S(n - 2)
S(1) = 1
S(2) = 1

以上两个问题均可以用递归和根据公式用循环的方法来计算，时间复杂度分别为2^n和n, 以斐波那契额问题为例，代码如下：

```Java
//java version
//recursive

public static int getFibnacciRecurse(int n){
    if(n < 1){
        return 0;
    }else if(n == 1 || n == 2){
        return 1;
    }
    
    return getFibonacciRecurse(n - 1) + getFibonacciRecurse(n - 2);
}

//java version
//loop
public static int getFibonacciLoop(int n){
    
    int m1 = 1, m2 = 1;
    int index = 3, tmp = 0;
    
    while(index <= n){
        tmp = m1 + m2;
        m1 = m2;
        m2 = tmp;
        index++;
    }
    
    return tmp;
}
```

利用矩阵计算方法，为了更快的计算
考虑到A(n) = A(n - 1) + A(n - 2), 右侧可以写成矩阵相乘的形式：



对上式可以进一步变形，成：



重复使用此公式，可以得到：



计算结果，快速幂计算方法：首先考虑计算10^15，15二进制为00001111，当计算10^1之后其结果可以用来计算10^2，因此10^4，10^8也可以通过一次乘法计算出来；通过这种方式可以在log(n)的时间复杂度上计算出矩阵的幂。
以下为快速矩阵方式计算斐波那契额数列第n项代码：

```Java
//java version
//quick matrix multi of pow exp
public static long[][] MatrixPowerSpeed(long[][] matrix, int pow){
		
		long[][] tmp = matrix;
		long[][] res = new long[matrix.length][matrix[0].length];
		
		for(int i = 0; i < matrix.length; i ++){
			res[i][i] = 1;
		}
		
		while(pow != 0){
			
			if ((pow & 1) != 0) {
				res = Matrix.MatrixMulti(res, tmp);
			}
			
			tmp = Matrix.MatrixMulti(tmp, tmp);
			pow = pow >> 1;
		}
		
		return res;
	}

//basic two matrix multi
public static long[][] MatrixMulti(long[][] matrix1, long[][] matrix2){
		
		int matrix1RowLen = matrix1.length;
		int matrix1ColumnLen = matrix1[0].length;
		int matrix2RowLen = matrix2.length;
		int matrix2ColumnLen = matrix2[0].length;
		
		if (matrix1ColumnLen != matrix2RowLen) {
			System.out.println("matrix multi condition not met!");
		}
		
		long[][] res = new long[matrix1RowLen][matrix2ColumnLen];
		
		for(int i = 0; i < matrix1RowLen; i++){
			for(int j = 0; j < matrix2ColumnLen; j++){
				res[i][j] = 0;
				for(int k = 0; k < matrix1ColumnLen; k ++){
					res[i][j] += matrix1[i][k] * matrix2[k][j];
				}
			}
		}
		
		return res;
}
```