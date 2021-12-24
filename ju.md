

# C++ Practice

------



# QUESTION 3

1. ## **What is the meaning of Template Class?**

   Template class, is a Template for classes. C++ provides us with a way where we can create a class that will serve as a blueprint/template for future classes. A template class will have generic variables and methods of type “T”, which can later be customized to be used with different data types as per the requirement.

   这里老师问的应该是 C++模板类的意思是什么

   首先需要了解 模板类 是什么，其实有个平级的概念叫做 模板函数 ，他们都是C++模板的基础，注意哦，这个模板在C++里面是核心，但是它理解起来不难，它是落实了面向对象编程的一种设计理念，用来简化代码量的，当然也是很容易学会的

   这里有`runoob`的一篇文章，做这个题目之前一定要先去仔细读一下这篇文章：https://www.runoob.com/cplusplus/cpp-templates.html

   读完过后相信你对模板函数和模板类已经有了一定的了解，但是可能对它具体的应用场景还是很模糊，事实上我们写的代码大量使用了模板的概念，

   比如我们可能用过`std::list<int>`来使用一个链表存储`int`类型的数据

   有没有想过这个`std::list<int>`中的`int`为什么要用尖括号包裹起来呢？答案就是因为这里的list就是一个模板类，因为我们这个list类是从C++ STL库中直接调用的，我们如果点进去看它的具体实现的话，会发现这样一段代码

   ```c++
   template<typename _Tp, typename _Alloc = std::allocator<_Tp> >
      class list : protected _List_base<_Tp, _Alloc>
       {
   	//...后面的省略了，因为充斥了大量宏声明，看起来很乱。
   ```

   可以很明显看见，名称为list的这个类是一个template类，因为它有一个template关键词，然后紧接着有一个尖括号`<typename _Tp, typename _Alloc = std::allocator<_Tp> >`,这说明了这个模板类它的创建也需要传入一个尖括号，这个尖括号里接收两个参数，第一个参数就代表我们这个链表里面存放的数据类型，也许是int或者string，当然这由你决定，第二个参数可以缺省，因为她有一个默认值是std::allocator<_Tp>，这个我们就不用管他

   其实上面的属于拓展了解，但是有个东西我们一定要牢记，就是模板类该怎么去定义，又怎么去根据一个模板类创建对象呢？

   ```c++
   template <class type> class class-name {
   	//...这里省略成员函数和成员变量
   }
   
   //其实我们看很多库里面，它把class这个关键词提行写了，效果和上面其实是一模一样的，一定要辨别出来哦
   template <class type> 
   class class-name {
   	//...这里省略成员函数和成员变量
   }
   //还有一种情况就是它是这样声明一个模板类的
   template <typename type> 
   class class-name {
   	//...这里省略成员函数和成员变量
   }
   
   //看出来它和上面的不同了吗，没错就是在尖括号里面class和typename的区别，我们不去深究这两个关键词的意思，因为它们几乎是一模一样的意思
   //区别在这里：https://liam.page/2018/03/16/keywords-typename-and-class-in-Cxx/
   //不过我们可以初略理解为：class就是typename，无论它代码怎么写他们都是一样的东西！
   ```

   当我们有了一个模板类的声明过后，我们怎么去根据这个模板类来创建对象呢？来看看下面这段代码

   ```c++
   template <typename MI> class Dog{
   public:
       int age;
       int getMyDogAge(){
   	  return age;
       }
       MI keyword;
    	MI getDogKeyword(){
           return keyword;
       }
   }
   
   //我们创建一个Dog类
   Dog<string> dog1;
   //再调用dog1.getDogKeyword()就可以返回string类型的keyword啦，尽管这里还没有赋值(
   Dog<int> dog2;
   //再调用dog2.getDogKeyword()就可以返回int类型的keyword啦，尽管这里还没有赋值(
   ```

2. ## **What is the purpose of using Template Class?**

   ```c++
   template <typename T> class AddNum {
   public:
   	void setMember(T newValue);
   protected:
   	T member;
   };
   ```

   Improved code reusability

   模板类的意义就在于提高了代码的复用性，我们可以看看上面Dog的那个例子，如果不采用模板类，可能我们就需要提前声明两个类了

   ```c++
   class Dog_KWD_STR{//关键词是string类型的Dog
   public:
       int age;
       int getMyDogAge(){
   	  return age;
       }
       string keyword;
       string getDogKeyword(){
           return keyword;
       }
   }
   
   class Dog_KWD_INT{//关键词是INT类型的Dog
   public:
       int age;
       int getMyDogAge(){
   	  return age;
       }
       string keyword;
       string getDogKeyword(){
           return keyword;
       }
   }
   //我们创建一个Dog_KWD_STR类
   Dog_KWD_STR dog1;
   //再调用dog1.getDogKeyword()就可以返回string类型的keyword啦，尽管这里还没有赋值(
   Dog_KWD_INT dog2;
   //再调用dog2.getDogKeyword()就可以返回int类型的keyword啦，尽管这里还没有赋值(
   ```

