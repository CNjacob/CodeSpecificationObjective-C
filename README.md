# <center>Objective-C代码规范</center>
### <center>    V1.0.0 </center>

### 修订履历

|版本|修订日期|修订内容|修订人|备注|
|---|---|---|---|---|
|V1.0.0|2021.05.01|初版|陈俊|-|

<br/>

## 1. 代码组织

<br/>

### 1.1 利用代码块
> 一个 GCC 非常模糊的特性，以及 Clang 也有的特性是，代码块如果在闭合的圆括号内的话，会返回最后语句的值

示例代码：

```Objective-C
NSURL *url = ({
    NSString *urlString = [NSString stringWithFormat:@"%@/%@", baseURLString, endpoint];
    [NSURL URLWithString:urlString];
});
```

<br/>

### 1.2 Pragma Mark
> #pragma mark - 是一个在类内部组织代码并且帮助你分组方法实现的好办法 \
注意⚠：即使 paragma mark 是一个好办法，但是它不是让你类里面方法数量增加的一个理由：类里面有太多方法说明类做了太多事情，需要考虑重构了。

示例代码：

```Objective-C
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

### 1.3 明确编译器警告和错误
> 编译器会标记你代码中被 Clang 规则定义为错误或警告的地方。当你发现一些代码会导致某个问题，但是暂时却解决不了，你可以明确一个错误或者警告

示例代码：

```Objective-C
- (NSInteger)divide:(NSInteger)dividend by:(NSInteger)divisor {
#error Whoa, buddy, you need to check for zero here!
    return (dividend / divisor);
}

- (float)divide:(float)dividend by:(float)divisor {
#warning Dude, don't compare floating point numbers like this!
    if (divisor != 0.0) {
        return (dividend / divisor);
    }
    else {
        return NAN;
    }
}
```

<br/>

### 1.4. 注释

<br/>

#### 1.4.1 方法注释

* 注释应该尽量保持简洁，代码应该尽量达到能自我解释的程度

* 注释必须和代码保持同步，不要出现代码修改了，注释不更新的情况

* 公开方法需尽量注释清楚，如果公开方法有带参数,可按需对参数进行说明

* 公开方法的注释建议使用 /** 方法描述 \n  @param 参数名 参数描述 \n */ 或直接使用Xcode快捷键（option + command + /）的方式，\
便于开发者通过 option 快捷键快速查看方法注释

* 其他内部私有方法可参照第4条，也可使用 // 单行注释 的方式描述

* 方法内部行注释需对齐

示例代码：

```Objective-C
/**
 通过对象名称初始化
 
 @param objectName 对象名称
 */
- (instancetype)initWithObjectName:(NSString *)objectName;
```

或者（Xcode快捷键方式）

```Objective-C
/// 通过对象名称初始化
/// @param objectName 对象名称
- (instancetype)initWithObjectName:(NSString *)objectName;
```
私有方法
```Objective-C
// 这是一个私有方法
- (void)__xh_privateMethod {
    
}
```

<br/>

#### 1.4.2 属性注释

* 建议属性注释使用 /** 属性描述 */注释, 或者直接使用Xcode快捷键（option + command + /），Xcode对这两种注释有提示功能，\
便于开发人员在编码时能更快的了解该属性的作用;

* 建议属性与@interface ... @end之间各空1行;

示例代码：

```Objective-C
@interface XHObject : NSObject

/// 对象名称
@property (nonatomic, copy) NSString *objectName;

@end
```

或者（Xcode快捷键方式）

```Objective-C
@interface XHObject : NSObject

/** 对象名称 */
@property (nonatomic, copy) NSString *objectName;

