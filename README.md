# <center>Objective-C代码规范</center>
### <center>    V1.0.0 </center>

### 修订履历

|版本|修订日期|修订内容|修订人|备注|
|---|---|---|---|---|
|V1.0.0|2021.05.01|初版|陈俊|-|

<br/>

## 1. 代码结构
在.m文件中的代码结构，提倡以下约定
1. 使用 #pragma mark - XXX 将函数或者方法那功能进行分组；
2. 分组之间空2行，方法之间空1行；
3. delgate或协议相关方法放到一般内容之后

示例代码：
```Objective-c
#import "XHObject.h"

@implementation XHObject
@synthesize objectName = _objectName;

#pragma mark - Lifecycle
- (instancetype)init {
    if (self = [super init]) {
        
    }
    
    return self;
}

- (void)dealloc {
    
}


#pragma mark - Public
- (instancetype)initWithObjectName:(NSString *)objectName {
    if (self = [super init]) {
        self.objectName = objectName;
        
        [self __xh_privateMethod];
    }
    
    return self;
}


#pragma mark - Private
- (void)__xh_privateMethod {
    
}


#pragma mark - Property Setter/Getter
- (void)setObjectName:(NSString *)objectName {
    _objectName = objectName;
}

- (NSString *)objectName {
    return _objectName;
}


#pragma mark - Other
- (NSString *)description {
    return [NSString stringWithFormat:@"%@_%@", [self class], self.objectName];
}

@end
```

<br/>

## 2. 注释

<br/>

### 2.1 方法注释
1. 注释应该尽量保持简洁，代码应该尽量达到能自我解释的程度；

2. 注释必须和代码保持同步，不要出现代码修改了，注释不更新的情况;

3. 公开方法需尽量注释清楚，如果公开方法有带参数,可按需对参数进行说明;

4. 公开方法的注释建议使用 /** 方法描述 \n  @param 参数名 参数描述 \n */ 或直接使用Xcode快捷键（）的方式，便于开发者通过 option 快捷键快速查看方法注释；

5. 其他内部私有方法可参照第4条，也可使用 // 单行注释 的方式描述

6. 方法内部行注释需对齐;

示例代码：
```Objective-c
/**
 通过对象名称初始化
 
 @param objectName 对象名称
 */
- (instancetype)initWithObjectName:(NSString *)objectName;
```
或者（Xcode快捷键方式）
```Objective-c
/// 通过对象名称初始化
/// @param objectName 对象名称
- (instancetype)initWithObjectName:(NSString *)objectName;
```
私有方法
```Objective-c
// 这是一个私有方法
- (void)__xh_privateMethod {
    
}
```

<br/>

### 2.2 属性注释
1. 建议属性注释使用 /** 属性描述 */注释, 或者直接使用Xcode快捷键（option + command + /，/// 属性描述），Xcode对这两种注释有提示功能，便于开发人员在编码时能更快的了解该属性的作用;

2. 建议属性与@interface ... @end之间各空1行;

示例代码：
```Objective-c
@interface XHObject : NSObject

/// 对象名称
@property (nonatomic, copy) NSString *objectName;

@end
```
或者（Xcode快捷键方式）
```Objective-c
@interface XHObject : NSObject

/** 对象名称 */
@property (nonatomic, copy) NSString *objectName;

@end
```


<br/>

## 3. 命名与编码规范

<br/>

### 3.1类名命名规则
1. 类名、类别名字及协议名字，都采用大驼峰式命名规则；

2. 文件名要能反映出它所包含的类的名称；

3. Category的文件名要包含它所扩展的那个类的名称，并且类别名称要尽量能够描述它的功能;


<br/>

### 3.2 协议编码规则


<br/>

### 3.3 控件/控制器定义命名规则

<br/>

### 3.4 方法


<br/>

#### 3.4.1 方法名和参数命名规则

#### 3.4.2 方法声明和编码规范

### 3.5 函数

### 3.6 变量

成员属性编码规则


### 3.7 常量

Foundation框架常量赋值规则


定义普通常量以字母k开头


### 3.8 枚举与宏定义


枚举编码规则


NS_ENUM定义普通枚举


NS_OPTIONS定义位枚举


### 3.9 通知和异常


通知NSNotification常量命名规则


### 3.10 异常命名规则


### 3.11 布尔值


### 3.12 条件语句


### 3.12.1 if else 条件语句

### 3.12.2 switch case 语句



### 3.13 其他规则
