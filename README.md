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
