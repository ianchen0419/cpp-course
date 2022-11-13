# Section 7: Arrays and Vectors

## Array

C++的Array具有以下特性：
* 固定大小：一但建立了Array，大小就被固定了，如果需要更多空間，則需要修改原始碼，改變最大大小
* 在記體體中連續儲存：比如宣告了一個陣列，包含100個數字，C++會從記憶體中準確撥出400 bytes的空間，然後分成100份，因此在記體體中，這100個數字都是連續性的存放
* 不進行邊界檢查：比如宣告了一個陣列，內含100個數字，如果呼叫了`myArray[500]`，C++不會幫你編譯檢查出錯誤

## Declaring and Initializing Arrays

宣告陣列

`int`表示這個陣列元素的型別
`test_scores`是陣列的名稱
`[5]`表示這個陣列內含5個元素
（由於test_scores這個陣列只有宣告，沒有初始化，因此裡面的5個值都是垃圾值，不知道可能會被放入什麼內容）

```cpp
int test_scores [5];
```

宣告+初始化陣列

宣告一個陣列，包含5個數字，分別是100、99、98、97、96
```cpp
int test_scores [5] {100, 99, 98, 97, 96};
```

宣告一個陣列，包含10個數字，第1個數字是3、第2個是5，剩下8個是0
```cpp
int high_score_per_level [10] {3, 5};
```

大括號裡面什麼都沒寫，表示這個陣列中10個數字都是0
```cpp
int high_score_per_level [10] {};
```

這個陣列宣告時沒有指定數量，因此C++觀察初始值有5個內容，幫忙解釋成初始宣告數量為5，又等於：`int another_array [5] {1, 2, 3, 4, 5};`
```cpp
int another_array [] {1, 2, 3, 4, 5};
```

## Accessing and Modifying

訪問陣列元素，使用中括號+索引值
```cpp
int test_scores [5] {100, 99, 98, 97, 96};
cout << test_scores[0] << endl; // 100;
```

## Multidimensional Array

宣告一個2D陣列：
```cpp
int movie_rating [3][4] {
	{0, 4, 3, 5},
	{2, 3, ,3, 5},
	{1, 4, 4, 5}
}
```

## Vectors

C++的Array是固定大小，但是Vector可以動態修改大小，並且，Vector也能進行邊界檢查

跟Array不同的是，Vecotr是一個物件，宣告時，要載入Vecotr Library，而且Vector是標準庫的一部分，所以需要使用namespace或是`std::`宣告

宣告空Vector：
```cpp
#include <vector>

using namespace std;

int main() {
	vector<char> vowels;
	vector<int> test_scores;

	return 0;
}
```

（以下的範例為了簡化，省略include vector的部分）
宣告一個內含10個int的陣列，並且裡面10個數字都設為0

```cpp
vector<int> test_scores(10);
```

實例化
```cpp
vector<char> vowels {'a', 'i', 'u', 'e', 'o'};
vector<int> test_scores {100, 99, 98, 97, 96};
```

一口氣指定初始值：宣告一個叫做hi_temperatures的Vector，裡面含365個double元素，並且設定每個double的值為80.0
```cpp
vector<double> hi_temperatures(365, 80.0);
```

訪問Vector元素，利用中括號：
```cpp
vector<char> vowels {'a', 'i', 'u', 'e', 'o'};

cout << vowels[0] << endl; // 'a'
```

也可以利用`at()`語法訪問
```cpp
vector<char> vowels {'a', 'i', 'u', 'e', 'o'};

cout << vowels.at(0) << endl; // 'a'
```

插入值，使用`puch_back()`語法
```cpp
vector<char> vowels {'a', 'i', 'u', 'e'};

vowels.push_back('o');
```

查看Vector有多少元素
```cpp
vector<char> vowels {'a', 'i', 'u', 'e', 'o'};

cout << vowels.size() << endl; // 5
```

2D Vector
```cpp
vector<vector<int>> movie_ratings {
	{0, 4, 3, 5},
	{2, 3, ,3, 5},
	{1, 4, 4, 5}
}

cout << movie_ratings[0][2] << endl; // 3
cout << movie_ratings.at(0).at(2) << endl; // 3
```











