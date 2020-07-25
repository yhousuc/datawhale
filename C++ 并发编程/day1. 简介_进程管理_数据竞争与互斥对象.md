# day1. C++ 并发编程 

**进程间通信的方式**：信号、套接字、文件、管道 

缺点：设置复杂，速度慢，资源开销大，管理进程

优点：os的保护机制，安全， 可以远程连接



**线程**：轻量型进程,所有线程共享地址空间，共享资源

代价：资源管理、不能分布式



**并发：**

- 优势：关注点分离、性能
- 关注点分离：相关代码和无关代码分离，代码的可读性增强





**C++ 多线程历史**:

- C++ 98 使用的是 平台相关的 C 语言API来处理多线程
- C++ 11 
  - 线程感知内存模型
  - 管理线程类
  - 保护共享数据类
  - 线程间同步操作类
  - 低层原子操作类



**代码**：

```c++
# include <iostream>
# include <thread>

using namespace std;
void hello()
{
    cout << "Hello Concurrent World"<< endl;
}
int main()
{
    thread t(hello);
    t.detach();
    if(t.joinable())	//判断线程是否可以join
    {
        t.join();
    }
    return 0;
}

```

- 线程启动
  - thread t(hello);
- 线程加入的方式
  - join() 主线程等待子线程结束后，继续运行
  - detach() 主线程不等待，且线程 detach()后将无法join()。 

#### 针对下一个案列

```C++
# include <iostream>
# include <thread>

using namespace std;
void hello()
{
    cout << "Hello Concurrent World"<< endl;
}
int main()
{
    thread t(hello);
    for(int i = 0; i < 10; ++i)
    {
        cout << "From main: " << i << endl;
    }
    //t.join();
    if(t.joinable())
    {
        t.join();
    }
    return 0;
}
```

主线程中使用了for循环，假如在循环中出现了异常，则会影响 后续的 t.join，那么可以使用下面的方式

```C++
# include <iostream>
# include <thread>

using namespace std;
void hello()
{
    cout << "Hello Concurrent World"<< endl;
}
int main()
{
    thread t(hello);
    try{//使用try-catch 异常处理
        for(int i = 0; i < 10; ++i)
        {
            cout << "From main: " << i << endl;
        }
    }
    catch (...) {
        t.join();
        throw;
    }
    //t.join();
    if(t.joinable())
    {
        t.join();
    }
    return 0;
}
```

#### 可以通过类来构造线程对象

```C++
# include <iostream>
# include <thread>

using namespace std;
void hello()
{
    cout << "Hello Concurrent World"<< endl;
}

class Fctor{
public:
    void operator()(){
        for(int i = 0; i > -100; i--)
        {
            cout << "from t1: " << i << endl;
        }
    }
};
int main()
{
    //Fctor fct;
    //std::thread t((Fctor()));
    std::thread t{Fctor()};
    try{
        for(int i = 0; i < 100; ++i)
        {
            cout << "From main: " << i << endl;
        }
    }
    catch (...) {
        t.join();
        throw;
    }
    //t.join();
    if(t.joinable())
    {
        t.join();
    }
    return 0;
}
```

#### 类中添加参数

```C++
# include <iostream>
# include <thread>
# include <string>
using namespace std;
void hello()
{
    cout << "Hello Concurrent World"<< endl;
}

class Fctor{
public:
    void operator()(std::string msg){
        for(int i = 0; i > -100; i--)
        {
            cout << "from t1: " << i << " " << msg << endl;
        }
    }
};
int main()
{
    //Fctor fct;
    //std::thread t((Fctor()));
    std::string s = "I Love U";
    std::thread t{Fctor(), s};
    try{
        for(int i = 0; i < 100; ++i)
        {
            cout << "From main: " << i << endl;
        }
    }
    catch (...) {
        t.join();
        throw;
    }
    //t.join();
    if(t.joinable())
    {
        t.join();
    }
    return 0;
}
```

- 引用是地址操作，可以避免很多赋值操作
- 值传递代码

