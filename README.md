# **Common C++ tips**



### 1. Generate a [random](https://en.cppreference.com/w/cpp/numeric/random) number


```cpp
#include <random>

float generateNormalRandom()
{
    // Static instances are initialized only once
    static std::mt19937 gen{std::random_device{}()};
    static std::normal_distribution<float> distrib(0.0f, 1.0f);

    return distrib(gen);
}

```

###  2. [Parallel](https://en.cppreference.com/w/cpp/algorithm/execution_policy_tag_t) execution 

We use [Threading Building Blocks](https://www.intel.com/content/www/us/en/developer/tools/oneapi/onetbb.html#gs.lsmvlt) from Intel.


```bash
//install in Linux
sudo apt install libtbb-dev
```

```cpp
#include <iostream>
#include <vector>
#include <execution> // this header has to be added
#include <algorithm>

int main()
{

    std::vector<double> v(1 << 30, 0.5);
    auto f = std::find(std::execution::par, v.begin(), v.end(), 0.6);
    // we use std::execution::par to execute algorithm in pararrel
    // in order to compare to sequential method run:
    // auto f = std::find(v.begin(), v.end(), 0.6);
}

```

```bash
//you have to add flag: -ltbb
g++ parallel_computation.cpp --std=c++2a -ltbb -o pcomp

time ./pcomp
```

### 3. [Optimization](https://gcc.gnu.org/onlinedocs/gcc-11.1.0/gcc/Optimize-Options.html)

Adding the optimization flag -O1, -O2, or -O3 you can force the compiler to optimize your code to a certain level. We can imagine that in this case optimization is the process of translating or mapping the higher-level software (C++) to the code which can be run as fast as possible on the hardware. Please note that optimization is the resource greedy process (it takes CPU resources, and memory and extends the compilation time).

```bash
// for the previous code in tips 2.
//you have to add flag: -ltbb
g++ parallel_computation.cpp --std=c++2a -ltbb -O3 -o pcomp

```

### 4. Parallel Computation. [Atomic operations](https://en.cppreference.com/w/cpp/atomic/atomic)

Atomic operations play a significant role when our application uses threads and threads access the same part of memory.
Threads are not synchronized and can access the same location of memory at the same time. For instance, we can imagine, that two threads want to update a certain part of memory (value += 1). It can happen that at the same time one thread will write the value but the other read it (both threads operate on the same memory).
The atomic operation solves the problem, where (+=) — read/write operation or other ([link](https://en.cppreference.com/w/cpp/atomic/atomic)) is considered as a single operation. The application reads and writes as one instruction.


```cpp
int M; 

Eigen::Matrix<int, 3, 1> M_1 = Eigen::Matrix<int, 3, 1>::Ones();

auto work = []()
{
    for (auto ii = 0; ii < 10; ii++) // 10-OK //100-NO
    {
        M += M_1.dot(M_1);
    }
};

```

In this atomic example, the result is always 6000.

```cpp
std::atomic<int> M; //only one diffrence

Eigen::Matrix<int, 3, 1> M_1 = Eigen::Matrix<int, 3, 1>::Ones();

auto work = []()
{
    for (auto ii = 0; ii < 10; ii++) // 10-OK //100-NO
    {
        M += M_1.dot(M_1);
    }
};

```
















