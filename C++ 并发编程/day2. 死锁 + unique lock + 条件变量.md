## day2. 死锁 + unique lock + 条件变量

t1线程  与 主线程 格子拥有一个互斥对象，还在等待其他的互斥对象。

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
        std::lock_guard<std::mutex> locker2(m_mutex2);
        std::cout << "From" << id << ": " << value << std::endl;
    }
    void shared_print2(std::string id, int value)
    {

        std::lock_guard<std::mutex> locker2(m_mutex2);
        std::lock_guard<std::mutex> locker(m_mutex);
        std::cout << "From" << id << ": " << value << std::endl;
    }
private:
    std::mutex m_mutex;
    std::mutex m_mutex2;
    std::ofstream f;
};


void function_1(LofFile& log){
    for(int i = 0; i > -100; i-- )
        log.shared_print(" t1: ", i);
}

int main()
{
    LofFile log;
    std::thread t1(function_1, std::ref(log));
    for(int i = 0; i < 100; i++)
        log.shared_print2(" main: ", i);
    t1.join();
    return 0;
}
```

如何避免死锁：

1、保证 lock 的顺序相同。

2、 std::lock()

```C++
    void shared_print(std::string id, int value)
    {
        std::lock(m_mutex, m_mutex2);
        std::lock_guard<std::mutex> locker(m_mutex, std::adopt_lock);
        std::lock_guard<std::mutex> locker2(m_mutex2, std::adopt_lock);
        std::cout << "From" << id << ": " << value << std::endl;
    }
    void shared_print2(std::string id, int value)
    {
        std::lock(m_mutex, m_mutex2);
        std::lock_guard<std::mutex> locker2(m_mutex2, std::adopt_lock);
        std::lock_guard<std::mutex> locker(m_mutex, std::adopt_lock);
        std::cout << "From" << id << ": " << value << std::endl;
    }
```





## Unique Lock 和 lazy initialization

unique lock 为专门的语句添加指定的锁，使得锁更有弹性控制

```C++
void shared_print(std::string id, int value)
{
    std::unique_lock<std::mutex> locker(m_mutex, std::defer_lock);  //提供更多的弹性
    locker.lock(); //为专门的语句添加指定的锁
    std::cout << "From" << id << ": " << value << std::endl;
    locker.unlock();
    //...
}
```

unique_lock 可以移动 // 更消耗性能

lock_guard 不可以移动

两者都不可以复制

```C++
std::unique_lock<std::mutex> locker2 = std::move(locker); //转移锁
```





```C++
    void shared_print(std::string id, int value)
    {
//        {
//            std::unique_lock<std::mutex> locker(m_mutex_open, std::defer_lock);//对open操作绑定了一个locker
//            if(!f.is_open())
//            {
//                f.open("log.txt");
//            }
//        }
        std::call_once(m_flag, [&](){f.open("log.txt")});
        std::unique_lock<std::mutex> locker(m_mutex, std::defer_lock);  //提供更多的弹性
        std::cout << "From" << id << ": " << value << std::endl;
    }
private:
    std::once_flag m_flag;
    std::mutex m_mutex;
    std::mutex m_mutex_open;
    std::ofstream f;
};
```

## 条件变量

经典的生产者和消费者模型

```C++
//生产者
void function_1(){
    int count = 10;
    while( count > 0)
    {
        std::unique_lock<mutex> locker(mu);
        q.push_front(count);
        locker.unlock();
        std::this_thread::sleep_for(chrono::seconds(1));
        count--;
    }
}

//消费者
void function_2(){
    int data = 0;
    while(data != 1)
    {
        std::unique_lock<mutex> locker(mu);
        if(!q.empty())
        {
            data = q.back();
            q.pop_back();
            locker.unlock();
            cout << "t2 got a value from t1: " << data << endl;
        }
        else{
            locker.unlock();
            //std::this_thread::sleep_for(chrono::milliseconds (10));
        }
    }
}
```

在上述代码中消费者通过无限循环的方式，导致效率很低，我们当判断队列为空的时候，就可以

使用睡眠函数来将其效率增高



### 条件变量登场

条件变量中，加入了cond.notify_one(); 可以通知其他线程开始运行

```C++
# include <iostream>
# include <thread>
# include <string>
# include <mutex>
# include <fstream>
# include <functional>
# include <deque>
# include <condition_variable>

using namespace std;

std::deque<int> q;
std::mutex mu;
std::condition_variable cond;

//生产者
void function_1() {
    int count = 10;
    while (count > 0) {
        std::unique_lock<mutex> locker(mu);
        q.push_front(count);
        locker.unlock();
        cond.notify_one();
        //cond.notify_all(); //激活所有的线程
        std::this_thread::sleep_for(chrono::seconds(1));
        count--;
    }
}

//消费者
void function_2() {
    int data = 0;
    while (data != 1) {
        std::unique_lock<mutex> locker(mu);
        cond.wait(locker, [](){return !q.empty();});
        data = q.back();
        q.pop_back();
        locker.unlock();
        cout << "t2 got a value from t1: " << data << endl;
    }
}
```