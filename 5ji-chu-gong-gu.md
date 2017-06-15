> # 基础巩固

---

![](http://www.resumetarget.com/blog/wp-content/uploads/2013/06/bad-interview.jpg)

> ## 什么情况使用 weak 关键字，相比 assign 有什么不同？

* 什么情况使用 weak 关键字？

* 在 ARC 中,在有可能出现循环引用的时候,往往要通过让其中一端使用 weak 来解决,比如: delegate 代理属性

* 自身已经对它进行一次强引用,没有必要再强引用一次,此时也会使用 weak,自定义 IBOutlet 控件属性一般也使用 weak；当然，也可以使用strong。在下文也有论述：_**《IBOutlet连出来的视图属性为什么可以被设置成weak?》**_

* 不同点：

* `weak`此特质表明该属性定义了一种“非拥有关系” \(nonowning relationship\)。为这种属性设置新值时，设置方法既不保留新值，也不释放旧值。此特质同assign类似，然而在属性所指的对象遭到摧毁时，属性值也会清空\(nil out\)。而`assign`的“设置方法”只会执行针对“纯量类型” \(scalar type，例如 CGFloat 或NSlnteger 等\)的简单赋值操作。

* assign 可以用非 OC 对象,而 weak 必须用于 OC 对象

> ## 怎么用 copy 关键字？

* NSString、NSArray、NSDictionary 等等经常使用copy关键字，是因为他们有对应的可变类型：NSMutableString、NSMutableArray、NSMutableDictionary；

* block 也经常使用 copy 关键字，block 使用 copy 是从 MRC 遗留下来的“传统”,在 MRC 中,方法内部的 block 是在栈区的,使用 copy 可以把它放到堆区.在 ARC 中写不写都行：对于 block 使用 copy 还是 strong 效果是一样的，但写上 copy 也无伤大雅，还能时刻提醒我们：编译器自动对 block 进行了 copy 操作。如果不写 copy ，该类的调用者有可能会忘记或者根本不知道“编译器会自动对 block 进行了 copy 操作”，他们有可能会在调用之前自行拷贝属性值。这种操作多余而低效。你也许会感觉我这种做法有些怪异，不需要写依然写。如果你这样想，其实是你“日用而不知”，你平时开发中是经常在用我说的这种做法的，比如下面字符串属性不写copy也行，但是你会选择写还是不写呢？

* copy 此特质所表达的所属关系与 strong 类似。然而设置方法并不保留新值，而是将其“拷贝” \(copy\)。当属性类型为 NSString 时，经常用此特质来保护其封装性，因为传递给设置方法的新值有可能指向一个 NSMutableString 类的实例。这个类是 NSString 的子类，表示一种可修改其值的字符串，此时若是不拷贝字符串，那么设置完属性之后，字符串的值就可能会在对象不知情的情况下遭人更改。所以，这时就要拷贝一份“不可变” \(immutable\)的字符串，确保对象中的字符串值不会无意间变动。只要实现属性所用的对象是“可变的” \(mutable\)，就应该在设置新属性值时拷贝一份。

* 用 @property 声明 NSString、NSArray、NSDictionary 经常使用 copy 关键字，是因为他们有对应的可变类型：NSMutableString、NSMutableArray、NSMutableDictionary，他们之间可能进行赋值操作，为确保对象中的字符串值不会无意间变动，应该在设置新属性值时拷贝一份。

> ## 这个写法会出什么问题： @property \(copy\) NSMutableArray \*array？

* 添加,删除,修改数组内的元素的时候,程序会因为找不到对应的方法而崩溃.因为 copy 就是复制一个不可变 NSArray 的对象；

* 默认使用了 atomic 属性会严重影响性能， 该属性使用了同步锁，会在创建时生成一些额外的代码用于帮助编写多线程程序，这会带来性能问题，通过声明 nonatomic 可以节省这些虽然很小但是不必要额外开销。一般情况下并不要求属性必须是“原子的”，因为这并不能保证“线程安全” \( thread safety\)，若要实现“线程安全”的操作，还需采用更为深层的锁定机制才行。例如，一个线程在连续多次读取某属性值的过程中有别的线程在同时改写该值，那么即便将属性声明为 atomic，也还是会读到不同的属性值。

> ## 如何让自己的类用 copy 修饰符？如何重写带 copy 关键字的 setter？

* 若想令自己所写的对象具有拷贝功能，则需实现 NSCopying 协议。如果自定义的对象分为可变版本与不可变版本，那么就要同时实现 NSCopying 与 NSMutableCopying 协议。

* 具体步骤：- \(id\)copyWithZone:\(NSZone \*\)zone;一提到让自己的类用 copy 修饰符，我们总是想覆写copy方法，其实真正需要实现的却是 “copyWithZone” 方法。

> ## @property 的本质是什么？ivar、getter、setter 是如何生成并添加到这个类中的

* @property 的本质是什么？@property = ivar + getter + setter。“属性” \(property\)有两大概念：ivar（实例变量）、存取方法（access method ＝ getter + setter）。“属性” \(property\)作为 Objective-C 的一项特性，主要的作用就在于封装对象中的数据。 Objective-C 对象通常会把其所需要的数据保存为各种实例变量。实例变量一般通过“存取方法”\(access method\)来访问。其中，“获取方法” \(getter\)用于读取变量值，而“设置方法” \(setter\)用于写入变量值。

* property在runtime中是objc\_property\_t定义如下：typedef struct objc\_property \*objc\_property\_t；而objc\_property是一个结构体，包括name和attributes，定义如下：

```
struct property_t {

    const char *name;

    const char *attributes;

}；
```

```
typedef struct {

    const char *name; /**< The name of the attribute */

    const char *value; /**< The value of the attribute (usually empty) */

} objc_property_attribute_t;
```

* ivar、getter、setter 是如何生成并添加到这个类中的?“自动合成”\( autosynthesis\)。完成属性定义后，编译器会自动编写访问这些属性所需的方法，此过程叫做“自动合成”\(autosynthesis\)。需要强调的是，这个过程由编译器在编译期执行，所以编辑器里看不到这些“合成方法”\(synthesized method\)的源代码。除了生成方法代码 getter、setter 之外，编译器还要自动向类中添加适当类型的实例变量，并且在属性名前面加下划线，以此作为实例变量的名字。在前例中，会生成两个实例变量，其名称分别为 \_firstName 与 \_lastName。也就是说我们每次在增加一个属性,系统都会在 ivar\_list 中添加一个成员变量的描述,在 method\_list 中增加 setter 与 getter 方法的描述,在属性列表中增加一个属性的描述,然后计算该属性在对象中的偏移量,然后给出 setter 与 getter 方法对应的实现,在 setter 方法中从偏移量的位置开始赋值,在 getter 方法中从偏移量开始取值,为了能够读取正确字节数,系统对象偏移量的指针类型进行了类型强转.

> ## @protocol 和 category 中如何使用 @property？

* 在 protocol 中使用 property 只会生成 setter 和 getter 方法声明,我们使用属性的目的,是希望遵守我协议的对象能实现该属性

* category 使用 @property 也是只会生成 setter 和 getter 方法的声明,如果我们真的需要给 category 增加属性的实现,需要借助于运行时的两个函数：

```
1. objc_setAssociatedObject

2. objc_getAssociatedObject
```

> ## runtime 如何实现 weak 属性？

* weak 属性的特点：weak 此特质表明该属性定义了一种“非拥有关系” \(nonowning relationship\)。为这种属性设置新值时，设置方法既不保留新值，也不释放旧值。此特质同 assign 类似， 然而在属性所指的对象遭到摧毁时，属性值也会清空\(nil out\)。

* runtime 对注册的类， 会进行布局，对于 weak 对象会放入一个 hash 表中。 用 weak 指向的对象内存地址作为 key，当此对象的引用计数为0的时候会 dealloc，假如 weak 指向的对象内存地址是a，那么就会以a为键， 在这个 weak 表中搜索，找到所有以a为键的 weak 对象，从而设置为 nil。

> ## @property中有哪些属性关键字？/ @property 后面可以有哪些修饰符？

属性可以拥有的特质分为四类:

1. 原子性--- nonatomic 特质。在默认情况下，由编译器合成的方法会通过锁定机制确保其原子性\(atomicity\)。如果属性具备 nonatomic 特质，则不使用自旋锁。请注意，尽管没有名为“atomic”的特质\(如果某属性不具备 nonatomic 特质，那它就是“原子的” \( atomic\) \)，但是仍然可以在属性特质中写明这一点，编译器不会报错。若是自己定义存取方法，那么就应该遵从与属性特质相符的原子性。

2. 读/写权限---readwrite\(读写\)、readonly \(只读\)

3. 内存管理语义---assign、strong、 weak、unsafe\_unretained、copy

4. 方法名---getter=&lt;name&gt; 、setter=&lt;name&gt;。@property \(nonatomic, getter=isOn\) BOOL on。在数据反序列化、转模型的过程中，服务器返回的字段如果以 init 开头，所以你需要定义一个 init 开头的属性，但默认生成的 setter 与 getter 方法也会以 init 开头，而编译器会把所有以 init 开头的方法当成初始化方法，而初始化方法只能返回 self 类型，因此编译器会报错。

> ## weak属性需要在dealloc中置nil么？

不需要。在ARC环境无论是强指针还是弱指针都无需在 dealloc 设置为 nil ， ARC 会自动帮我们处理，即便是编译器不帮我们做这些，weak也不需要在 dealloc 中置nil：在属性所指的对象遭到摧毁时，属性值也会清空\(nil out\)。

> ## @synthesize和@dynamic分别有什么作用？

1. @property有两个对应的词，一个是 @synthesize，一个是 @dynamic。如果 @synthesize和 @dynamic都没写，那么默认的就是@syntheszie var = \_var;

2. @synthesize 的语义是如果你没有手动实现 setter 方法和 getter 方法，那么编译器会自动为你加上这两个方法。

3. @dynamic 告诉编译器：属性的 setter 与 getter 方法由用户自己实现，不自动生成。（当然对于 readonly 的属性只需提供 getter 即可）。假如一个属性被声明为 @dynamic var，然后你没有提供 @setter方法和 @getter 方法，编译的时候没问题，但是当程序运行到 instance.var = someVar，由于缺 setter 方法会导致程序崩溃；或者当运行到 someVar = var 时，由于缺 getter 方法同样会导致崩溃。编译时没问题，运行时才执行相应的方法，这就是所谓的动态绑定。

> ## ARC下，不显式指定任何属性关键字时，默认的关键字都有哪些？

* 对应基本数据类型默认关键字是

```
atomic,readwrite,assign
```

* 对于普通的 Objective-C 对象

```
atomic,readwrite,strong
```

> ## 用@property声明的NSString（或NSArray，NSDictionary）经常使用copy关键字，为什么？如果改用strong关键字，可能造成什么问题？

* 因为父类指针可以指向子类对象,使用 copy 的目的是为了让本对象的属性不受外界影响,使用 copy 无论给我传入是一个可变对象还是不可对象,我本身持有的就是一个不可变的副本.

* 如果我们使用是 strong ,那么这个属性就有可能指向一个可变对象,如果这个可变对象在外部被修改了,那么会影响该属性.copy 此特质所表达的所属关系与 strong 类似。然而设置方法并不保留新值，而是将其“拷贝” \(copy\)。

* 当属性类型为 NSString 时，经常用此特质来保护其封装性，因为传递给设置方法的新值有可能指向一个 NSMutableString 类的实例。这个类是 NSString 的子类，表示一种可修改其值的字符串，此时若是不拷贝字符串，那么设置完属性之后，字符串的值就可能会在对象不知情的情况下遭人更改。所以，这时就要拷贝一份“不可变” \(immutable\)的字符串，确保对象中的字符串值不会无意间变动。只要实现属性所用的对象是“可变的” \(mutable\)，就应该在设置新属性值时拷贝一份。

* 用 @property 声明 NSString、NSArray、NSDictionary 经常使用 copy 关键字，是因为他们有对应的可变类型：NSMutableString、NSMutableArray、NSMutableDictionary，他们之间可能进行赋值操作，为确保对象中的字符串值不会无意间变动，应该在设置新属性值时拷贝一份。

> ## @synthesize合成实例变量的规则是什么？假如property名为foo，存在一个名为\_foo的实例变量，那么还会自动合成新变量么？

* 实例变量 = 成员变量 ＝ ivar。如果使用了属性的话，那么编译器就会自动编写访问属性所需的方法，此过程叫做“自动合成”\( auto synthesis\)。需要强调的是，这个过程由编译器在编译期执行，所以编辑器里看不到这些“合成方法” \(synthesized method\)的源代码。除了生成方法代码之外，编译器还要自动向类中添加适当类型的实例变量，并且在属性名前面加下划线，以此作为实例变量的名字。上述语法会将生成的实例变量命名为 \_myFirstName 与 \_myLastName ，而不再使用默认的名字。一般情况下无须修改默认的实例变量名，但是如果你不喜欢以下划线来命名实例变量，那么可以用这个办法将其改为自己想要的名字。笔者还是推荐使用默认的命名方案，因为如果所有人都坚持这套方案，那么写出来的代码大家都能看得懂。

* 总结下 @synthesize 合成实例变量的规则，有以下几点：

```
1. 如果指定了成员变量的名称,会生成一个指定的名称的成员变量,
2. 如果这个成员已经存在了就不再生成了.
3. 如果是 @synthesize foo; 还会生成一个名称为foo的成员变量，也就是说：如果没有指定成员变量的名称会自动生成一个属性同名的成员变量。
```

* 在有了自动合成属性实例变量之后，@synthesize还有哪些使用场景？什么情况下不会autosynthesis（自动合成）。当你想手动管理 @property 的所有内容时，你就会尝试通过实现 @property 的所有“存取方法”（the accessor methods）或者使用 @dynamic 来达到这个目的，这时编译器就会认为你打算手动管理 @property，于是编译器就禁用了 autosynthesis（自动合成）。因为有了 autosynthesis（自动合成），大部分开发者已经习惯不去手动定义ivar，而是依赖于 autosynthesis（自动合成），但是一旦你需要使用ivar，而 autosynthesis（自动合成）又失效了，如果不去手动定义ivar，那么你就得借助 @synthesize 来手动合成 ivar。上述语法会将生成的实例变量命名为 \_myFirstName 与 \_myLastName，而不再使用默认的名字。一般情况下无须修改默认的实例变量名，但是如果你不喜欢以下划线来命名实例变量，那么可以用这个办法将其改为自己想要的名字。笔者还是推荐使用默认的命名方案，因为如果所有人都坚持这套方案，那么写出来的代码大家都能看得懂。

```
1. 同时重写了 setter 和 getter 时，这时候要么手动创建 ivar。要么使用@synthesize foo = _foo; ，关联 @property 与 ivar。
2. 重写了只读属性的 getter 时
3. 使用了 @dynamic 时
4. 在 @protocol 中定义的所有属性
5. 在 category 中定义的所有属性
6. 重载的属性 ，意味着当你在子类中重载了父类中的属性，你必须 使用 @synthesize 来手动合成ivar
```

> ## objc中向一个nil对象发送消息将会发生什么？

* 如果一个方法返回值是一个对象，那么发送给nil的消息将返回0\(nil\)。

* 如果方法返回值为指针类型，其指针大小为小于或者等于sizeof\(void\*\)，float，double，long double 或者 long long 的整型标量，发送给 nil 的消息将返回0。

* 如果方法返回值为结构体,发送给 nil 的消息将返回0。结构体中各个字段的值将都是0。

* 如果方法的返回值不是上述提到的几种情况，那么发送给 nil 的消息的返回值将是未定义的。

* 具体原因如下：objc是动态语言，每个方法在运行时会被动态转为消息发送，即：objc\_msgSend\(receiver, selector\)。objc在向一个对象发送消息时，runtime库会根据对象的isa指针找到该对象实际所属的类，然后在该类中的方法列表以及其父类方法列表中寻找方法运行，然后在发送消息的时候，objc\_msgSend方法不会返回值，所谓的返回内容都是具体调用时执行的。那么，回到本题，如果向一个nil对象发送消息，首先在寻找对象的isa指针时就是0地址返回了，所以不会出现任何错误。

> ## 什么时候会报unrecognized selector的异常？

* 当调用该对象上某个方法,而该对象上没有实现这个方法的时候，可以通过“消息转发”进行解决。

* 流程如下。objc是动态语言，每个方法在运行时会被动态转为消息发送，即：objc\_msgSend\(receiver, selector\)。objc在向一个对象发送消息时，runtime库会根据对象的isa指针找到该对象实际所属的类，然后在该类中的方法列表以及其父类方法列表中寻找方法运行，如果，在最顶层的父类中依然找不到相应的方法时，程序在运行时会挂掉并抛出异常unrecognized selector sent to XXX 。但是在这之前，objc的运行时会给出三次拯救程序崩溃的机会：

* Method resolution

objc运行时会调用+resolveInstanceMethod:或者 +resolveClassMethod:，让你有机会提供一个函数实现。如果你添加了函数，那运行时系统就会重新启动一次消息发送的过程，否则 ，运行时就会移到下一步，消息转发（Message Forwarding）。

1. Fast forwarding

   如果目标对象实现了-forwardingTargetForSelector:，Runtime 这时就会调用这个方法，给你把这个消息转发给其他对象的机会。只要这个方法返回的不是nil和self，整个消息发送的过程就会被重启，当然发送的对象会变成你返回的那个对象。否则，就会继续Normal Fowarding。这里叫Fast，只是为了区别下一步的转发机制。因为这一步不会创建任何新的对象，但下一步转发会创建一个NSInvocation对象，所以相对更快点。

2. Normal forwarding

这一步是Runtime最后一次给你挽救的机会。首先它会发送-methodSignatureForSelector:消息获得函数的参数和返回值类型。如果-methodSignatureForSelector:返回nil，Runtime则会发出-doesNotRecognizeSelector:消息，程序这时也就挂掉了。如果返回了一个函数签名，Runtime就会创建一个NSInvocation对象并发送-forwardInvocation:消息给目标对象。为了能更清晰地理解这些方法的作用，git仓库里也给出了一个Demo，名称叫“ \_objc\_msgForward\_demo ”,可运行起来看看。

> ## Objective-C 中对 self 和 super 的理解？

很多人会想当然的认为“ super 和 self 类似，应该是指向父类的指针吧！”。这是很普遍的一个误区。其实 super 是一个 Magic Keyword， 它本质是一个编译器标示符，和 self 是指向的同一个消息接受者！他们两个的不同点在于：super 会告诉编译器，调用 class 这个方法时，要去父类的方法，而不是本类里的。上面的例子不管调用\[self class\]还是\[super class\]，接受消息的对象都是当前 Son ＊xxx 这个对象。当使用 self 调用方法时，会从当前类的方法列表中开始找，如果没有，就从父类中再找；而当使用 super 时，则从父类的方法列表中开始找。然后调用父类的这个方法。这也就是为什么说“不推荐在 init 方法中使用点语法”，如果想访问实例变量 iVar 应该使用下划线（ \_iVar ），而非点语法（ self.iVar ）。点语法（ self.iVar ）的坏处就是子类有可能覆写 setter 。

> ## runtime如何实现weak变量的自动置nil？

runtime 对注册的类， 会进行布局，对于 weak 对象会放入一个 hash 表中。 用 weak 指向的对象内存地址作为 key，当此对象的引用计数为0的时候会 dealloc，假如 weak 指向的对象内存地址是a，那么就会以a为键， 在这个 weak 表中搜索，找到所有以a为键的 weak 对象，从而设置为 nil。

> ## runloop和线程有什么关系？

总的说来，Run loop，正如其名，loop表示某种循环，和run放在一起就表示一直在运行着的循环。实际上，run loop和线程是紧密相连的，可以这样说run loop是为了线程而生，没有线程，它就没有存在的必要。Run loops是线程的基础架构部分， Cocoa 和 CoreFundation 都提供了 run loop 对象方便配置和管理线程的 run loop （以下都以 Cocoa 为例）。每个线程，包括程序的主线程（ main thread ）都有与之相应的 run loop 对象。

> ## objc使用什么机制管理对象内存？

通过 retainCount 的机制来决定对象是否需要释放。每次 runloop 的时候，都会检查对象的 retainCount，如果retainCount 为 0，说明该对象没有地方需要继续使用了。

> ## BAD\_ACCESS在什么情况下出现？

访问了悬垂指针，比如对一个已经释放的对象执行了release、访问已经释放对象的成员变量或者发消息。

> ## 使用block时什么情况会发生引用循环，如何解决？

一个对象中强引用了block，在block中又强引用了该对象，就会发射循环引用。解决方法是将该对象使用weak或者block修饰符修饰之后再在block中使用。

> ## 在block内如何修改block外部变量？

默认情况下，在block中访问的外部变量是复制过去的，即：写操作不对原变量生效。但是你可以加上 \_\_block 来让其写操作生效。我们都知道：Block不允许修改外部变量的值，这里所说的外部变量的值，指的是栈中指针的内存地址。\_\_block 所起到的作用就是只要观察到该变量被 block 所持有，就将“外部变量”在栈中的内存地址放到了堆中。进而在block内部也可以修改外部变量的值。Block不允许修改外部变量的值。Apple这样设计，应该是考虑到了block的特殊性，block也属于“函数”的范畴，变量进入block，实际就是已经改变了作用域。在几个作用域之间进行切换时，如果不加上这样的限制，变量的可维护性将大大降低。又比如我想在block内声明了一个与外部同名的变量，此时是允许呢还是不允许呢？只有加上了这样的限制，这样的情景才能实现。于是栈区变成了红灯区，堆区变成了绿灯区。“定义后”和“block内部”两者的内存地址是一样的，我们都知道 block 内部的变量会被 copy 到堆区，“block内部”打印的是堆地址，因而也就可以知道，“定义后”打印的也是堆的地址。block会对对象类型的指针进行copy，copy到堆中，但并不会改变该指针所指向的堆中的地址，所以在上面的示例代码中，block体内修改的实际是a指向的堆中的内容。上文已经说过：Block不允许修改外部变量的值，这里所说的外部变量的值，指的是栈中指针的内存地址。栈区是红灯区，堆区才是绿灯区。

> ## 使用系统的某些block api，是否也考虑引用循环问题？

系统的某些block api中，UIView的block和OperationWithBlock不需要考虑循环引用的问题。使用 GCD 、NSNotificationCenter就要小心循环引用的问题了。

> ## KVC和KVO的keyPath一定是属性么？

KVC 支持实例变量，KVO 只能手动支持手动设定实例变量的KVO实现监听。

> ## IBOutlet连出来的视图属性为什么可以被设置成weak?

因为既然有外链那么视图在xib或者storyboard中肯定存在，视图已经对它有一个强引用了。不过这个回答漏了个重要知识，使用storyboard（xib不行）创建的vc，会有一个叫\_topLevelObjectsToKeepAliveFromStoryboard的私有数组强引用所有top level的对象，所以这时即便outlet声明成weak也没关系

