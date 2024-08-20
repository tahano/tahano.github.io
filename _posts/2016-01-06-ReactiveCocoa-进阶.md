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
  
  
  
  
- ```
  RACSignal *signalA = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
	      
	      [subscriber sendNext:@"Hello"];
	      
	      [subscriber sendCompleted];
        
        return nil;
    }];
    
    RACSignal *signalB = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
              [subscriber sendNext:@"World"];
        
        [subscriber sendCompleted];
        
        return nil;
    }];
    
    RACSignal *signalC = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"!"];
        
        [subscriber sendCompleted];
        
        return nil;
    }];
    
    // 拼接 A B, 把signalA拼接到signalB后，signalA发送完成，signalB才会被激活。
    RACSignal *concatSignalAB = [signalA concat:signalB];
    
    // A B + C
    RACSignal *concatSignalABC = [concatSignalAB concat:signalC];
    
    
    // 订阅拼接的信号, 内部会按顺序订阅 A->B->C
    // 注意：第一个信号必须发送完成，第二个信号才会被激活...
    [concatSignalABC subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
  ```

- ```
  isposable *(id<RACSubscriber> subscriber) {
	        
          [subscriber sendNext:@2];
          
          return nil;
      }];
      
    }] subscribeNext:^(id x) {
      
      // 只能接收到第二个信号的值，也就是then返回信号的值
      NSLog(@"%@", x);
      
    }];
    
    ///
    输出：2
  ```
- 


- ```
  
	
	  RACSignal *signalB = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        [subscriber sendNext:@"B"];
        
        return nil;
    }];
  
    // 合并信号, 任何一个信号发送数据，都能监听到
    RACSignal *mergeSianl = [signalA merge:signalB];
  
    [mergeSianl subscribeNext:^(id x) {
        
        NSLog(@"%@", x);
    }];
    
    // 输出
  2017-01-03 13:29:08.013 ReactiveCocoa进阶[3627:718315] A
  2017-01-03 13:29:08.014 ReactiveCocoa进阶[3627:718315] B
  
    
  ```

- **作用**

	重试：只要 发送错误 `sendError:`,就会 重新执行 创建信号的Block 直到成功
	
	```
	__block int i = 0;
    
    [[[RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
        
        if (i == 5) {
            
            [subscriber sendNext:@"Hello"];
            
        } else {
            
            // 发送错误
            NSLog(@"收到错误:%d", i);
            [subscriber sendError:nil];
        }
        
        i++a进阶[2443:667226] 收到错误信息:3
  2017-01-04 14:36:51.596 ReactiveCocoa进阶[2443:667226] 收到错误信息:4
  2017-01-04 14:36:51.596 ReactiveCocoa进阶[2443:667226] Hello
  
  ```

###### replay

- **作用**

	
	

```
nViewModel alloc] init];
    }
    
    return _loginViewModel;
}
```

`LoginViewModel.h`

```
#import <UIKit/UIKit.h>

@interface Account : NSObject

@property (nonatomic, strong) NSString *account;
@property (nonatomic, strong) NSString *pwd;

@end


@interface LoginViewModel : UIViewController

@property (nonatomic, strong) Account *account;

// 是否允许登录的信号
@property (nonatomic, strong, readonly) RACSignal *enableLoginSignal;

@property (nonatomic, strong, readonly) RACCommand *LoginCommand;

@end

```

`LoginViewModel.m`

```
#import "LoginViewModel.h"

@implementation Account

@end


@interface LoginViewModel ()

@end

@implementation LoginViewModel

- (instancetype)init {
    
    if (self = [super init]) {
        [self initialBind];
    }
    return self;
}

- (void)initialBind {

    // 监听账号属性改变， 把他们合成一个信号
    _enableLoginSignal = [RACSubject combineLatest:@[RACObserve(self.account, account), RACObserve(self.account, pwd)] reduce:^id(NSString *accout, NSString *pwd){
        
        return @(accout.length && pwd.length);
    }];
    
    // 处理业务逻辑
    _LoginCommand = [[RACCommand alloc] initWithSignalBlock:^RACSignal *(id input) {
        
        NSLog(@"点击了登录");
        return [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {
            
            // 模仿网络延迟

            dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(0.5 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
                
                // 返回登录成功 发送成功信号
                [subscriber sendNext:@"登录成功"];
            });
            
            return nil;
        }];
    }];
    
    
    // 监听登录产生的数据
    [_LoginCommand.executionSignals.switchToLatest subscribeNext:^(id x) {
       
        if ([x isEqualToString:@"登录成功"]) {
            NSLog(@"登录成功");
        }
        
    }];
    
    [[_LoginCommand.executing skip:1] subscribeNext:^(id x) {
        
        if ([x isEqualToNumber:@(YES)]) {
            
            NSLog(@"正在登陆...");
        } else {
            
        // 登录成功
        NSLog(@"登陆成功");
        
        }
        
    }];
}

#pragma mark - lazyLoad

- (Account *)account
{
    if (_account == nil) {
        _account = [[Account alloc] init];
    }
    return _account;
}

- (void)viewDidLoad {
    [super viewDidLoad];
    
}

@end

```

