#@property 指示符
@(object-c)

###  assign
- 指示符只是简单的赋值，不更改所赋值的引用计数，主要用于 NSInteger 等基本类型
 比如：short    int   float  结构体等C语言等数据类型
    ```
       @property (nonatomic, assign) id className;
       //id必须用assign
 
    ```
 
------------------------
###atomic 和 nonatomic

- **autonmic**  ：原子操作，这个是线程安全的，当一个线程进入方法体后，其他先线程不能进入。但是执行效率低。主要用于多线程
-  **nonatomic** ：非原子操作，线程不安全， 当一个线程进入方法体后，其他线程可以进入，会造成数据的不安全，当是效率高，主要用于单线程中
- ------------------------------
###copy
- 当调用set方法对成员变量赋值的时候，会将要被赋值的对象先复制一个副本，然后在将副本赋值给该成员变量，这样的话可以保证成员变量 `不被修改`。引用计数会加1 。

- 如果没有用copy修饰的话，当成员变量是可变类型的时候或子类是可变类型的话，就可被修改，比如
 ```
 @proprety（assign）NSString *name；
  
  NSMuTableString str = [NSMuTableString stringWithString:@"sb，可改变的"]
  
  [  book setName ：str]
  //当str发生改变的时候 name属性也会跟着一起被改变
  
   ```
 
-------------------------------------

### getter和setter修饰符

> 用于自定义get和set方法名的

``` @property(assign, nonmatic, getter = name, setter = age)  ```

----------------------
###readonly 和 readwrite
-  **readonly** ：只生成get方法，没有set方法
-  **readwrite**：只生成set方法 ，没有get方法

------------------------
###return
- 主要用于计数应用加1
- 一般是用于字符串（ NSString，NSMutableString），数组（NSMutableArray，NSArray），字典对象，视图对象（UIView ），控制器对象（UIViewController）等 
比如： 
@property (nonatomic,retain) NSString * myString; 
@property (nonatomic, retain) UIView * myView; 
@property (nonatomic, retain) UIViewController * myViewController; 
>在使用ARC的情况下常用的指示符：当一个对象引用计数大于1时，该对象不该被回收，当使用ARC的话，一般很少用。
- 启用ARC机制：  - fobjc - arc

-------------------------------------
###strong和 week
-  **strong** ：强引用，只要指向被赋值的对象，那么该对象就不会被自动回收。


-   **week** ：弱引用，弱引用指向的对象，可能被回收。

>什么时候用：如果不希望该属性的引用的对象被回收的话，用strong，性能低。
>为了保证性能，避免内存泄漏，可以使用week，对应声明为week的指针，指针指向的地址一旦被释放，这些指针都被赋值为nil，能防止出现悬空指针。



- 事实上 （week==assign）（strong==retain）

>注意：苹果建议使用week代替assign

####比如 1


1  从storyboard或者xib上创建控件，在控件放在view上的时候，已经形成了如下的引用关系,以UIButton为例：
UIViewController->UIView->subView->UIButton然后你为这个UIButton声明一个weak属性  
`@property(nonatomic,weak) IBOOutlet UIButton *btn;`
相当于xib/sb对这个Button是强引用，你声明的属性对它是弱引用。

2 手动创建控件
a). 将控件声明成strong
`@property(nonatomic,strong) UIButton *btn;`
那么你在实现这个控件时只需这样：

`
_btn = [[UIButton alloc]init];
[self.view addSubview:_btn]
`

b). 将控件声明成
weak@property(nonatomic,weak) UIButton *btn;
那么你在实现这个控件时需要这样：


`
UIButton *button = [[UIButton alloc]init];
_btn = button;
[self.view addSubview:_btn];
`
有一种情况：如果label1添加到button1，label2添加到button1，而self.view addSubView button1,如果当button从父视图移除的话，label1和label2不想消失，那么就得用strong（强引用），而button用不用strong都无所谓，因为界面都消失了，要button有何用。。。。。

>#####注意：NSString一般用copy ，控件类用week ，复杂类型一般用Strong，基本数据类型（NSInteger，int ，float。。。。。）一般用assign（一般的赋值）

####比如 2

```
1>
@property (nonatomic,strong) NSString * str1; 
@property (nonatomic,week) NSString * str2; 
_str1 = [NSMUtableString alloc]initWithString:@"杠杠滴"
_str2 = str1;
_str1 = nil;
NSLog(@"%@", str2);
//输出结果为空


2>
@property (nonatomic,strong) NSString * str1; 
@property (nonatomic,strong) NSString * str2; 
_str1 = [NSMUtableString alloc]initWithString:@"杠杠滴"
_str2 = str1;
_str1 = nil;
NSLog(@"%@", _str2);
//输出结果为:杠杠滴
```
>####解释：
![Alt text](./1450627289258.png)

> #### ARC原则：只要还有一个变量指向对象，对象就会保持在内存中
> #### 1》week: str2虽然和str1指向同一段内存，但是str2并不是拥有者，所以当str1=nil，内存就被释放了，str2也就等于nil。
> ####2 》strong：str2和str1都指向同一段内存，而且都是拥有者，当str1= nil 释放后，str2仍然指向那块内存，也就是等于@“杠杆的”。


-----------------------
###unsafe_unretained
- 和week基本类似，与week不同的是，当unsafe_unretained指针引用的对象被回收后，unsafe_unretained不会被置为nil。
>注： 一般来说，unsafe_unretuained不如week指示符


#>待完善。。。。。

<font color=red>xx</font>
