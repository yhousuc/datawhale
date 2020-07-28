# day3: Future ,promise and async()

std::future 类可以从未来获取数据

fu.get() 只能获取数据一次

```C++
# include <iostream>
# include <thread>
# include <string>
# include <mutex>
# include <fstream>
# include <condition_variable>
# include <future>


using namespace std;

std::mutex mu;
std::condition_variable cond;
//生产者
int factorial(int N) {
    int res = 1;
    for (int i = N; i > 1; i--) {
        res *= i;
    }
    cout << "Result is: " << res << endl;
    cond.notify_one();
    return res;
}


int main() {\
    int x;
//    std::thread t1(factorial, 4, std::ref(x));
//
//    t1.join();
    std::future<int> fu = std::async(factorial, 4);
    x = fu.get();
    cout << "from main: " << x << endl;
    return 0;
}
```

这里async会不会启动子线程：根据

```c++
async(std::launch::deferred, factorial, 4); // 不会启动子线程

async(std::launch::async, factorial, 4);	//会启动子线程

async(std::launch::async |std::launch::deferred , factorial, 4); //是否创建子线程取决于函数内部实现。
```