# 实战二：网络请求数据

#### 需求
1. 请求一段网络数据，将请求到的数据在`tableView`上展示
2. 该数据为豆瓣图书的搜索返回结果，URL：url:https://api.douban.com/v2/book/search?q=悟空传

#### 分析
1. 界面的所有业务逻辑都交给**控制器**做处理
2. 网络请求交给**MV**模型处理

#### 步骤

1. 控制器提供一个视图模型（requesViewModel），处理界面的业务逻辑
2. VM提供一个命令，处理请求业务逻辑
3. 在创建命令的block中，会把请求包装成一个信号，等请求成功的时候，就会把数据传递出去。
4. 请求数据成功，应该把字典转换成模型，保存到视图模型中，控制器想用就直接从视图模型中获取。

#### 其他

网络请求与图片缓存用到了[AFNetworking](https://github.com/AFNetworking/AFNetworking) 和 [SDWebImage](https://github.com/rs/SDWebImage),自行在Pods中导入。

```
platform :ios, '8.0'

target 'ReactiveCocoa进阶' do

use_frameworks!
pod 'ReactiveCocoa', '~> 2.5'
pod 'AFNetworking'
pod 'SDWebImage'
end
```

#### 运行效果

![](https://ww3.sinaimg.cn/large/006y8lVagw1fbgw1xnz74j30bj0l4408.jpg)


#### 代码

`SearchViewController.m`

```
#import "SearchViewController.h"
#import "RequestViewModel.h"

@interface SearchViewController ()<UITableViewDataSource>

@property (nonatomic, strong) UITableView *tableView;

@property (nonatomic, strong) RequestViewModel *requesViewModel;

@end

@implementation SearchViewController

- (RequestViewModel *)requesViewModel
{
    if (_requesViewModel == nil) {
        _requesViewModel = [[RequestViewModel alloc] init];
    }
    return _requesViewModel;
}

- (void)viewDidLoad {
    [super viewDidLoad];
    
    
    self.tableView = [[UITableView alloc] initWithFrame:self.view.frame];
    
    self.tableView.dataSource = self;
    
    [self.view addSubview:self.tableView];
    
    //
    RACSignal *requesSiganl = [self.requesViewModel.reuqesCommand execute:nil];
    
    [requesSiganl subscribeNext:^(NSArray *x) {
        
        self.requesViewModel.models = x;
        
        [self.tableView reloadData];
    }];
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    return self.requesViewModel.models.count;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString *ID = @"cell";
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];
    if (cell == nil) {
        
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:ID];
    }
    
    Book *book = self.requesViewModel.models[indexPath.row];
    cell.detailTextLabel.text = book.subtitle;
    cell.textLabel.text = book.title;
    
    [cell.imageView sd_setImageWithURL:[NSURL URLWithString:book.image] placeholderImage:[UIImage imageNamed:@"cellImage"]];
    
    
    return cell;
}
@end
```

`RequestViewModel.h`

```
#import <Foundation/Foundation.h>

@interface Book : NSObject

@property (nonatomic, copy) NSString *subtitle;
@property (nonatomic, copy) NSString *title;
@property (nonatomic, copy) NSString *image;

@end

@interface RequestViewModel : NSObject

// 请求命令
@property (nonatomic, strong, readonly) RACCommand *reuqesCommand;

//模型数组
@property (nonatomic, strong) NSArray *models;


@end
```

`RequestViewModel.m
