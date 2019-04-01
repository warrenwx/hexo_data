---
title: Sunday-Method-Pattern-Match
date: 2018-08-22 20:42:21
tags:
---
###Substr finding algorithm : Sunday

```cpp
 int sunday(char SArrary[], int iSLen, char TArrary[], int iTLen)
{
	int i = 0, j = 0, pos = -1;
	while (i < iSLen)
	{
		if (SArrary[i] == TArrary[j])
		{
			++i;
			++j;
			if (j == iTLen - 1)
			{
				pos = i - j;
			}
		}
		else
		{
			// 下一个需要比较的字符串位置next
			int next = iTLen - j + i;
			int k = iTLen - 1;
			for (; k >= 0; --k)
			{
				if (SArrary[next] == TArrary[k])
				{
					break;
				}
			}
			//找到对应的字符
			if (k >= 0)
			{
				// i的新下标位置
				i = next - k;
			}
			else
			{
				++i;
			}
			j = 0;
		}
	}

	return pos;
}
```
