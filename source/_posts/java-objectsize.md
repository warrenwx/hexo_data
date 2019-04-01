---
title: java-objectsize
date: 2018-10-24 18:51:11
tags: java object size sizeof
---
### One simple way to get object size
```java
<dependency>
    <groupId>com.carrotsearch</groupId>
    <artifactId>java-sizeof</artifactId>
    <version>0.0.5</version>
</dependency>

System.out.println(RamUsageEstimator.sizeOf(new String("hello world!")));
```
