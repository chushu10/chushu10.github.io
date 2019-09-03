---
layout: post
title: "一次经典的 C++ 排错过程"
date: 2019-09-03
categories: cplusplus
mathjax: false
---

最近在看 *C++ Primer* 英文第四版，其中有一章节讲到了模板类的成员函数该怎么写，并且给出了一个队列（Queue）的例子，源代码可以[参考这里](https://github.com/Choosue/cpp-primer/tree/master/Chapter-16/inclusion_compilation_model/Queue)。

我使用了 VSCode 插件商城里的 [Create Makefile](https://marketplace.visualstudio.com/items?itemName=zenor.makefile-creator) 插件生成了 Makefile，但是编译的时候却报了如下错误：

```bash
liuchushudeMBP:Queue liuchushu$ make all
g++ -Wall -Werror -Wextra -pedantic -std=c++17 -g -fsanitize=address   -c -o Queue.o Queue.cc
Queue.cc:2:6: error: variable has incomplete type 'void'
void Queue<Type>::destroy()
     ^
Queue.cc:2:11: error: expected ';' at end of declaration
void Queue<Type>::destroy()
          ^
          ;
Queue.cc:2:11: error: expected unqualified-id
Queue.cc:10:17: error: qualified name refers into a specialization of variable template 'Queue'
void Queue<Type>::pop()
     ~~~~~~~~~~~^
Queue.cc:2:6: note: variable template 'Queue' declared here
void Queue<Type>::destroy()
     ^
4 errors generated.
make: *** [Queue.o] Error 1
```

这个报错简直让人云里雾里，百思不得其解。首先，解释一下我在 [Queue.h](https://github.com/Choosue/cpp-primer/blob/master/Chapter-16/inclusion_compilation_model/Queue/Queue.h) 文件里的写法：

```cpp
#ifndef QUEUE_H
#define QUEUE_H

// other code...

#include "Queue.cc"
#endif
```

`#include "Queue.cc"` 这一行的目的是将 `.cc` 文件里的函数定义包含进来，这种写法在 C++ 里被称为 inclusion compilation model，暂且将其翻译为包含编译模式。使用这种写法是为了让 C++ 的模板方法在编译时，编译器既可以看到模板方法的声明又可以看到其定义，同时声明放在 `.h` 文件里、定义放在 `.cc` 文件里，以便于区分。举个例子：

```cpp
// Queue.h
template <class Type> class Queue {
    // other declarations...
    template <class Iter> void copy_elems(Iter, Iter);
    // other declarations...
};

// Queue.cc
template <class Type> template <class Iter>
void Queue<Type>::copy_elems(Iter beg, Iter end)
{
    while (beg != end) {
        push(*beg);
        ++beg;
    }
}
```

在上面的代码片段中，头文件 `Queue.h` 中的 `Queue` 类中声明了模板函数 `copy_elems`，而其定义则写在 `Queue.cc` 里，我们通过在 `Queue.h` 里包含 `Queue.cc` 文件，就可以让编译器同时看到模板函数 `copy_elems` 的声明和定义。

可为什么会有上面那个莫名其妙的错误呢？我尝试着将 `#include "Queue.cc"` 这一行注释掉，还是有同样的问题！

```bash
liuchushudeMBP:Queue liuchushu$ make all
g++ -Wall -Werror -Wextra -pedantic -std=c++17 -g -fsanitize=address   -c -o Queue.o Queue.cc
Queue.cc:2:6: error: variable has incomplete type 'void'
void Queue<Type>::destroy()
     ^
Queue.cc:2:11: error: expected ';' at end of declaration
void Queue<Type>::destroy()
# 省略其他报错
```

等等！我已经注释掉 `#include "Queue.cc"` 了，按理说不应该再报这个文件的错误了啊。哦！原来是我的 Makefile 里编译了这个 `.cc` 文件：

```makefile
CXX = g++
CXXFLAGS = -Wall -Werror -Wextra -pedantic -std=c++17 -g -fsanitize=address
LDFLAGS =  -fsanitize=address

SRC = Queue.cc program.cc
OBJ = $(SRC:.cc=.o)
EXEC = program

all: $(EXEC)

$(EXEC): $(OBJ)
	$(CXX) $(LDFLAGS) -o $@ $(OBJ) $(LBLIBS)

clean:
	rm -rf $(OBJ) $(EXEC)
```

一旦编译这个 `.cc` 文件，而它有没有包含 `Queue.h` 文件，那么就找不到 `Queue` 的定义，就会产生上述报错。我们无需编译 `Queue.cc` 文件，因为 `Queue.h` 文件就已经包含了 `.cc` 文件里的函数定义了。

最后，在 Makefile 里编译 `Queue.cc` 的编译就可以正常编译了：

```makefile
SRC = program.cc
```