@end
```

<br/>

## 2. 命名与编码规范

<br/>

### 2.1 常量

1. 常量应该以大驼峰法命名，并以相关类名作为前缀。

推荐：

```Objective-C
static const NSTimeInterval XHSignInViewControllerFadeOutAnimationDuration = 0.4;
```

不推荐：

```Objective-C
static const NSTimeInterval fadeOutTime = 0.4;
```

2. 推荐使用常量来代替字符串字面值和数字，这样能够方便复用，而且可以快速修改而不需要查找和替换。 \
常量应该用 static 声明为静态常量，而不要用 #define，除非它明确的作为一个宏来使用

推荐：

```Objective-C
static NSString * const XHCacheControllerDidClearCacheNotification = @"XHCacheControllerDidClearCacheNotification";

static const CGFloat XHImageThumbnailHeight = 50.0f;
```

不推荐：

```Objective-C
#define XHCacheControllerDidClearCacheNotification @"XHCacheControllerDidClearCacheNotification"

#define XHImageThumbnailHeight 50.0f
```

3. 常量可以在 `.h文件` 中通过 `extern`  的形式暴露给外部，并在 `.m文件` 中为它赋值

示例代码：

```Objective-C
extern NSString *const XHCacheControllerDidClearCacheNotification;
```

4. 一般的，只有公有的常量才需要添加命名空间作为前缀。

<br/>

### 2.2 变量

1. 属性名和变量名都采用小驼峰式命名规则

示例代码：

```Objective-C
@property (nonatomic, copy) NSString *objectName;
```

```Objective-C
NSString *objectName = nil;
```

2. 不要使用含糊不清的缩写单词来命名变量

推荐：

```Objective-C
NSString *objectName;
```

不推荐：

```Objective-C
NSString *objN;
```

3. 指针符号 "*" 靠近变量名字

推荐：

```Objective-C
NSString *objectName;
```

不推荐：

```Objective-C
NSString* objectName;

NSString * objectName;

NSString*objectName;
```

4. 赋值操作符 "=" 两边各空1格

推荐：

```Objective-C
NSString *objectName = nil;
```

不推荐：

```Objective-C
NSString *objectName= nil;

NSString *objectName =nil;

NSString *objectName=nil;
```

5. property属性括号两边各空1格, 属性关键字以逗号加空格隔开

推荐：

```Objective-C
@property (nonatomic, copy) NSString *objectName;
```

不推荐：

```Objective-C
@property(nonatomic, copy) NSString *titleLabel;

@property (nonatomic,copy) NSString *titleLabel;
```

6. 属性关键字 `nonatomic` 尽量放在最前面, `strong`, `weak`, `assign`, `copy` 放在nonatomic后面

推荐：

```Objective-C
/// 对象名称
@property (nonatomic, copy) NSString *objectName;
```

不推荐：

```Objective-C
/// 对象名称
@property (copy, nonatomic) NSString *titleLabel;
```

7. 使用 property 时，优先使用点语法

示例代码：

```Objective-C
/// 对象名称
@property (nonatomic, copy) NSString *objectName;

// 访问属性时,优先使用点语法
self.objectName = @"XHObject";
```

<br/>

### 2.3 方法

1. 方法名和参数名都采用小驼峰式命名规则;

示例代码：

```Objective-C
- (instancetype)initWithObjectName:(NSString *)objectName;
```

2. 方法名可以使⽤用情态动词( can , should , will 等)来提⾼高清晰性，但请不要使⽤ do 或 does

示例代码：

```Objective-C
- (BOOL)canHide;

- (BOOL)shouldRefreshData;

- (void)willChangeData;
```

3. 方法声明中，-/+和返回值类型之间要空1个空格，方法名和参数类型之间以及参数类型和参数名之间不留空格

推荐：

```Objective-C
- (instancetype)initWithObjectName:(NSString *)objectName;
```

不推荐：

```Objective-C
-(instancetype)initWithObjectName:(NSString *)objectName;

- (instancetype) initWithObjectName:(NSString *)objectName;

