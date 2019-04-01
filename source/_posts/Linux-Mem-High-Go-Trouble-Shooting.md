---
title: Linux-Mem-High-Go-Trouble-Shooting
date: 2018-11-02 17:50:59
tags:
---
![image1](https://raw.githubusercontent.com/warrenwx/hexo_data/master/files/20181102/1.png)

memory rise up to 90%, process was killed.

![image1](https://raw.githubusercontent.com/warrenwx/hexo_data/master/files/20181102/2.png)

the incoming throughput is normal, at 00:00

![image1](https://raw.githubusercontent.com/warrenwx/hexo_data/master/files/20181102/3.png)

the outcoming throughput rise abruptly, which last 4 mins (23:59 – 00:03). I think it is an occasion that concurrent files uploading.

![image1](https://raw.githubusercontent.com/warrenwx/hexo_data/master/files/20181102/4.png)

establish tcp connections, which last 8 mins (23:59 – 00:07)

![image1](https://raw.githubusercontent.com/warrenwx/hexo_data/master/files/20181102/5.png)

close connections by itself, which last 4 mins (23:59 – 00:03)

![image1](https://raw.githubusercontent.com/warrenwx/hexo_data/master/files/20181102/6.png)

analysis: at the beginning of 23:59:00, 7.6k connections will be generated. At 00:00, connections start to become closed. 00:03:00 half of the connections were fully released

The problem was occured after repairing a high concurrency scene problem, which the concurrent control was used to solve the problem of  high memory. The above is the system status of the problem.

conclusion: concurrency control is not achieved, 7.6 K connection last for 1 minutes is not the expected phenomenon, the next step is walkthrough the golang code.

It is found that when the go program processes the concurrency with the channel, the asynchronous func is used between the produce and consume procudure, so that the consumption and return resource back is so quick, and the purpose of controlling concurrency cannot be achieved.

Modification method: return the space after the product is actually consumed.
