# Objective-C学习笔记
## 基础规则
* **#import头文件包含** 
包含oc（.m后缀文件）文件要用import，c/cpp文件用include，这个不能混合使用
* **OC中的框架**
Foundation框架是OC中一个功能丰富的重要框架文件，一般都需要导入。Foundation框架是OC中的基础框架，其中包含很多重要的功能框架的头文件（NS前 缀），因此也称之为**主头文件**。
```objective-c
# import <Foundation/Foundation.h>
```
* OC程序和C/C++类似，也包括**头文件(.h)和代码文件(.m)** 如果要实现C/C++与OC的混合编程（语句级的混合编写），后缀名需要写成.mm文件。Xcode中，如需要创建一个OC的功能类，可通过创建一个**cocoa.class**实现，它将自动创建一个h和一个m文件（OC中不要求类名和文件名相同）。
* **程序入口**
在iOS项目文件中，viewcontroller.m是程序入口。

* **基本数据类型**
字符串类型：NSString（OC中字符串前要加@符号，表示为OC中的字符串对象类型）
浮点数类型：CGfloat(在coregraph框架中定义)
整数类型：NSInteger
NSObject

## OC中的类
### 类的声明和定义
*  OC要求所有的类都必须有父类，NSObject是所有类的基类，祖宗类。 interface中声明的，implement中必须给出实现；反之，则不然。	
```objc
Person *p1 = [Person new];
Person *p2 = [[Person alloc]init];
```
*  两者等价，new操作本质就是先进行内存分配alloc，然后做初始化init（init是祖宗类NSObject的初始化方法），该初始化方法会将例如NSString指针置null，将NSInteger设为0等。
* OC中实例的创建是动态创建的。因此，p1只是指向对象的指针，而非所创建的对象本身。

**类的声明interface和定义implement代码**
```Objective-C
#import<Foundation/Foundation.h>

// 类的声明，通常写入h文件中
@interface Person ：NSObject{

//add properties
//属性前加前缀下划线
    NSString *_name;
    CGfloat _number_1;
    CGfloat _number_2;
    ......
}
//via "-" add object methods
//OC中函数名的标签式写法，如下，使用标签和不使用标签；
-(void)setName:(NSString*)myname :(DataType)mynumber_1 :(DataType)mynumber_2;
-(void)setName:(NSString*)myname setNum_1:(DataType)mynumber_1 setNum_2:(DataType)mynumber_2;
......

// via "+" add class methods
//Person *p1 = [Person new]中，通过Person类名调用的new就属于类方法
//OC中以函数名前的+-号前缀，区分实例方法和类方法，前者必须通过实例名调用，后者通过类名调用；
+
@end

//类的定义，通常写入m文件中
@implement Person

@end

//调用类的主函数
Person *p1 = [Person new];
Person *p2 = [[Person alloc]init];
```

* 类中的方法可以进行声明（供外部调用），也可以只写定义不进行声明（仅供类内部其他函数调用）。**对类中的成员变量和方法的访问权限进行控制是类的一大特性——封装**，这也引出下面的set/get方法。
* **set/get方法**
    * set/get方法是为了外部调用能够读写所创建对象的成员变量 。方法名称有固定格式如下。
    **set/get方法写法**
```objc
// 声明一个类
@interface Person : NSObject{
    NSString* _name;
    NSInteger _age;
}

//set方法：方法名写法为【set*】*为变量名称，首字母需要大写
    -(void)setName:(NSString*)name;
    -(void)setAge(NSInteger)age；
    
//get方法：方法名为成员变量名（不加前缀）
    -(NSString*)name；
    -(NSInteger)age;

@end

@implement Person
    //set
    -(void)setName:(NSString*)name{
        _name = name;
    }
    -(void)setAge(NSInteger)age{
        _age = age;
    }
    
    //get
    -(NSString*)name{
        return _name;
    }
    -(NSInteger)age{
        return _age;
    }
@end
```

### 类的构造
#### 基本构造（无参构造）
* init方法是祖宗类NSObject的构造方法，因此也是所有类的基本构造方法，在类构造中一般都必需继承init方法；
* initWith* 是构造方法的固定名称，注意一定以initWith开头；
* 返回值类型包括两种(id)和(instancetype)，前者是旧版本写法，后者是正规写法，instancetype作为一种类型仅在构造方法中使用，表示构造完成后返回的对象

```objc
//构造方法写法
//super为关键词，表示继承自父类发给发
-(instancetype)initWithPerson{
    self = [super init];
    if (self){
        //初始化代码
    }
    return self;
}
```