- (instancetype)initWithObjectName:(NSString *) objectName;
```

4. 方法名和参数名应该尽量读起来像一句话

示例代码：

```Objective-C
// convertPoint:fromView:
- (CGPoint)convertPoint:(CGPoint)point fromView:(nullable UIView *)view;
```

5. 方法名之后空1格，紧随左大括号 `{` ，此时无需换行 \
但是右大括号 `}` 必须另起一行

推荐：

```Objective-C
- (void)setupUIView {
    // Do Something
    
}
```

不推荐：

```Objective-C
- (void)setupUIView{
    // Do Something
    
}

- (void)setupUIView
{
    // Do Something
    
}
```

6. 重载父类方法时，调用super的代码和重载的代码之间留一行空行，将super方法的调用和重载代码区隔开来

示例代码：

```Objective-C
- (void)viewWillDisappear:(BOOL)animated {
    [super viewWillDisappear:animated];
    
    // Do Something
}
```

<br/>

### 2.4 函数

> 这里的函数指纯C函数，这里提倡与苹果风格类似的约定。
* 函数名采用大驼峰式命名方式;
* 参数名采用小驼峰式命名方式;
* 如果函数和某个特定类型相关，那么函数名前缀要和类型前缀一样。

示例代码：

```Objective-C
CGRectMake(CGFloat x, CGFloat y, CGFloat width, CGFloat height)
```

<br/>

### 2.5 枚举

1. 定义枚举常量时，使用NS_ENUM或NS_OPTIONS，NS_ENUM和NS_OPTIONS都提供了类型检查

2. 定义枚举时，一定要注释，并且格式为: [枚举类型名] + [类型]

示例代码：

```Objective-C
typedef NS_ENUM(NSInteger, UIViewAnimationTransition) {
    UIViewAnimationTransitionNone,
    UIViewAnimationTransitionFlipFromLeft,
    UIViewAnimationTransitionFlipFromRight,
    UIViewAnimationTransitionCurlUp,
    UIViewAnimationTransitionCurlDown,
};

typedef NS_OPTIONS(NSUInteger, UIViewAutoresizing) {
    UIViewAutoresizingNone                 = 0,
    UIViewAutoresizingFlexibleLeftMargin   = 1 << 0,
    UIViewAutoresizingFlexibleWidth        = 1 << 1,
    UIViewAutoresizingFlexibleRightMargin  = 1 << 2,
    UIViewAutoresizingFlexibleTopMargin    = 1 << 3,
    UIViewAutoresizingFlexibleHeight       = 1 << 4,
    UIViewAutoresizingFlexibleBottomMargin = 1 << 5
};
```

<br/>

### 2.6 通知

1. 通知常量命名格式：[相关联的类名] + [Did | Will] + [具体事项名] + Notification

2. 不建议使用宏定义来定义通知常量

3. 具体可参照上文 `3.1 常量` 部分内容

示例代码：

```Objective-C
// .h文件
extern NSString *const XHCacheControllerDidClearCacheNotification;

// .m文件
static NSString * const XHCacheControllerDidClearCacheNotification = @"XHCacheControllerDidClearCacheNotification";
```

<br/>

### 2.7 异常

1. 异常名字的命名规则：[前缀] + [具体事项名] + Exception

示例代码：

```Objective-C
NSColorListIOException
```

<br/>

### 2.8 布尔值

1. Objective-C的布尔值只使用 `YES` 和 `NO`，`true` 和 `false` 一般用于C或C++的代码中

2. 不要将 某个值 或 表达式的结果 与 `YES` 或 `NO` 进行比较

推荐：

```Objective-C
if (self.isLogin) {
    // Do Something
}
```

不推荐：

```Objective-C
if (self.isLogin == YES) {
    // Do Something
}
```

3. 如果返回值为BOOL值，必须确保返回值为 `YES` 或 `NO` ，最好不要存在多值的情况

推荐：

```Objective-C
- (BOOL)isLogin {
    return (self.userToken.length != 0) ? YES : NO;
}
```

不推荐：

```Objective-C
- (BOOL)isLogin {
    return self.userToken.length;
}
```

<br/>

### 2.9 条件语句

1. 条件语句体应该总是被大括号包围

推荐：

```Objective-C
if (!error) {
    return success;
}
```

不推荐：

```Objective-C
if (!error)
    return success;
