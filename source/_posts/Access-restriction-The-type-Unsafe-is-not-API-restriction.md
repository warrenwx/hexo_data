---
title: 'Access restriction: The type Unsafe is not API (restriction'
date: 2018-10-25 15:28:23
tags: Access restriction The type Unsafe is not API (restriction.
---
### Access restriction: The type xxx is not API (restriction
Generally, we can see this in eclipse, as visit a non-public API is forbidden, such as sun.misc.Unsafe. Here I want to cancel this check temporarily.
steps as follow:

#### step1. find project properties, and click on 'build path'
[![](https://raw.githubusercontent.com/warrenwx/hexo_data/master/files/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20181025153958.png)
#### step2. double click 'Access rules' or just edit it
[![](https://raw.githubusercontent.com/warrenwx/hexo_data/master/files/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20181025154023.png)
#### step3. add a 'rule', fill blanks like this. project rebuild.
[![](https://raw.githubusercontent.com/warrenwx/hexo_data/master/files/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20181025154219.png)
