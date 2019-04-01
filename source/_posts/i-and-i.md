---
title: i-and-i
date: 2018-08-22 20:41:41
tags:
---

Generally, we use operator ++ to increase variable, especially in loop.  
raw datatype situation

We can using ++i and i++ equally. Translating the two command into assembly language, firstly i++, a = i++;

```asm

00B61AC3  mov         eax,dword ptr [i]  
00B61AC6  mov         dword ptr [a],eax  
00B61AC9  mov         ecx,dword ptr [i]  
00B61ACC  add         ecx,1  
00B61ACF  mov         dword ptr [i],ecx
++i can be expressed as follow, b = ++i;  

00B61AD2  mov         eax,dword ptr [i]  
00B61AD5  add         eax,1  
00B61AD8  mov         dword ptr [i],eax  
00B61ADB  mov         ecx,dword ptr [i]  
00B61ADE  mov         dword ptr [b],ecx
```

Obviously, they cost time equally.

Now, we consider the other situation.
user-defined datatype situation

Considering class INTEGER,  

```cpp
template <class T>
class INTEGER{
	T value;
	public:
		INTEGER(T intVal){
			value = intVal;
		}
		INTEGER(){
			value = 1;
		}
		INTEGER& operator ++(){
			value += 1;
			return *this;
		}
		INTEGER operator ++(int){
			INTEGER tempInteger(value);
			value ++;
			return tempInteger;
		}
		void print_val(){
			cout << hex << "0x" << value << endl;
		}
};
```