```

和

```Objective-C
if (!error) return success;
```

2. 条件语句之后空1格，紧随左大括号 `{` ，此时无需换行 \
如果有 `else` 语句 ，`else {` 或 `else if {` 另起一行 \
但是右大括号 `}` 都应另起一行

推荐：

```Objective-C
if (isLogin) {
    // Do Something

} 
else {
    // DO Something else
}
```

不推荐：

```Objective-C
if (isLogin) {
    // Do Something

} else {
    // DO Something else
}
```

3. 多层嵌套的条件语句，优先考虑条件不成立可以立即跳出的情况;

推荐：

```Objective-C
if (!a) {
    return;
}

if (!b) {
    DO Something
    return;
}

if (!c) {
    DO Something
    return;
}

// DO Something

```

不推荐：

```Objective-C
if (a) {
    if (b) {
        if (c) {
            // DO Something

        } 
    } 
}
```

4. 三目运算，应该只用在它能让代码更加清楚的地方

推荐：

```Objective-C
result = a > b ? x : y;
```

不推荐：

```Objective-C
result = a > b ? x = c > d ? c : d : y;
```

5. 当三元运算符的条件语句中已经检查的对象，与第一个返回对象相同时，可以使用 ` ? : ` 表达式

推荐：

```Objective-C
result = object ? : [self createObject];
```

不推荐：

```Objective-C
result = object ? object : [self createObject];
```

6. 当条件语句过于复杂时，可将它们提取出来赋值给一个 `BOOL` 变量

推荐：

```Objective-C
BOOL nameContainsSwift  = [sessionName containsString:@"Swift"];
BOOL isCurrentYear      = [sessionDateCompontents year] == 2021;
BOOL isSwiftSession     = nameContainsSwift && isCurrentYear;

if (isSwiftSession) {
    // Do something
}
```

不推荐：

```Objective-C
if ([sessionName containsString:@"Swift"] && [sessionDateCompontents year] == 2021) {
    // Do something
}
```

<br/>

### 2.10 switch case 语句

1. 括号在 `case` 语句里面不是必要的。但是当一个 `case` 包含了多行语句的时候，则必须加上括号

示例代码：

```Objective-C
switch (condition) {
    case 1:
        // ...
        break;
    case 2: {
        // ...
        // Multi-line example using braces
        break;
    }
    case 3:
        // ...
        break;
    default:
        // ...
        break;
}
```

2. 有时候可以使用 `fall-through` 在不同的 `case` 里面执行同一段代码

示例代码：

```Objective-C
switch (condition) {
    case 1:
    case 2:
        // code executed for values 1 and 2
        break;
    default: 
        // ...
        break;
}
```

3. 在 `switch` 语句里面使用一个可枚举的变量的时候，`default` 是不必要的，好处是： \
为了避免使用默认的 `case`，如果新的值加入到 `enum`，程序员会马上收到一个 `warning` 通知

示例代码：

```Objective-C
switch (menuType) {
    case XHEnumNone:
        // ...
        break;
    case XHEnumValue1:
        // ...
        break;
    case XHEnumValue2:
        // ...
        break;
}
```

<br/>

## 3. 其他规则

1. 缩进使用 4 个空格，尽量不要使用 `tab` 键。

2. 建议：每行代码的长度最多不超过120个字符;

3. 建议：为了简洁和便于阅读,尝试将单个函数或方法的实现代码控制在50行内;

4. 建议：单个实现文件里的代码行数控制在500～600行内;

5. 建议：当单个实现文件里的代码行数接近或超过800行时，就应当开始考虑使用 `Category` 分割实现文件了。


## 4. 参考链接
[《禅与 Objective-C 编程艺术》](https://github.com/objc-zen/objc-zen-book)

[《简书·DH_Fantasy 》](https://www.jianshu.com/p/571cc736dc86)
