---
title: LCS
date: 2018-08-22 20:41:48
tags:
---

###LCS Problem

```Java
int getLcs(string a, string b) {
	int lengthA = a.length() + 1, lengthB = b.length() + 1;
	vector<vector<char> > matrix;
	matrix.resize((lengthA));
	for (int i = 0; i < lengthA; ++i) {
		matrix[i].resize(lengthB);
	}	
	int max_pos = -1, max_val = -1;
	for (int i = 1; i < lengthA; ++i) {
		for (int j = 1; j < lengthB; ++j) {
			if (a[i - 1] == b[j - 1]) {
				matrix[i][j] = matrix[i - 1][j - 1] + 1;
				if (max_val < matrix[i][j]) {
					max_val = matrix[i][j];
					max_pos = i;
				}
			}
			else 
			{
				matrix[i][j] = max(matrix[i - 1][j], matrix[i][j - 1]);
			}
		}
	}
	
	return max_pos;
}
```