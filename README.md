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
