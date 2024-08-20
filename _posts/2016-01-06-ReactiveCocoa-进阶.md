---
layout:     post
title:      ReactiveCocoa 进阶
subtitle:   函数式编程框架 ReactiveCocoa 进阶
date:       2017-01-06
author:     BY
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - iOS
    - ReactiveCocoa
    - 函数式编程
    - 开源框架
---
# 前言

>



# 常见操作方法介绍

- ```
  [[_textField.rac_textSignal flattenMap:^RACStream *(id value) {
	      
	      // block调用时机：信号源发出的时候
	      
	      // block作用：改变信号的内容
        
        // 返回RACReturnSignal
        return [RACReturnSignal return:[NSString stringWithFormat:@"信号内容：%@", value]];
        
    }] subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
  ```
  
  
  
  
