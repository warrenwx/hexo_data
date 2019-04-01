---
title: Permutation
date: 2018-08-22 20:42:08
tags:
---
###Permuation with Lua code

```Lua
function print_arr(dataArr, length)
	local str = "";
	for i = 1, length do
		str = str.."\t"..tostring(dataArr[i]);
	end
	print(str)
end

function swap(data, i, index)
	t = data[index];
	data[index] = data[i];
	data[i] = t;
end

function permutation(data, sbegin, send)
	if sbegin == send then
		print_arr(data, 4);
		return;
	end

	for i = sbegin, send do
		swap(data, sbegin, i);
		permutation(data, sbegin + 1, send);
		swap(data, sbegin, i);
	end
end

data = {1, 2, 3, 4};
permutation(data, 1, 4);
```
```cpp
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

void swap(char *a, char *b){
	char temp = *a;
	*a = *b;
	*b = temp;
	temp = *a;
}

void print_arr(string str){
	for (string::iterator it = str.begin(); it != str.end(); ++it){
			cout << *it << '\t';
		}
		cout << endl;
}

void permutation(string str, int begin, int end){
	if (begin == end){
		print_arr(str);
	}

	for (int i = begin; i < str.length(); ++i){
		swap(&str[begin], &str[i]);
		permutation(str, begin + 1, end);
		swap(&str[begin], &str[i]);
	}
}

int main(){
	string str("abc");
	permutation(str, 0, str.length() - 1);
	return 0;
}
 
```