####  有参数构造

# 协议和委托代理
## 协议
*  **协议 本质是一些方法的列表**
**协议的声明**
```objc
@protocol Mydelegate <NSObject>
	-(void)walk;
	-(void)run;
	-(void)eat;
@end
```

* 协议通过上述形式进行声明（创建一个新的h文件在其中进行声明，通常没有对应的cpp文件），其中NSObject是一个基础协议（类似于其对应的类是所有类的基类）
* 协议可以在任何的h或者cpp文件中进行声明，使用该协议的类只需要包含相应的头文件即可
* 协议名称一般以*delegate形式命名

*  与协议类似，实现对类功能扩展的还有**继承、类别、类扩展、协议** 一共四种
  * 继承
  * 类别
  * 类扩展
  * 协议

### 协议的用法
* 协议的定义，即按照上述方式在delegate的protocol中进行定义，列举遵守该协议类应具备的方法列表

* 协议的遵守，在需要遵守该协议类的头文件中约定

```objc
@interface Student : NSObject <Mydelegate>

@end
```

* 协议的实现，在类实现中对协议中的方法进行实现
  * 默认（默认使用@required关键词，必须实现）在protocol中公布的方法都需要在implement中进行实现，也可以通过@optional关键词声明可选择实现方法（未必一定实现）

# 其他

##  深拷贝与浅拷贝

深浅拷贝及协议的使用

深浅拷贝的对象（指针对象，基础数据类型不存在此概念）
深拷贝：拷贝出的新对象再申请一块内存空间并进行拷贝
浅拷贝：没有新内容空间的建立，仅将新对象的指针指向被复制对象

## oc内存管理中的retain和release

OC使用引用计数来管理内存，每一个继承NSObject的对象，内部都维护了一个引用计数器retainCount，当对象创建时(调用alloc或者new)引用计数器会+1, 手动调用retain()方法可以使引用计数器+1，手动调用release()方法可以使引用计数器-1，当引用计数器为0时，对象会自动调用"析构函数" dealloc()方法来回收资源和释放内存。
这样当一个对象被多个地方使用和管理时，可以通过retain()将引用计数器+1，来获取使用权限(防止其他使用者释放该对象)，用完了之后再通过调用release()将引用计数器-1来放弃使用权限(此时如果引用计数器为0，说明没有其他地方再使用该对象了，直接会被释放，如果引用计数器不为0，则证明还有其他地方再使用这个对象，该对象不会被释放)。这是一种设计非常优雅的内存管理机制，谁使用谁retain()用完之后release()，如果已经没有人使用它了，引用计数器为0，则释放掉。

MRU与ARU：
是oc管理内存的方式，对程序中的对象进行引用计数（通过引用计数器），每当有对象被开始使用时+1，使用完毕-1，当计数器为0时释放对象。
自动引用计数ARC：
手动引用计数MRC：
    两个关键字：
        retain：针对对象类型，是浅拷贝；用法 Person *p2 = [p retain](省略的写法只写Person *p2 = p，但这样仿佛不会进行引用计数，故规范写法应是加上retain)【**即通常的将现有对象直接赋值给新建对象指针（为什么是新建对象的指针？因为OC的对象都是动态分配在堆中，只能通过指针访问）的操作，都是浅拷贝（问题来了，如何进行深拷贝呢，请看下一个copy关键词）**】;  
        copy：针对NSString等，是深拷贝，具体分为copy和mutablecopy（可变拷贝与不可变拷贝），该方法是通过拷贝协议实现的。
    例如，OC中的NSString类遵守了拷贝协议（NSCopying协议、NSMutableCopying协议），因此可以进行拷贝操作如下。

```objc
//顺带提下 oc中nsstring的两种初始化方法
//1.实例方法，需要手动release
NSString *str = [NSString alloc]initWithFormat:@"%@",@"hello world"]
//2.类方法，自动release
NSString *str = [NSString stringWithFormat:＠"％＠"，＠"Hello World"]

//不过一般初始化还是用最简单的
NSString *str = @"hello world";
```
* 但是NSString直接使用上述@“helloworld”进行初始化，字符串并不创建在堆区，难以管理其生命周期，因此常用 NSString *str = [@“helloworld” copy]进行深拷贝，方便生命周期管理。

对上述的str对象，可进行拷贝操作如下
```objc
//1.不可变拷贝，拷贝的对象不可变
NSString *str_1 = [str copy];
//2.可变拷贝，拷贝的对象是副本，是可变的
NSString *str_1 = [str mutablecopy];
```