3. ## **How to define the template method `setMember`() ?**

   注意看第二题的题干，第二题给的题干内容应该是出现在头文件的(.h文件)，我们在`.cpp`文件写方法的具体代码

   答案就是下面这个

   ```c++
   template <typename T> void setMember(T newValue){
       this->member = newValue;
   }
   ```

   

4. ## **And please explain every part of definition.**

   这个题是承接第3题的，从左到右依次解释每个名词：

   `template`表明这是模板类 中 的一个函数

   `<typename T>`表示这个这个函数的泛型变量取名为`T`

   `void`表示这个函数的返回值是空类型void

   `setMember`表示这个函数的名称，由题干中的声明中可以得到，这就是我们需要具体完善的函数

   `(T newValue)`表示这个函数接收的类型是泛型T的变量，因为它是一个泛型变量，所以它有无限种可能，它在具体应用中可能是一个int,可能是string,可能是char，也有可能是一个vector,等等...

   `this->member = newValue`表示把形参`newValue`的值赋给成员属性member,注意这个member是个泛型为T的成员属性，所以他和`newValue`都是同一种类型

   

   

   

# QUESTION 4

1. ## **How to express a string by using C language?**

   在 C 编程中，字符串是以空字符结尾的字符序列`\0`。例如：

   ```C
   char c[] = "c string";
   ```

   当编译器遇到双引号括起来的字符序列时，它`\0`默认在末尾附加一个空字符

   ![C编程中字符串的内存图](https://cdn.programiz.com/sites/tutorial2program/files/c-string.jpg)

   如何声明一个字符串？

   以下是声明字符串的方法：

   ```c
   char s[5];
   ```

   ![C 编程中的字符串声明](https://cdn.programiz.com/sites/tutorial2program/files/c-string-declaration_0.jpg)

   在这里，我们声明了一个 5 个字符的字符串。

   **如何初始化字符串？**

   这个应该是回答的重点

   ```c
   char c[] = "abcd";
   
   char c[50] = "abcd";
   
   char c[] = {'a', 'b', 'c', 'd', '\0'};
   
   char c[5] = {'a', 'b', 'c', 'd', '\0'};
   ```

   ![C 编程中字符串的初始化](https://cdn.programiz.com/sites/tutorial2program/files/c-string-initialization.jpg)

2. ## Please write a function `reverseName` which returns a string in reverse order of original one.

   ```c
   char* reverseName(char *s) {
       // 获取字符串长度
       int len = 0;
       char *p = s;
       while (*p != 0) {
           len++;
           p++;
       }
       //复制一份输入的字符串，因为题目要求我们不能直接对原字符串进行翻转
       char *s_copyed = new char[len];
       memcpy(s_copyed,s,len);
   
       // 交换 ...
       int i = 0;
       char c;
       while (i <= len / 2 - 1) {
           c = *(s_copyed + i);
           *(s_copyed + i) = *(s_copyed + len - 1 - i);
           *(s_copyed + len - 1 - i) = c;
           i++;
       }
       return s_copyed;
   }
   ```

   

   

# QUESTION 5

```c++
class Cubeside{
public:
	virtual char * getName()=0;
}
Class DialSide: public Cubeside {
public:
	char *getName() override;
	void setDial(uint16_t value);
protected:
	uint16_t dialvalue =0;
	static char const * dialName="xxyy";
};
```

1. ## There are two "= o" in these codes, what is the meaning of them each?

   - 首先讲一下下面这个`uint16_t dialvalue =0;`吧，这个非常简单，它就是一个protected访问限制的普通成员属性，这里用了`=0`只是为了给它初始化赋值为0，这个uint16_t就是typedef起的别名，它的定义在库函数中(`typedef unsigned short  uint16_t;`)，我们就无需过多思考啦,但是我们要知道它是一个unsigned short，为两个字节哟！！

   - 然后就是`virtual char * getName()=0;`这个地方用到了"=0"，这里的意思是声明了一个纯虚函数！纯虚函数！纯虚函数.....
   
   呜呜呜虚函数我猜你也不知道，不过这个真不怪你，虚函数是和多态这个概念紧密联系的，在Java里面每个类的成员函数都是虚函数，所以对这个虚函数的理解就比较容易，C++里面的虚函数就需要`virtual`这个关键词来声明，具体什么情景下使用需要狠狠阅读菜鸟教程，可能和模板类一样的有一点点抽象，但是一定要耐心阅读，因为它们在面向对象编程里面都是非常非常非常重要的概念。
   
   [C++多态(其实就是讲虚函数)](https://www.runoob.com/cplusplus/cpp-polymorphism.html)
   
   然后就是纯虚函数的概念，一定要去感觉一下
   
   [C++纯虚函数](https://www.runoob.com/w3cnote/cpp-virtual-functions.html)
   
   我觉得有几点关于虚函数的一定一定一定要强调一下
   
   **虚函数只能借助于指针或者引用来达到多态的效果。而且虚函数的特性想要体现出来，一定会存在类的继承**
   
   - 定义一个函数为虚函数，不代表函数为不被实现的函数。
   
   - 定义他为虚函数是为了允许用基类的指针来调用子类的这个函数。
   
   - 定义一个函数为纯虚函数，才代表函数没有被实现。
   
   - 定义纯虚函数是为了实现一个接口，起到一个规范的作用，规范继承这个类的程序员必须实现这个函数。

   

2. ## Then define two pointer in main() program:

   ```c++
   Cubeside *c1 =new Cubeside ();
   Cubeside *c2 = new DialSide ();
   ```
   
   ## what is the effect of following codes:
   
   ```c++
   cl->getName();
   c2->getName();
   ```
   
   这里老师让解释这两行代码的效果，注意看`getName`函数，这是一个虚函数，因为这个函数在基类(父类)Cubeside类中被声明为virtual了
   
   c1 是基类(父类)/Cubeside类 的指针,当我们通过这个指针去调用getName()方法时，会去虚函数表中寻找getName函数的真实地址，因为这个指针指向的对象是Cubeside类的对象，那么调用的方法也会是 Cubeside类中的getName方法，然后当我们调用的时候会发现报错，为什么呢？因为这是一个纯虚函数，纯虚函数是没有函数内容的，所以这个函数调用会报错！
   
   c2 也是基类Cubeside类的指针，`Cubeside *c2 = new DialSide ();`尽管等号右边返回值应该是一个(DialSide对象的指针)，这里应该是有一个隐式转换，把(DialSide* 偷偷转换 成了 Cubeside*)，那么这样会出现什么效果呢？我们前面说了，虚函数的特性想要展现出来必须通过指针，这里指针类型虽然是Cubeside，但是指向的内存是DialSide类的对象，当我们调用它的getName方法时，会去虚函数表中寻找getName函数的真实地址，由于虚函数的特性是**允许用基类的指针来调用子类的这个函数**，所以这个时候会调用子类即DialSide类中重写了的getName函数，根据题目已知信息是无法获得结果的(题干中看不到getName函数的具体实现，但是执行`c2->getName();`调用的一定会是子类(DialSide类)中重写(override)了的getName函数
# QUESTION 6

1. ## why we usually divide a class codes into .h and .cpp files?

   Improve the readability of the function interface, facilitate the expansion of the software, and improve the maintainability

   提高了函数接口的可读性，有利于软件的扩展，提高了可维护性

   

   这个可能还很难感觉到它的优势，因为除了C、C++这些语言，都没有把声明(.h)和实现(.cpp)分开写，但是老师这里还让我们解释为什么分开写，其实C++不使用.h(不分开写)一样的能够实现程序功能。只是那样会导致不是很美观,然后链接时会出现一些小问题，反正大家都在分开写我们也分开写吧！考试考到了就写上面的答案

2. ## What command we should use to create one shared library file in a CLion project?

   这里老师的意思是如何编译创建一个动态库文件，这里又涉及到了一个动态库的概念了，我们以前在Clion中写的程序是通过CMake直接编译成了可执行文件(Windows 下文件后缀名为.exe),但是动态库文件的后缀名是(在Windows下是.dll)，怎么办呢？这时候我们需要修改项目中的CMakeLists.txt文件，Clion左边的目录树可以直接看到这个文件

   ```cmake
   cmake_minimum_required(VERSION 3.21)
   project(pig)
   
   set(CMAKE_CXX_STANDARD 14)
   
   # 这行不要=>add_executable(myPigApp main.cpp)
   
   #Cmake配置文件里面这样写，注意()内的三个参数
   #第一个代表动态库的名称,不用写后缀名(如Windows下的.dll，Linux下的.so)
   #第二个值是(SHARED,STATIC,MODULE,EXCLUDE_FROM_ALL)里面的一个，题目要求我们是创建共享库，所以我们就填SHARED就行啦
   #第三个值是被编译的文件，如果有多个文件则需要用逗号分隔
   add_library(myPigShared SHARED main.cpp,main.h)
   ```

   按照上面的配置，我们再通过Clion->Build->Build Project即可创建(编译)一个名为myPigShared.dll的文件在当前Project的`cmake-build-debug`目录下，当然如果我们是Linux的系统编译出来的文件名称可能是myPigShared.so，这个不用考虑太多

   

3. ## What command we should use to create one executable file in a CLion project?

4. ## What command we should use to link the library file with executable file?

5. ## Try to use some codes to demonstrate these commands and explain every part of the command line.

# QUESTION 7

```c++
typedef struct{
	char name[10];
	char gender[6];
}student;
```

1. ## Use strcpy method to copy a 5 characters long string into the name field, what will happen?
2. ## Copy a 10 characters long string, what will happen?
3. ## Copy a 11 characters long string, what will happen?
4. ## Copy a 20 characters long string, what will happen?
5. ## Try to use a sample to explain ?

# QUESTION 8

**Your program performs the operation :**

```c++
int x;
std::ifstream fileStream("test.txt");
fileStream >> x;
```

1. ## What will happen if the value actually in the file is not a number,and how can your program recover?
