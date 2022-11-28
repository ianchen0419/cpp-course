# Section 11: Functions

## C++的Function

C++標準函示庫總覽：https://en.cppreference.com/w/cpp/header

應用練習：隨機數產生器


```cpp
#include <iostream>
#include <cmath>
#include <ctime>

using namespace std;


int main() {
    
    int random_number {};
    size_t count {10}; // 指定隨機數的數量為10
    int min {1};
    int max {6};
    
    // rand()會隨機從0到RAND_MAX之間產生一個數字，RAND_MAX在每台機器的輸出都不一樣
    cout << RAND_MAX << endl;
    
    // srand是seed rand的意思，使用系統時間播種隨機數
    srand(time_t(nullptr));
    
    for(size_t i {1}; i < count; i ++) {
        random_number = rand() % max + min;
        cout << random_number << endl;
    }
    
    cout << endl;
    
    return 0;
}
```

`rand()`函數是
