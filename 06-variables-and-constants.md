# Section 6: Variables and Constants

## Delcaring and Initializing Variables



C++有3種初始化參數的方法：
* `int age = 21;`：C風格的參數初始化
* `int age (21);`：構造函數（Constructor）初始化
* `int age {21};`：C++11初始化語法

最推薦使用的是這種：`int age {21};`

補充：`int age;`：只有宣告，沒有初始化參數

## Primitive Data Types

又稱作「Fundamental data types」

在C++中，原始數據類型能夠儲存多少值，是跟電腦的規格有關：

|電腦位元|可儲存值（次方）|可儲存值（byte數）|
|-------|--------|-----|
|8位元|2的8次方|256|
|16位元|2的16次方|65,356|
|32位元|2的32次方|4,294,967,296|
|64位元|2的64次方|18,446,744,073,709,551,616|

### Character

```cpp
char middle_initial {'J'};
cout << "My middle initial is " << middle_initial << endl;
// My middle initial is J
```

### Interger

`unsigned`用來儲存只有`0`或是正數的數值（如果未宣告，預設都為`signed`）

`short`表示用來儲存比較短的數字，數字範圍是`-32,768`至`32,767`
```cpp
unsigned short int exam_score {56};
```

`long`用來宣告大數字，範圍是`-2,147,483,648`至`2,147,483,647`
```cpp
long people_in_florida {20610000};
```

`long long`，範圍是`-9,223,372,036,854,775,808`至`9,223,372,036,854,775,807`
可以用單引號`'`分割數字，增加易讀性，不會影響數字內容
```cpp
long long people_on_earth {7'600'000'000};
```

用大括號`{}`初始化數字可以避免溢出，例如這個範例中，使用了`short`宣告了數字，但是指派的數字明顯超出這個範圍

```cpp
short people_on_earth {9223372036854775807};
```

因為使用大括號，所以編譯器會提示紅色的編譯錯誤，因此會阻止程式運行

但是如果是用等號宣告，則編譯器只會出現黃色的警告而已，所以還是可以運行程式，但是會得到一個完全不一樣的數字（因為內存溢出）

```cpp
short people_on_earth = 9223372036854775807;
```

### Floating 

`float`用來宣告7位數
```cpp
float car_payment {401.23};
```

`double`宣告15位數
```cpp
double pi {3.14159};
```

`long double`宣告很大的有浮數數字
```cpp
long double large_amount {2.7e120};
```

### Boolean

在C++中，`0`是`false`，其他都是`true`

```cpp
bool game_over {false};
cout << game_over << endl; // 0
```

## `sizeof`

可以用`sizeof`來計算變數或是型別的byte

計算型別大小
```cpp
sizeof(int); //4
sizeof(double); //8
```

計算變數大小
```cpp
sizeof(some_variable);
sizeof some_variable;
```

也可以透過引用`<climits>`與`<cfloat>`測量int或是float的最大最小值的byte size

```cpp
#include <iostream>
#include <cfloat>
#include <climits> // C++20可以省略此行

using namespace std;

int main(int argc, const char * argv[]) {
	cout << sizeof(INT_MIN) << endl; // 4
	cout << sizeof(INT_MAX) << endl; // 4
	cout << sizeof(LONG_MIN) << endl; // 8
	cout << sizeof(LONG_MAX) << endl; // 8
	cout << sizeof(FLT_MIN) << endl; // 4
	cout << sizeof(FLT_MAX) << endl; // 4

	return 0;
}
```

## Constant

常數就是不能改值的變數
C++有好幾種宣告常數的方式

Declared constants：用關鍵字`const`宣告

```cpp
const string x = "Hi";
x = "Heyy"; // error
```

Defined constants：用預處理器關鍵字`#define`宣告，常見於古典版本的C++，不建議用於現代版本的C++中

```cpp
#define pi 3.14
```

`#define pi 3.14`的意思是，當C++在程式中發現`pi`，就替換成`3.14`








