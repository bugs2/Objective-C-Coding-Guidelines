# Objective-C-Coding-Guidlines

这篇编码风格指南概括了iOS小组的编码规范，风格问题没有最好，大家统一就行，建议尽快熟悉遵循

## 介绍
###  参考三个源头
* [raywenderlich.com Objective-C编码规范](https://github.com/samlaudev/Objective-C-Coding-Style)
* [Objective-C-Coding-Guidelines-In-Chinese](https://github.com/samlaudev/Objective-C-Coding-Style)
* [Chinamobo Objective-C 编码规范](https://github.com/Chinamobo/iOS-Team-Norms/blob/master/CodeStyle.md)

### 重新排版整合成一份有两个目的
* 根据自身情况，有一些小修改
* 为方便查看


非常感谢前人做的努力！

## 目录

* [语言](#language)
* [代码组织](#code-organization)
* [空格](#spacing)
* [注释](#comments)
 * [文件注释](#file-comments)
 * [代码注释](#code-comments)
* [命名](#naming)
  * [基本原则](#name-base-rule)
  * [类和协议](#name-class-protocol)
  * [头文件](#name-headfile)
  * [函数](#name-functions)
* [方法](#methods)
    * [命名](#methods-name)
    * [存取方法](#methods-getter-setter) 
    * [委托方法](#methods-delegate)
    * [集合类操作方法](#methods-Collection)
    * [通知](#methods-Notifactions)
* [变量](#variables)
* [常量](#constants)
* [枚举类型](#enumerated-types)
* [Case语句](#case-statements)
* [私有属性](#private-properties)
* [布尔值](#booleans)
* [条件语句](#conditionals)
  * [三元操作符](#ternary-operator)
* [编码风格](#codestyle)
    * [属性特性](#property-attributes)
    * [点符号语法](#codestyle-dot-notation-syntax)
    * [下划线](#codestyle-underline)
    * [字面值（语法糖）](#codestyle-literals)
    * [Init方法](#codestyle-init-methods)
    * [类构造方法](#codestyle-class-constructor-methods)
    * [CGRect函数](#codestyle-cgrect-functions)
    * [黄金路径](#codestyle-golden-path)
    * [错误处理](#codestyle-error-handling)
    * [单例模式](#codestyle-singletons)
    * [换行符](#codestyle-line-breaks)
    * [Xcode工程](#codestyle-xcode-project)
    * [不要使用new方法](#codestyle-not-new)
    * [Public API要尽量简洁](#codestyle-simple-api)
    * [\#import和\#include](#codestyle-include-import)
    * [引用框架的根头文件](#codestyle-import-root)
    * [使用ARC](#codestyle-arc)
    * [按照定义的顺序释放资源](#codestyle-release-resource)
    * [保证NSString在赋值时被复制](#codestyle-nsstring)
    * [使用NSNumber的语法糖](#codestyle-number-)
    * [nil检查](#codestyle-check-nil)
    * [属性的线程安全](#codestyle-thread-safe)
    * [Delegate要使用弱引用](#codestyle-weak-delegate)


<b id="language"></b>
## 语言

应该使用US英语.

**应该:**

```objc
UIColor *myColor = [UIColor whiteColor];
```

**不应该:**

```objc
UIColor *myColour = [UIColor whiteColor];
```

<b id="code-organization"></b>
## 代码组织

在函数分组和protocol/delegate实现中使用`#pragma mark -`来分类方法，要遵循以下一般结构：

```objc
#pragma mark - Lifecycle
- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - UITextFieldDelegate 	//协议名使用原名
#pragma mark - UITableViewDataSource 
#pragma mark - UITableViewDelegate

#pragma mark - Event
- (IBAction)submitData:(id)sender {}
- (void)someButtonDidPressed:(UIButton*)button

#pragma mark - Public
- (void)publicMethod {}

#pragma mark - Private
- (void)privateMethod {}

#pragma mark - Setter & Getter
- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - NSCopying
- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject
- (NSString *)description {}
```

<b id="spacing"></b>
## 缩进

* 缩进使用**4**个空格，确保在Xcode偏好设置来设置。(raywenderlich.com使用**2**个空格)
* 方法大括号和其他大括号(`if`/`else`/`switch`/`while` 等.)总是在同一行语句打开但在新行中关闭。

**应该:**

```objc
if (user.isHappy) {
    //Do something
} else {
    //Do something else
}
```

**不应该:**

```objc
if (user.isHappy)
{
  //Do something
}
else {
  //Do something else
}
```

* 在方法之间应该有且只有一行，这样有利于在视觉上更清晰和更易于组织。在方法内的空白应该分离功能，但通常都抽离出来成为一个新方法。
* 优先使用auto-synthesis。但如果有必要，`@synthesize` 和 `@dynamic`应该在实现中每个都声明新的一行。
* 如果一个函数有特别多的参数或者名称很长，应该将其按照`:`来对齐分行显示：

```objective-c
-(id)initWithModel:(IPCModle)model
       ConnectType:(IPCConnectType)connectType
        Resolution:(IPCResolution)resolution
          AuthName:(NSString *)authName
          Password:(NSString *)password
               MAC:(NSString *)mac
              AzIp:(NSString *)az_ip
             AzDns:(NSString *)az_dns
             Token:(NSString *)token
             Email:(NSString *)email
          Delegate:(id<IPCConnectHandlerDelegate>)delegate;
```

在分行时，如果第一段名称过短，后续名称可以以Tab的长度（4个空格）为单位进行缩进：

```objective-c
- (void)short:(GTMFoo *)theFoo
        longKeyword:(NSRect)theRect
  evenLongerKeyword:(float)theInterval
              error:(NSError **)theError {
    ...
}
```
* 参数有Block，应该避免以冒号对齐的方式来调用方法。冒号对齐的方法包含代码块，Xcode的对齐方式令它难以辨认。

**应该:**

```objc
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
  // something
} completion:^(BOOL finished) {
  // something
}];
```

**不应该:**

```objc
// colon-aligning makes the block indentation hard to read
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

* 函数调用的格式和书写差不多，可以按照函数的长短来选择写在一行或者分成多行：
**应该:**
```objective-c
//写在一行
[myObject doFooWith:arg1 name:arg2 error:arg3];

//分行写，按照':'对齐
[myObject doFooWith:arg1
               name:arg2
              error:arg3];

//第一段名称过短的话后续可以进行缩进
[myObj short:arg1
          longKeyword:arg2
    evenLongerKeyword:arg3
                error:arg4];
```

**不应该:**

```objective-c
//错误，要么写在一行，要么全部分行
[myObject doFooWith:arg1 name:arg2
              error:arg3];
[myObject doFooWith:arg1
               name:arg2 error:arg3];

//错误，按照':'来对齐，而不是关键字
[myObject doFooWith:arg1
          name:arg2
          error:arg3];
```

* @public和@private标记符应该以**一个空格**来进行缩进：

```objective-c
@interface MyClass : NSObject {
 @public
  ...
 @private
  ...
}
@end
```

* 协议（Protocols）

在书写协议的时候注意用`<>`括起来的协议和类型名之间是没有空格的，比如`IPCConnectHandler()<IPCPreconnectorDelegate>`,这个规则适用所有书写协议的地方，包括函数声明、类声明、实例变量等等：

```objective-c
@interface MyProtocoledClass : NSObject<NSWindowDelegate> {
 @private
    id<MyFancyDelegate> _delegate;
}

- (void)setDelegate:(id<MyFancyDelegate>)aDelegate;
@end
```

* 闭包（Blocks）

根据block的长度，有不同的书写规则：

- 较短的block可以写在一行内。
- 如果分行显示的话，block的右括号`}`应该和调用block那行代码的第一个非空字符对齐。
- block内的代码采用**4个空格**的缩进。
- 如果block过于庞大，应该单独声明成一个变量来使用。
- `^`和`(`之间，`^`和`{`之间都没有空格，参数列表的右括号`)`和`{`之间有一个空格。

```objective-c
//较短的block写在一行内
[operation setCompletionBlock:^{ [self onOperationDone]; }];

//分行书写的block，内部使用4空格缩进
[operation setCompletionBlock:^{
    [self.delegate newDataAvailable];
}];

//使用C语言API调用的block遵循同样的书写规则
dispatch_async(_fileIOQueue, ^{
    NSString* path = [self sessionFilePath];
    if (path) {
      // ...
    }
});

//较长的block关键字可以缩进后在新行书写，注意block的右括号'}'和调用block那行代码的第一个非空字符对齐
[[SessionService sharedService]
    loadWindowWithCompletionBlock:^(SessionWindow *window) {
        if (window) {
          [self windowDidLoad:window];
        } else {
          [self errorLoadingWindow];
        }
    }];

//较长的block参数列表同样可以缩进后在新行书写
[[SessionService sharedService]
    loadWindowWithCompletionBlock:
        ^(SessionWindow *window) {
            if (window) {
              [self windowDidLoad:window];
            } else {
              [self errorLoadingWindow];
            }
        }];

//庞大的block应该单独定义成变量使用
void (^largeBlock)(void) = ^{
    // ...
};
[_operationQueue addOperationWithBlock:largeBlock];

//在一个调用中使用多个block，注意到他们不是像函数那样通过':'对齐的，而是同时进行了4个空格的缩进
[myObject doSomethingWith:arg1
    firstBlock:^(Foo *a) {
        // ...
    }
    secondBlock:^(Bar *b) {
        // ...
    }];
```

<b id="comments"></b>
## 注释

读没有注释代码的痛苦你我都体会过，好的注释不仅能让人轻松读懂你的程序，还能提升代码的逼格。注意注释是为了让别人看懂，而不是仅仅你自己。

<b id="file-comments"></b>
### 文件注释

每一个文件都**必须**写文件注释，文件注释通常包含

- 文件所在模块
- 作者信息
- 历史版本信息
- 版权信息
- 文件包含的内容，作用

一段良好文件注释的栗子：

```objective-c
//
//  commonLib.h
//  TBox
//  Description: 	
		This file provide some covenient tool in calling library tools. One can easily include 
	library headers he wants by declaring the corresponding macros. 
		I hope this file is not only a header, but also a useful Linux library note.
			
	History:
		2012-??-??: On about come date around middle of Year 2012, file created as "commonLib.h"
		2012-08-20: Add shared memory library; add message queue.
		2012-08-21: Add socket library (local)
		2012-08-22: Add math library
		2012-08-23: Add socket library (internet)
		2012-08-24: Add daemon function
		2012-10-10: Change file name as "AMCCommonLib.h"
		2012-12-04: Add UDP support in AMC socket library
		2013-01-07: Add basic data type such as "sint8_t"
		2013-01-18: Add CFG_LIB_STR_NUM.
		2013-01-22: Add CFG_LIB_TIMER.
		2013-01-22: Remove CFG_LIB_DATA_TYPE because there is already AMCDataTypes.h
//  Created by ZC on 2017/2/18.
//  Copyright © 2017年 Hikvison AutoMotive Technology. All rights reserved.
//
```

*文件注释的格式通常不作要求，能清晰易读就可以了，但在整个工程中风格要统一。*

<b id="code-comments"></b>
### 代码注释

好的代码应该是“自解释”（self-documenting）的，但仍然需要详细的注释来说明参数的意义、返回值、功能以及可能的副作用。

方法、函数、类、协议、类别的定义都需要注释，推荐采用Apple的标准注释风格，有两种风格。  
1./** */ 风格，使用`alt+cmd+.`自动生成，好处是可以在引用的地方`alt+点击`自动弹出注释，非常方便。  
建议用在头文件方法、函数、属性说明

2.// 风格，使用`cmd+.`自动生成，好处是比较简单。
大部分代码使用//注释，统一放在注释代码顶部，并保持相同缩进

一些良好的注释：

```objective-c
/**
 Create a new preconnector to replace the old one with given mac address.
 
 NOTICE: We DO NOT stop the old preconnector, so handle it by yourself.
 @param type       Connect type the preconnector use.
 @param macAddress Preconnector's mac address.
 */
- (void)refreshConnectorWithConnectType:(IPCConnectType)type  Mac:(NSString *)macAddress;

/**
 Stop current preconnecting when application is going to background.
 */
-(void)stopRunning;

/**
 Get the COPY of cloud device with a given mac address.
 
 @param macAddress Mac address of the device.
 @return Instance of IPCCloudDevice.
 */
-(IPCCloudDevice *)getCloudDeviceWithMac:(NSString *)macAddress;

// A delegate for NSApplication to handle notifications about app
// launch and shutdown. Owned by the main app controller.
@interface MyAppDelegate : NSObject {
  ...
}
@end
```

协议、委托的注释要明确说明其被触发的条件：

```objective-c
// Delegate - Sent when failed to init connection, like p2p failed.
-(void)initConnectionDidFailed:(IPCConnectHandler *)handler;
```

如果在注释中要引用参数名或者方法函数名，使用`||`将参数或者方法括起来以避免歧义：

```objective-c
// Sometimes we need |count| to be less than zero.

// Remember to call |StringWithoutSpaces("foo bar baz")|
```

**定义在头文件里的接口方法、属性必须要有注释！**

<b id="naming"></b>
## 命名

<b id="name-base-rule"></b>
### 基本原则

#### 清晰
Apple命名规则尽可能坚持，特别是与这些相关的[memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508))。
命名应该尽可能的清晰和简洁，但在Objective-C中，清晰比简洁更重要。由于Xcode强大的自动补全功能，我们不必担心名称过长的问题。

```objective-c
//清晰
insertObject:atIndex:

//不清晰，insert的对象类型和at的位置属性没有说明
insert:at:

//清晰
removeObjectAtIndex:

//不清晰，remove的对象类型没有说明，参数的作用没有说明
remove:
```

不要使用单词的简写，拼写出完整的单词：
```objective-c
//清晰
settingsButton;
destinationSelection
setBackgroundColor:

//不清晰，不要使用简写
setBut;
destSel
setBkgdColor:
```
然而，有部分单词简写在Objective-C编码过程中是非常常用的，以至于成为了一种规范，这些简写可以在代码中直接使用，下面列举了部分：

```objective-c
alloc   == Allocate				max    == Maximum
alt     == Alternate				min    == Minimum
app     == Application				msg    == Message
calc    == Calculate				nib    == Interface Builder archive
dealloc == Deallocate				pboard == Pasteboard
func    == Function					rect   == Rectangle
horiz   == Horizontal				Rep    == Representation (used in class name such as NSBitmapImageRep).
info    == Information				temp   == Temporary
init    == Initialize				vert   == Vertical
int     == Integer
```

临时变量可以写得很短，如 i、k、vc 这样；
临时变量可以使用匈牙利前缀，但数据类型不可以作为前缀：

```C
// OK
wCell, vcMaster, vToolbar

// 糟糕，数据类型作为前缀
bool_switchState, floatBoxHeight
```

推荐的前缀：

前缀 | 含义
-----|-----
x    | 坐标
y    | 坐标
w    | 宽度
h    | 高度
vc   | 视图控制器
v    | 视图

命名方法或者函数时要避免歧义

```objective-c
//有歧义，是返回sendPort还是send一个Port？
sendPort

//有歧义，是返回一个名字属性的值还是display一个name的动作？
displayName
```
#### 一致性

整个工程的命名风格要保持一致性，最好和苹果SDK的代码保持统一。不同类中完成相似功能的方法应该叫一样的名字，比如我们总是用`count`来返回集合的个数，不能在A类中使用`count`而在B类中使用`getNumber`。

#### 使用前缀

如果代码需要打包成Framework给别的工程使用，或者工程项目非常庞大，需要拆分成不同的模块，使用命名前缀是非常有用的。

- 前缀由大写的字母缩写组成，比如Cocoa中`NS`前缀代表Founation框架中的类，`IB`则代表Interface Builder框架。

- 可以在为类、协议、函数、常量以及typedef宏命名的时候使用前缀，但注意**不要**为成员变量或者方法使用前缀，因为他们本身就包含在类的命名空间内。

- 命名前缀的时候不要和苹果SDK框架冲突。

<b id="name-class-protocol"></b>
### 类和协议

类名以大写字母开头，应该包含一个*名词*来表示它代表的对象类型，同时可以加上必要的前缀，比如`NSString`, `NSDate`, `NSScanner`, `NSApplication`等等。

而协议名称应该清晰地表示它所执行的行为，而且要和类名区别开来，所以通常使用`ing`词尾来命名一个协议，比如`NSCopying`,`NSLocking`。

有些协议本身包含了很多不相关的功能，主要用来为某一特定类服务，这时候可以直接用类名来命名这个协议，比如`NSObject`协议，它包含了id对象在生存周期内的一系列方法。

<b id="name-headfile"></b>
### 头文件

源码的头文件名应该清晰地暗示它的功能和包含的内容：

- 如果头文件内只定义了单个类或者协议，直接用类名或者协议名来命名头文件，比如`NSLocale.h`定义了`NSLocale`类。

- 如果头文件内定义了一系列的类、协议、类别，使用其中最主要的类名来命名头文件，比如`NSString.h`定义了`NSString`和`NSMutableString`。

- 每一个Framework都应该有一个和框架同名的头文件，包含了框架中所有公共类头文件的引用，比如`Foundation.h`

- Framework中有时候会实现在别的框架中类的类别扩展，这样的文件通常使用`被扩展的框架名`+`Additions`的方式来命名，比如`NSBundle+Additions.h`。

<b id="name-functions"></b>
### 函数

在很多场合仍然需要用到函数，比如说如果一个对象是一个单例，那么应该使用函数来代替类方法执行相关操作。

函数的命名和方法有一些不同，主要是：

- 函数名称一般带有缩写前缀，表示方法所在的框架。
- 前缀后的单词以“驼峰”表示法显示，第一个单词首字母大写。

函数名的第一个单词通常是一个动词，表示方法执行的操作：

```objective-c
NSHighlightRect
NSDeallocateObject
```

如果函数返回其参数的某个属性，省略动词：

```objective-c
unsigned int NSEventMaskFromType(NSEventType type)
float NSHeight(NSRect aRect)
```

如果函数通过指针参数来返回值，需要在函数名中使用`Get`：

```objective-c
const char *NSGetSizeAndAlignment(const char *typePtr, unsigned int *sizep, unsigned int *alignp)
```

函数的返回类型是BOOL时的命名：

```objective-c
BOOL NSDecimalIsNotANumber(const NSDecimal *decimal)
```

<b id="methods"></b>
## 方法

<b id="methods-name"></b>
###  命名

在方法签名中，应该在方法类型(-/+ 符号)之后有一个空格。在方法各个段之间应该也有一个空格(符合Apple的风格)。在参数之前应该包含一个具有描述性的关键字来描述参数。

"and"这个词的用法应该保留。它不应该用于多个参数来说明，就像`initWithWidth:height`以下这个例子：

**应该:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**不应该:**

```objc
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```

Objective-C的方法名通常都比较长，这是为了让程序有更好地可读性，按苹果的说法*“好的方法名应当可以以一个句子的形式朗读出来”*。

方法一般以小写字母打头，每一个后续的单词首字母大写，方法名中不应该有标点符号（*包括下划线*），有两个例外：

- 可以用一些通用的大写字母缩写打头方法，比如`PDF`,`TIFF`等。
- 可以用带下划线的前缀来命名私有方法或者类别中的方法。

如果方法表示让对象执行一个动作，使用动词打头来命名，注意不要使用`do`，`does`这种多余的关键字，动词本身的暗示就足够了：

```objective-c
//动词打头的方法表示让对象执行一个动作
- (void)invokeWithTarget:(id)target;
- (void)selectTabViewItem:(NSTabViewItem *)tabViewItem;
```

如果方法是为了获取对象的一个属性值，直接用属性名称来命名这个方法，注意不要添加`get`或者其他的动词前缀：

```objective-c
//正确，使用属性名来命名方法
- (NSSize)cellSize;

//错误，添加了多余的动词前缀
- (NSSize)calcCellSize;
- (NSSize)getCellSize;
```

对于有多个参数的方法，务必在每一个参数前都添加关键词，关键词应当清晰说明参数的作用：

```objective-c
//正确，保证每个参数都有关键词修饰
- (void)sendAction:(SEL)aSelector toObject:(id)anObject forAllCells:(BOOL)flag;

//错误，遗漏关键词
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;

//正确
- (id)viewWithTag:(NSInteger)aTag;

//错误，关键词的作用不清晰
- (id)taggedView:(int)aTag;
```

不要用`and`来连接两个参数，通常`and`用来表示方法执行了两个相对独立的操作（*从设计上来说，这时候应该拆分成两个独立的方法*）：

```objective-c
//错误，不要使用"and"来连接参数
- (int)runModalForDirectory:(NSString *)path andFile:(NSString *)name andTypes:(NSArray *)fileTypes;

//正确，使用"and"来表示两个相对独立的操作
- (BOOL)openFile:(NSString *)fullPath withApplication:(NSString *)appName andDeactivate:(BOOL)flag;
```
方法的参数命名也有一些需要注意的地方:
- 和方法名类似，参数的第一个字母小写，后面的每一个单词首字母大写
- 不要在方法名中使用类似`pointer`,`ptr`这样的字眼去表示指针，参数本身的类型足以说明
- 不要使用只有一两个字母的参数名
- 不要使用简写，拼出完整的单词

下面列举了一些常用参数名：

```objective-c
...action:(SEL)aSelector
...alignment:(int)mode
...atIndex:(int)index
...content:(NSRect)aRect
...doubleValue:(double)aDouble
...floatValue:(float)aFloat
...font:(NSFont *)fontObj
...frame:(NSRect)frameRect
...intValue:(int)anInt
...keyEquivalent:(NSString *)charCode
...length:(int)numBytes
...point:(NSPoint)aPoint
...stringValue:(NSString *)aString
...tag:(int)anInt
...target:(id)anObject
...title:(NSString *)aString
```
<b id="methods-getter-setter"></b> 
### 存取方法（Accessor Methods）

存取方法是指用来获取和设置类属性值的方法，属性的不同类型，对应着不同的存取方法规范：

```objective-c
//属性是一个名词时的存取方法范式
- (type)noun;
- (void)setNoun:(type)aNoun;
//栗子
- (NSString *)title;
- (void)setTitle:(NSString *)aTitle;

//属性是一个形容词时存取方法的范式
- (BOOL)isAdjective;
- (void)setAdjective:(BOOL)flag;
//栗子
- (BOOL)isEditable;
- (void)setEditable:(BOOL)flag;

//属性是一个动词时存取方法的范式
- (BOOL)verbObject;
- (void)setVerbObject:(BOOL)flag;
//栗子
- (BOOL)showsAlpha;
- (void)setShowsAlpha:(BOOL)flag;
```

命名存取方法时不要将动词转化为被动形式来使用：
```objective-c
//正确
- (void)setAcceptsGlyphInfo:(BOOL)flag;
- (BOOL)acceptsGlyphInfo;

//错误，不要使用动词的被动形式
- (void)setGlyphInfoAccepted:(BOOL)flag;
- (BOOL)glyphInfoAccepted;
```

可以使用`can`,`should`,`will`等词来协助表达存取方法的意思，但不要使用`do`,和`does`：
```objective-c
//正确
- (void)setCanHide:(BOOL)flag;
- (BOOL)canHide;
- (void)setShouldCloseDocument:(BOOL)flag;
- (BOOL)shouldCloseDocument;

//错误，不要使用"do"或者"does"
- (void)setDoesAcceptGlyphInfo:(BOOL)flag;
- (BOOL)doesAcceptGlyphInfo;
```

为什么Objective-C中不适用`get`前缀来表示属性获取方法？因为`get`在Objective-C中通常只用来表示从函数指针返回值的函数：
```objective-c
//三个参数都是作为函数的返回值来使用的，这样的函数名可以使用"get"前缀
- (void)getLineDash:(float *)pattern count:(int *)count phase:(float *)phase;
```

<b id="methods-delegate"></b>
### 委托方法

当特定的事件发生时，对象会触发它注册的委托方法。委托是Objective-C中常用的传递消息的方式。委托有它固定的命名范式。

一个委托方法的第一个参数是触发它的对象，第一个关键词是触发对象的类名，除非委托方法只有一个名为`sender`的参数：
```objective-c
//第一个关键词为触发委托的类名
- (BOOL)tableView:(NSTableView *)tableView shouldSelectRow:(int)row;
- (BOOL)application:(NSApplication *)sender openFile:(NSString *)filename;

//当只有一个"sender"参数时可以省略类名
- (BOOL)applicationOpenUntitledFile:(NSApplication *)sender;
```

根据委托方法触发的时机和目的，使用`should`,`will`,`did`等关键词
```objective-c
- (void)browserDidScroll:(NSBrowser *)sender;

- (NSUndoManager *)windowWillReturnUndoManager:(NSWindow *)window;、

- (BOOL)windowShouldClose:(id)sender;
```

<b id="methods-Collection"></b>
### 集合操作类方法（Collection Methods）

有些对象管理着一系列其它对象或者元素的集合，需要使用类似“增删查改”的方法来对集合进行操作，这些方法的命名范式一般为：

```objective-c
//集合操作范式
- (void)addElement:(elementType)anObj;
- (void)removeElement:(elementType)anObj;
- (NSArray *)elements;

//栗子
- (void)addLayoutManager:(NSLayoutManager *)obj;
- (void)removeLayoutManager:(NSLayoutManager *)obj;
- (NSArray *)layoutManagers;
```

注意，如果返回的集合是无序的，使用`NSSet`来代替`NSArray`。如果需要将元素插入到特定的位置，使用类似于这样的命名：

```objective-c
- (void)insertLayoutManager:(NSLayoutManager *)obj atIndex:(int)index;
- (void)removeLayoutManagerAtIndex:(int)index;
```

如果管理的集合元素中有指向管理对象的指针，要设置成`weak`类型以防止引用循环。

下面是SDK中`NSWindow`类的集合操作方法：

```objective-c
- (void)addChildWindow:(NSWindow *)childWin ordered:(NSWindowOrderingMode)place;
- (void)removeChildWindow:(NSWindow *)childWin;
- (NSArray *)childWindows;
- (NSWindow *)parentWindow;
- (void)setParentWindow:(NSWindow *)window;
```

<b id="methods-Notifactions"></b>
### 通知
通知常用于在模块间传递消息，所以通知要尽可能地表示出发生的事件，通知的命名范式是：
	
	[触发通知的类名] + [Did | Will] + [动作] + Notification

栗子：

```objective-c
NSApplicationDidBecomeActiveNotification
NSWindowDidMiniaturizeNotification
NSTextViewDidChangeSelectionNotification
NSColorPanelColorDidChangeNotification
```
<b id="variables"></b>
## 变量

变量尽量以描述性的方式来命名。单个字符的变量命名应该尽量避免，除了在`for()`循环。

星号表示变量是指针。例如， `NSString *text` 既不是 `NSString* text` 也不是 `NSString * text`，除了一些特殊情况下常量。

[私有属性](#private-properties) 应该尽可能代替实例变量的使用。尽管使用实例变量是一种有效的方式，但更偏向于使用属性来保持代码一致性。

通过使用'back'属性(_variable，变量名前面有下划线)直接访问实例变量应该尽量避免，除了在初始化方法(`init`, `initWithCoder:`, 等…)，`dealloc` 方法和自定义的setters和getters。想了解关于如何在初始化方法和dealloc直接使用Accessor方法的更多信息，查看[这里](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6)

**应该:**

```objc
@interface RWTTutorial : NSObject

@property (strong, nonatomic) NSString *tutorialName;

@end
```

**不应该:**

```objc
@interface RWTTutorial : NSObject {
  NSString *tutorialName;
}
```

命名属性，属性和对象的存取方法相关联，属性的第一个字母小写，后续单词首字母大写，不必添加前缀。属性按功能命名成名词或者动词：

```objective-c
//名词属性
@property (strong) NSString *title;

//动词属性
@property (assign) BOOL showsAlpha;
```

属性也可以命名成形容词，这时候通常会指定一个带有`is`前缀的get方法来提高可读性：

```objective-c
@property (assign, getter=isEditable) BOOL editable;
```

命名实例变量，在变量名前加上`_`前缀（*有些有历史的代码会将`_`放在后面*），其它和命名属性一样：

```objective-c
@implementation MyClass {
    BOOL _showsTitle;
}
```


<b id="constants"></b>
## 常量

常量是容易重复被使用和无需通过查找和代替就能快速修改值。使用`const`定义浮点型或者单个的整数型常量，如果要定义一组相关的整数常量，应该优先使用枚举。常量的命名规范和函数相同：
**应该:**

```objc
static NSString * const RWTAboutViewControllerCompanyName = @"RayWenderlich.com";

static CGFloat const RWTImageThumbnailHeight = 50.0;
```

**不应该:**

```objc
#define CompanyName @"RayWenderlich.com"

#define thumbnailHeight 2
```

不要使用`#define`宏来定义常量，如果是整型常量，尽量使用枚举，浮点型常量，使用`const`定义。`#define`通常用来给编译器决定是否编译某块代码，比如常用的：

```objective-c
#ifdef DEBUG
```

注意到一般由编译器定义的宏会在前后都有一个`__`，比如*`__MACH__`*。
<b id="enumerated-types"></b>
## 枚举类型

当使用`enum`时，推荐使用新的固定基本类型规格，因为它有更强的类型检查和代码补全。建议使用 `NS_ENUM` 和 `NS_OPTIONS` 宏来定义枚举类型，参见官方的 [Adopting Modern Objective-C](https://developer.apple.com/library/ios/releasenotes/ObjectiveC/ModernizationObjC/AdoptingModernObjective-C/AdoptingModernObjective-C.html) 一文：

**例如:**

```objc
typedef NS_ENUM(NSInteger, RWTLeftMenuTopItemType) {
  RWTLeftMenuTopItemMain,
  RWTLeftMenuTopItemShows,
  RWTLeftMenuTopItemSchedule
};
```

你也可以显式地赋值(展示旧的k-style常量定义)：

```objc
typedef NS_ENUM(NSInteger, RWTGlobalConstants) {
  RWTPinSizeMin = 1,
  RWTPinSizeMax = 5,
  RWTPinCountMin = 100,
  RWTPinCountMax = 500,
};
```
定义bit map：

```objective-c
typedef NS_OPTIONS(NSUInteger, NSWindowMask) {
    NSBorderlessWindowMask      = 0,
    NSTitledWindowMask          = 1 << 0,
    NSClosableWindowMask        = 1 << 1,
    NSMiniaturizableWindowMask  = 1 << 2,
    NSResizableWindowMask       = 1 << 3
};
```

旧的k-style常量定义应该**避免**除非编写Core Foundation C的代码。

**不应该:**

```objc
enum GlobalConstants {
  kMaxPinSize = 5,
  kMaxPinCount = 500,
};
```

<b id="case-statements"></b>
## Case语句

大括号在case语句中并不是必须的，除非编译器强制要求。当一个case语句包含多行代码时，大括号应该加上。

```objc
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
有很多次，当相同代码被多个cases使用时，一个fall-through应该被使用。一个fall-through就是在case最后移除'break'语句，这样就能够允许执行流程跳转到下一个case值。为了代码更加清晰，一个fall-through需要注释一下。


```objc
switch (condition) {
  case 1:
    // ** fall-through! **
  case 2:
    // code executed for values 1 and 2
    break;
  default: 
    // ...
    break;
}

```

当在switch使用枚举类型时，'default'是不需要的。例如：

```objc
RWTLeftMenuTopItemType menuType = RWTLeftMenuTopItemMain;

switch (menuType) {
  case RWTLeftMenuTopItemMain:
    // ...
    break;
  case RWTLeftMenuTopItemShows:
    // ...
    break;
  case RWTLeftMenuTopItemSchedule:
    // ...
    break;
}
```

<b id="private-properties"></b>
## 私有属性

私有属性应该在类的实现文件中的类扩展(匿名分类)中声明，命名分类(比如`RWTPrivate `或`private`)应该从不使用除非是扩展其他类。匿名分类应该通过使用<headerfile>+Private.h文件的命名规则暴露给测试。

**例如:**

```objc
@interface RWTDetailViewController ()

@property (strong, nonatomic) GADBannerView *googleAdView;
@property (strong, nonatomic) ADBannerView *iAdView;
@property (strong, nonatomic) UIWebView *adXWebView;

@end
```

<b id="booleans"></b>
## 布尔值

Objective-C使用`YES`和`NO`。因为`true`和`false`应该只在CoreFoundation，C或C++代码使用。既然`nil`解析成`NO`，所以没有必要在条件语句比较。不要拿某样东西直接与`YES`比较，因为`YES`被定义为1和一个`BOOL`能被设置为8位。

这是为了在不同文件保持一致性和在视觉上更加简洁而考虑。

**应该:**

```objc
if (someObject) {}
if (![anotherObject boolValue]) {}
```

**不应该:**

```objc
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
if (isAwesome == YES) {} // Never do this.
if (isAwesome == true) {} // Never do this.
```

如果`BOOL`属性的名字是一个形容词，属性就能忽略"is"前缀，但要指定get访问器的惯用名称。例如：

```objc
@property (assign, getter=isEditable) BOOL editable;
```

文字和例子从这里引用[Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)


BOOL在Objective-C中被定义为`signed char`类型，这意味着一个BOOL类型的变量不仅仅可以表示`YES`(1)和`NO`(0)两个值，所以永远**不要**将BOOL类型变量直接和`YES`比较：

```objective-c
//错误，无法确定|great|的值是否是YES(1)，不要将BOOL值直接与YES比较
BOOL great = [foo isGreat];
if (great == YES)
  // ...be great!

//正确
BOOL great = [foo isGreat];
if (great)
  // ...be great!
```

同样的，也不要将其它类型的值作为BOOL来返回，这种情况下，BOOL变量只会取值的最后一个字节来赋值，这样很可能会取到0（NO）。但是，一些逻辑操作符比如`&&`,`||`,`!`的返回是可以直接赋给BOOL的：

```objective-c
//错误，不要将其它类型转化为BOOL返回
- (BOOL)isBold {
  return [self fontTraits] & NSFontBoldTrait;
}
- (BOOL)isValid {
  return [self stringValue];
}

//正确
- (BOOL)isBold {
  return ([self fontTraits] & NSFontBoldTrait) ? YES : NO;
}

//正确，逻辑操作符可以直接转化为BOOL
- (BOOL)isValid {
  return [self stringValue] != nil;
}
- (BOOL)isEnabled {
  return [self isValid] && [self isBold];
}
```

另外BOOL类型可以和`_Bool`,`bool`相互转化，但是**不能**和`Boolean`转化。

<b id="conditionals"></b>
## 条件语句

条件语句主体为了防止出错应该使用大括号包围，即使条件语句主体能够不用大括号编写(如，只用一行代码)。这些错误包括添加第二行代码和期望它成为if语句；还有，[even more dangerous defect](http://programmers.stackexchange.com/a/16530)可能发生在if语句里面一行代码被注释了，然后下一行代码不知不觉地成为if语句的一部分。除此之外，这种风格与其他条件语句的风格保持一致，所以更加容易阅读。

**应该:**

```objc
if (!error) {
  return success;
}
```

**不应该:**

```objc
if (!error)
  return success;
```

或

```objc
if (!error) return success;
```

<b id="ternary-operator"></b>
###  三元操作符

当需要提高代码的清晰性和简洁性时，三元操作符`?:`才会使用。单个条件求值常常需要它。多个条件求值时，如果使用`if`语句或重构成实例变量时，代码会更加易读。一般来说，最好使用三元操作符是在根据条件来赋值的情况下。

Non-boolean的变量与某东西比较，加上括号()会提高可读性。如果被比较的变量是boolean类型，那么就不需要括号。

**应该:**

```objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;

NSArray *items;
(items) ?: @[]
```

**不应该:**

```objc
result = a > b ? x = c > d ? c : d : y;
```

<b id="codestyle"></b>
##编码风格

每个人都有自己的编码风格，这里总结了一些比较好的Cocoa编程风格和注意点。

<b id="property-attributes"></b>
###  属性特性

所有属性特性应该显式地列出来，有助于新手阅读代码。属性特性的顺序应该是storage、atomicity，与在Interface Builder连接UI元素时自动生成代码一致。

**应该:**

```objc
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (strong, nonatomic) NSString *tutorialName;
```

**不应该:**

```objc
@property (nonatomic, weak) IBOutlet UIView *containerView;
@property (nonatomic) NSString *tutorialName;
```

NSString应该使用`copy` 而不是 `strong`的属性特性。

为什么？即使你声明一个`NSString`的属性，有人可能传入一个`NSMutableString`的实例，然后在你没有注意的情况下修改它。 

**应该:**

```objc
@property (copy, nonatomic) NSString *tutorialName;
```

**不应该:**

```objc
@property (strong, nonatomic) NSString *tutorialName;
```

<b id="codestyle-dot-notation-syntax"></b>
###  点符号语法
不要用点分语法来调用方法，只用来访问属性。这样是为了防止代码可读性问题。

```objective-c
//正确，使用点分语法访问属性
NSString *oldName = myObject.name;
myObject.name = @"Alice";

//错误，不要用点分语法调用方法
NSArray *array = [NSArray arrayWithObject:@"hello"];
NSUInteger numberOfItems = array.count;
array.release;
```

点语法是一种很方便封装访问方法调用的方式。当你使用点语法时，通过使用getter或setter方法，属性仍然被访问或修改。想了解更多，阅读[这里](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html)
点语法应该**总是**被用来访问和修改属性，因为它使代码更加简洁。[]符号更偏向于用在其他例子。



**应该:**
```objc
NSInteger arrayCount = [self.array count];
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**不应该:**
```objc
NSInteger arrayCount = self.array.count;
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```
<b id="codestyle-underline"></b>
###  下划线

当使用属性时，实例变量应该使用`self.`来访问和改变。这就意味着所有属性将会视觉效果不同，因为它们前面都有`self.`。

但有一个特例：在初始化方法里，实例变量(例如，_variableName)应该直接被使用来避免getters/setters潜在的副作用。

局部变量不应该包含下划线。

<b id="codestyle-literals"></b>
###  字面值（语法糖）

应该使用可读性更好的语法糖来构造`NSArray`，`NSDictionary`等数据结构，避免使用冗长的`alloc`,`init`方法。

**应该:**

```objc
NSArray *names = @[ @"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul" ];
NSDictionary *productManagers = @{ @"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill" };
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
```

**不应该:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

另外如果构造代码写在一行，需要在括号两端留有一个空格，使得被构造的元素于与构造语法区分开来：

```objective-c
//正确，在语法糖的"[]"或者"{}"两端留有空格
NSArray *array = @[ [foo description], @"Another String", [bar description] ];
NSDictionary *dict = @{ NSForegroundColorAttributeName : [NSColor redColor] };

//不正确，不留有空格降低了可读性
NSArray* array = @[[foo description], [bar description]];
NSDictionary* dict = @{NSForegroundColorAttributeName: [NSColor redColor]};
```

如果构造代码不写在一行内，构造元素需要使用**两个空格**来进行缩进，右括号`]`或者`}`写在新的一行，并且与调用语法糖那行代码的第一个非空字符对齐：

```objective-c
NSArray *array = @[
  @"This",
  @"is",
  @"an",
  @"array"
];

NSDictionary *dictionary = @{
  NSFontAttributeName : [NSFont fontWithName:@"Helvetica-Bold" size:12],
  NSForegroundColorAttributeName : fontColor
};
```

构造字典时，字典的Key和Value与中间的冒号`:`都要留有一个空格，多行书写时，也可以将Value对齐：

```objective-c
//正确，冒号':'前后留有一个空格
NSDictionary *option1 = @{
  NSFontAttributeName : [NSFont fontWithName:@"Helvetica-Bold" size:12],
  NSForegroundColorAttributeName : fontColor
};

//正确，按照Value来对齐
NSDictionary *option2 = @{
  NSFontAttributeName :            [NSFont fontWithName:@"Arial" size:12],
  NSForegroundColorAttributeName : fontColor
};

//错误，冒号前应该有一个空格
NSDictionary *wrong = @{
  AKey:       @"b",
  BLongerKey: @"c",
};

//错误，每一个元素要么单独成为一行，要么全部写在一行内
NSDictionary *alsoWrong= @{ AKey : @"a",
                            BLongerKey : @"b" };

//错误，在冒号前只能有一个空格，冒号后才可以考虑按照Value对齐
NSDictionary *stillWrong = @{
  AKey       : @"b",
  BLongerKey : @"c",
};
```
<b id="codestyle-init-methods"></b>
###  Init方法

Init方法应该遵循Apple生成代码模板的命名规则。返回类型应该使用`instancetype`而不是`id`

```objc
- (instancetype)init {
  self = [super init];
  if (self) {
    // ...
  }
  return self;
}
```

查看关于instancetype的文章[Class Constructor Methods](#class-constructor-methods)

<b id="codestyle-class-constructor-methods"></b>
###  类构造方法

当类构造方法被使用时，它应该返回类型是`instancetype `而不是`id`。这样确保编译器正确地推断结果类型。


```objc
@interface Airplane
+ (instancetype)airplaneWithType:(RWTAirplaneType)type;
@end
```
关于更多instancetype信息，请查看[NSHipster.com](http://nshipster.com/instancetype/)

<b id="codestyle-cgrect-functions"></b>
###  CGRect函数

当访问`CGRect`里的`x`, `y`, `width`, 或 `height`时，应该使用[`CGGeometry`函数](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html)而不是直接通过结构体来访问。引用Apple的`CGGeometry `:

> 在这个参考文档中所有的函数，接受CGRect结构体作为输入，在计算它们结果时隐式地标准化这些rectangles。因此，你的应用程序应该避免直接访问和修改保存在CGRect数据结构中的数据。相反，使用这些函数来操纵rectangles和获取它们的特性。


**应该:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0, 0.0, width, height);
```

**不应该:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```

<b id="codestyle-golden-path"></b>
###  黄金路径

当使用条件语句编码时，左手边的代码应该是"golden" 或 "happy"路径。也就是不要嵌套`if`语句，多个返回语句也是OK。

**应该:**

```objc
- (void)someMethod {
  if (![someOther boolValue]) {
	return;
  }

  //Do something important
}
```

**不应该:**

```objc
- (void)someMethod {
  if ([someOther boolValue]) {
    //Do something important
  }
}
```

<b id="codestyle-error-handling"></b>
###  错误处理

当方法通过引用来返回一个错误参数，判断返回值而不是错误变量。

**应该:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
  // Handle Error
}
```

**不应该:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
  // Handle Error
}
```

在成功的情况下，有些Apple的APIs记录垃圾值(garbage values)到错误参数(如果non-NULL)，那么判断错误值会导致false负值和crash。

<b id="codestyle-singletons"></b>
###  单例模式

单例对象应该使用线程安全模式来创建共享实例。

```objc
+ (instancetype)sharedInstance {
  static id sharedInstance = nil;

  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    sharedInstance = [[self alloc] init];
  });

  return sharedInstance;
}
```

这会防止[possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

<b id="codestyle-line-breaks"></b>
###  换行符

换行符是一个很重要的主题，因为它的风格指南主要为了打印和网上的可读性。

例如:

```objc
self.productsRequest = [[SKProductsRequest alloc] initWithProductIdentifiers:productIdentifiers];
```

一行很长的代码应该分成两行代码，下一行用两个空格隔开。

```objc
self.productsRequest = [[SKProductsRequest alloc] 
  initWithProductIdentifiers:productIdentifiers];
```

<b id="codestyle-xcode-project"></b>
###  Xcode工程

物理文件应该与Xcode工程文件保持同步来避免文件扩张。任何Xcode分组的创建应该在文件系统的文件体现。代码不仅是根据**类型**来分组，而且还可以根据**功能**来分组，这样代码更加清晰。

尽可能在target的Build Settings打开"Treat Warnings as Errors，和启用以下[additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings)。如果你需要忽略特殊的警告，使用 [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas)。

<b id="codestyle-not-new"></b>
### 不要使用new方法

尽管很多时候能用`new`代替`alloc init`方法，但这可能会导致调试内存时出现不可预料的问题。Cocoa的规范就是使用`alloc init`方法，使用`new`会让一些读者困惑。

<b id="codestyle-simple-api"></b>
### Public API要尽量简洁

共有接口要设计的简洁，满足核心的功能需求就可以了。不要设计很少会被用到，但是参数极其复杂的API。如果要定义复杂的方法，使用类别或者类扩展。

<b id="codestyle-include-import"></b>
### \#import和\#include

`#import`是Cocoa中常用的引用头文件的方式，它能自动防止重复引用文件，什么时候使用`#import`，什么时候使用`#include`呢？

- 当引用的是一个Objective-C或者Objective-C++的头文件时，使用`#import`
- 当引用的是一个C或者C++的头文件时，使用`#include`，这时必须要保证被引用的文件提供了保护域（#define guard）。

栗子：

```objective-c
#import <Cocoa/Cocoa.h>
#include <CoreFoundation/CoreFoundation.h>
#import "GTMFoo.h"
#include "base/basictypes.h"
```

为什么不全部使用`#import`呢？主要是为了保证代码在不同平台间共享时不出现问题。

<b id="codestyle-import-root"></b>
### 引用框架的根头文件

上面提到过，每一个框架都会有一个和框架同名的头文件，它包含了框架内接口的所有引用，在使用框架的时候，应该直接引用这个根头文件，而不是其它子模块的头文件，即使是你只用到了其中的一小部分，编译器会自动完成优化的。

```objective-c
//正确，引用根头文件
#import <Foundation/Foundation.h>

//错误，不要单独引用框架内的其它头文件
#import <Foundation/NSArray.h>
#import <Foundation/NSString.h>
```

<b id="codestyle-arc"></b>
### 使用ARC

除非想要兼容一些古董级的机器和操作系统，我们没有理由放弃使用ARC。在最新版的Xcode(6.2)中，ARC是自动打开的，所以直接使用就好了。

### 在init和dealloc中不要用存取方法访问实例变量

当`init``dealloc`方法被执行时，类的运行时环境不是处于正常状态的，使用存取方法访问变量可能会导致不可预料的结果，因此应当在这两个方法内直接访问实例变量。

```objective-c
//正确，直接访问实例变量
- (instancetype)init {
  self = [super init];
  if (self) {
    _bar = [[NSMutableString alloc] init];
  }
  return self;
}
- (void)dealloc {
  [_bar release];
  [super dealloc];
}

//错误，不要通过存取方法访问
- (instancetype)init {
  self = [super init];
  if (self) {
    self.bar = [NSMutableString string];
  }
  return self;
}
- (void)dealloc {
  self.bar = nil;
  [super dealloc];
}
```
<b id="codestyle-release-resource"></b>
### 按照定义的顺序释放资源

在类或者Controller的生命周期结束时，往往需要做一些扫尾工作，比如释放资源，停止线程等，这些扫尾工作的释放顺序应当与它们的初始化或者定义的顺序保持一致。这样做是为了方便调试时寻找错误，也能防止遗漏。

<b id="codestyle-nsstring"></b>
### 保证NSString在赋值时被复制

`NSString`非常常用，在它被传递或者赋值时应当保证是以复制（copy）的方式进行的，这样可以防止在不知情的情况下String的值被其它对象修改。

```objective-c
- (void)setFoo:(NSString *)aFoo {
  _foo = [aFoo copy];
}
```

<b id="codestyle-number-"></b>
### 使用NSNumber的语法糖

使用带有`@`符号的语法糖来生成NSNumber对象能使代码更简洁：

```objective-c
NSNumber *fortyTwo = @42;
NSNumber *piOverTwo = @(M_PI / 2);
enum {
  kMyEnum = 2;
};
NSNumber *myEnum = @(kMyEnum);
```

<b id="codestyle-check-nil"></b>
### nil检查

因为在Objective-C中向nil对象发送命令是不会抛出异常或者导致崩溃的，只是完全的“什么都不干”，所以，只在程序中使用nil来做逻辑上的检查。

另外，不要使用诸如`nil == Object`或者`Object == nil`的形式来判断。

```objective-c
//正确，直接判断
if (!objc) {
	...	
}

//错误，不要使用nil == Object的形式
if (nil == objc) {
	...	
}
```

<b id="codestyle-thread-safe"></b>
### 属性的线程安全

定义一个属性时，编译器会自动生成线程安全的存取方法（Atomic），但这样会大大降低性能，特别是对于那些需要频繁存取的属性来说，是极大的浪费。所以如果定义的属性不需要线程保护，记得手动添加属性关键字`nonatomic`来取消编译器的优化。

<b id="codestyle-weak-delegate"></b>
### Delegate要使用弱引用

一个类的Delegate对象通常还引用着类本身，这样很容易造成引用循环的问题，所以类的Delegate属性要设置为弱引用。

```objective-c
//delegate
@property (nonatomic, weak) id <IPCConnectHandlerDelegate> delegate;
```