但是对于我们自定义创建的类，iOS默认并没有遵守这上述两个拷贝协议。
如果想自定义一下copy 那么就必须遵守NSCopying协议，并且手动在类中实现 copyWithZone: 方法;
如果想自定义一下mutablecopy 那么就必须遵守NSMutableCopying协议，并且手动在类中实现 mutableCopyWithZone: 方法。

```objc
copy协议中的NSCopying和NSMutableCopying方法
//1.copy
@protocol NSCopying
- (id)copyWithZone:(nullable NSZone *)zone;
@end

//2.mutablecopy
@protocol NSMutableCopying
- (id)copyWithZone:(nullable NSZone *)zone;
@end

//拷贝一个自定义的Person类对象，在implement中实现上述协议方法
- (id)copyWithZone:(nullable NSZone *)zone{
    // 默认写法，创建一个新的类
    Person *p = [[self class]allocWithZone:zone];
    // 创建同样的类属性
    p.name = self.name;
    p.age = self.age;
        // 类中如果持有自定义对象，则也需要通过copy方法进行深拷贝，如下(moto为Person中持有的moto类)
    p.moto = [self.moto copy];
    
    return p; 
}

// 之后就可以在主函数中copy该对象
Person *sxg = [Person new];
Person *shk = [sxg copy];
```

附注：对上述代码中的几个陌生地方做下注释
* nullable：是一个属性/参数/方法的修饰关键词。来源是在swift语言中，实现了可以通过符号？和！修饰一个对象和方法是否为optional还是non-optional。OC为了兼容该特性，引入nullable和nonnull关键词，但演变出了三种写法，作用基本等同（对于属性、方法返回值、方法参数的修饰，建议使用：nonnull/nullable；
  对于 C 函数的参数、Block 的参数、Block 返回值的修饰，建议使用：_Nonnull/_Nullable，建议弃用 __nonnull/__nullable。）。
    * nullable(__nullable、_nullable)：表示对象可以是 NULL 或 nil
    * nonnull(__nonnull、_nonnull)：表示对象不应该为空
```objc
//1.方法返回值修饰：
- (nullable NSString *)method;
- (NSString * __nullable)method;
- (NSString * _Nullable)method;

//2.声明属性的修饰：
@property (nonatomic, copy, nullable) NSString *aString;
@property (nonatomic, copy) NSString * __nullable aString;
@property (nonatomic, copy) NSString * _Nullable aString;

//3.方法参数修饰：
- (void)methodWithString:(nullable NSString *)aString;
- (void)methodWithString:(NSString * _Nullable)aString;
- (void)methodWithString:(NSString * __nullable)aString;
```
* NSZone是什么？（可先了解下一条cocoa是什么）是cocoa程序中的内存池概念（NSZone是Apple用来分配和释放内存的一种方式，它不是一个对象，而是使用C结构存储了关于对象的内存管理的信息），cocoa会默认配置一个NSZone，所有对象的alloc和dealloc默认都是在这个NSZone中进行的，但弊端是全局范围会导致内存碎片化。因此在需要大量的创建/释放object时，cocoa提供了可自定义的NSZone。
* cocoa。是苹果的一套工具框架，核心包括三个框架Fouundation（核心框架）、Application Kit（macOS）、UIKit（iOS）。来源是最初苹果收购NS公司的操作系统作为其操作系统，该操作系统中带的开发套件几经改名，最后叫cocoa，也是其中类均以NS开头的缘由。

自定义对象的拷贝

description方法

## 沙盒路径
应用程序安装在不同手机时，程序文件路径会变化，沙盒路径应用而生。对文件的操作主要是读和写：
读文件：通过NSBundle，NSBundle这个类其实就是用来定位可执行资源的。获取到具体的可执行文件的位置，然后再加载。但是通过NSBundle读取的资源文件通常不可进行写入，若需创建新文件或写入文件请看下条。
写文件：通过NSString *homePath = NSHomeDirectory()获取文件资源路径，下面有三个文件夹document、library、tmp可供开发者写入。

* nsmutablearray，存储的是对象类型，对基本数据类型无法存放。

值类型 指针类型（引用类型）

类的复合，仅是一种程序设计方法原则，目的是方便地追加类的功能

%p 对象的地址
%@ 对象（例如字符串对象）
%d 整型
%lu 无符号长整型

@class
#import “Person.h”

1.成员对象向调用对象返回消息

2.委托模型，正向委托

3.反向委托

# 编程思想
* 面向对象语言中，类是实现功能的最小载体。任何的功能都依附于某个相关的对象，它必须是在某个类或者对象中实现的方法