```C++
# include <iostream>
# include <thread>
# include <string>
using namespace std;
void hello()
{
    cout << "Hello Concurrent World"<< endl;
}

class Fctor{
public:
    void operator()(std::string msg){
        cout << "from t1: " << msg << endl;
        msg = "I dont like u";
    }
};
int main()
{
    //Fctor fct;
    std::string s = "I Love U";
    //std::thread t((Fctor()),std::ref(s));
    std::thread t((Fctor()),s); //如果值传递，在这里会复制s作为线程t1的参数，但是引用传递，是传递指针
    //std::string s = "I Love U";
    //std::thread t{Fctor(), s};

    t.join();
    std::cout << "from main: " << s << endl;
    return 0;
}
```

![image-20200725222328395](C:\Users\13398\AppData\Roaming\Typora\typora-user-images\image-20200725222328395.png)

- 引用传递

```C++
# include <iostream>
# include <thread>
# include <string>
using namespace std;
void hello()
{
    cout << "Hello Concurrent World"<< endl;
}

class Fctor{
public:
    void operator()(std::string msg){
        cout << "from t1: " << msg << endl;
        msg = "I dont like u";
    }
};
int main()
{
    //Fctor fct;
    std::string s = "I Love U";
    std::thread t((Fctor()),std::ref(s));
    //std::thread t((Fctor()),s);
    //std::string s = "I Love U";
    //std::thread t{Fctor(), s};

    t.join();
    std::cout << "from main: " << s << endl;
    return 0;
}
```

![image-20200725222757319](C:\Users\13398\AppData\Roaming\Typora\typora-user-images\image-20200725222757319.png)

对于某些对象不能进行赋值（如线程对象）只能用move

```C++
# include <iostream>
# include <thread>
# include <string>
using namespace std;
void hello()
{
    cout << "Hello Concurrent World"<< endl;
}

class Fctor{
public:
    void operator()(std::string& msg){
        cout << "from t1: " << msg << endl;
        msg = "I dont like u";
    }
};
int main()
{
    //Fctor fct;
    std::string s = "I Love U";
    std::thread t((Fctor()),std::ref(s));
    std::thread t2 = std::move(t);
    //std::string s = "I Love U";
    //std::thread t{Fctor(), s};

    t2.join();
    std::cout << "from main: " << s << endl;
    return 0;
}
```

- 通过 get_id()来获取线程号

  ```C++
      std::cout << std::this_thread::get_id() << endl;
      std::thread t((Fctor()),std::ref(s));
      std::thread t2 = std::move(t);
      std::cout << t2.get_id() << endl;
  ```

- 查看能开的线程最大个数

  ```C++
  cout << std::thread::hardware_concurrency() << endl;
  ```

  

#### 数据竞争和互斥对象

​	在之前的代码中我们曾用两个线程（主线程、 新开线程t）来输出数据，但是数据结果十分杂乱，为什么？

- 由于这两个线程在竞争 COUT

- 解决数据竞争的方法就是使用互斥对象来同步资源的访问

  ```C++
  # include <iostream>
  # include <thread>
  # include <string>
  # include <mutex>
  
  std::mutex mu;
  
  void shared_print(std::string msg, int id)
  {
      //mu.lock(); 不介意使用lock() unlock()， 由于下一行代码发生异常时，会导致整个锁被锁住
      std::lock_guard<std::mutex> guard(mu);
      std::cout << msg << id << std::endl;
      //mu.unlock();
  }
  void function_1(){
      for(int i = 0; i > -100; i-- )
          shared_print("from t1: ", i);
  }
  
  int main()
  {
  
      std::thread t1(function_1);
      for(int i = 0; i < 100; i++)
          shared_print("from main: ", i);
      t1.join();
      return 0;
  }
  ```

  #### 更好的保证线程的安全方式，将锁私有化

  ```C++
  # include <iostream>
  # include <thread>
  # include <string>
  # include <mutex>
  # include <fstream>
  
  class LofFile{
  public:
      LofFile(){
          f.open("log.txt");
      }
      void shared_print(std::string id, int value)
      {
  
          std::lock_guard<std::mutex> locker(m_mutex);
          f << "From" << id << ": " << value << std::endl;
      }
  private:
      std::mutex m_mutex;
      std::ofstream f;
  };
  
  
  void function_1(LofFile& log){
      for(int i = 0; i > -100; i-- )
          log.shared_print("from t1: ", i);
  }
  
  int main()
  {
      LofFile log;
      std::thread t1(function_1, std::ref(log));
      for(int i = 0; i < 100; i++)
          log.shared_print("from main: ", i);
      t1.join();
      return 0;
  }
  ```

  

