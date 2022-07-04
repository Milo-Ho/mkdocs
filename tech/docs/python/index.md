1、【命名空间】

（函数内）局部变量找不到的时候，会向外（函数外）找；

写操作会屏蔽“向外寻找变量”的操作，让变量只在内部寻找，并且这是在编译的时候决定的；

2、【内存回收】

引用计数为0时，立即回收；

循环引用的危害（并非无法回收循环引用的对象）：无法立即回收，会延迟到gc时才会收；增加gc的频率；降低gc的效率；内存占用率波动大；

解决办法：弱引用；不要使用method做回调；

3、unbound method 和 bound method

A.__dict__["foo"] 是 function；

A.foo 是 unbound method；

a.foo 是 bound method；

A.foo.im_self is None

A.foo.im_func is a.foo.im_func is A.__dict__["foo"]

A.foo.im_class is a.foo.im_class is A

"im_"是"instance_method_"的缩写；

unbound method 和 bound method 几乎都是一次性的临时对象；

4、classmethod 是 bound method，它的im_self是类本身

5、staticmethod 是 function

6、int str这一类built-in的类型，+操作更快；对于自定义类型，__add__操作更快

7、property是class

8、super是一个built-in class

通过__mro__实现

__mro__即是method resolution order 

C++在编译时确定需要调用的函数；

Python在运行时确定；

多继承的时候mro的顺序是拓扑排序；

9、字符串拼接

最快：join，不产生临时的新的str对象

"%s"格式化字符串需要分析格式，也不够快

10、

减少在模块内import其他模块（启动慢）

减少函数调用层数（函数调用也有开销）

减少属性访问（bound method是一个新的临时对象，可以cache下来）

11、为什么使用脚本语言

hotfix 方便热更

可拓展性 可读性 可维护性 接近自然语言

crash 不会闪退

安全性 python修改数值，是改变了引用关系（hp引用的int对象改变了），修改的是引用的地址，而非修改地址上的值

12、空的tuple是单例

13、垃圾回收

衡量标准：运行时间、卡顿、空间的利用（减少内存碎片）、空间和时间的局部性


* [six.py](https://www.jianshu.com/p/62a6e3f2d1ca)
* type.\_\_new__
* super
* \_\_mro_entries__
* metaclass
* property
* tp_getset / tp_descr_get / tp_descr_set
* tp_getattr / tp_setattr / tp_getattro / tp_setattro
* 哈希冲突
* 运行时的实例变量存在哪里？
* 内存管理
    * GC / 内存池 / slot / 弱引用
* hotfix
    * find_module / load_module (sys.meta_path / sys.path_hooks ) ihooks.ModuleLoader
    * import / package / reload
* 协程
    * async await asyncio loop task
