---
title: 幂等设计
date: 2019-04-29 18:08:56
tags: 幂等 http 中间件
---

### Concept

The side-effects of N > 0 identical requests is the same as for a single request.

### Background

Generally, we design interfaces, which will interact each other. What will happend, if an interface is called many times. Obviously, some bad influences will come. Duplicated data, destroy other processing flow . etc. 

### Solutions

1. Intruducing a third-part service, which is responsible for processing out job, after submitting the job, the upstream can do other thing. the third-part service will execute the job transactional.
2. Simply introducing an 'id' in the calling flow. Firstly, apply for an id from pub-service. Then, no matter in the caller side or in the called side, we know the same 'id' won't be processed two times.

### Conclusion

Simply, we choose the second solution in general scenario. While we may use the first one in some Real Big system.