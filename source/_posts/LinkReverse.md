---
title: LinkReverse
date: 2018-08-22 20:41:55
tags:
---

####Link Reverse

```cpp
Node * reverse_link(Node *cur){
	if(cur == NULL){
		return cur;
	}

	if(cur->next == NULL){
		return cur;
	}

	Node *tmp = reverse_link(cur->next);
	cur->next->next = cur;
	cur->next = NULL;
	return tmp;
}

Node * reverse_link_loop(Node *cur){
	Node *pre = NULL;
	Node *next= NULL;

	while(cur){
		next = cur->next;
		cur->next = pre;
		pre = cur;
		cur = next;
	}
	return pre;
}
```