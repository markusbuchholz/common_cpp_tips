# **Common C++ tips**



### Generate a [random](https://en.cppreference.com/w/cpp/numeric/random) number


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
