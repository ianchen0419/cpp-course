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

`rand()`函數是偽隨機數，程式在執行`rand()`時，會先產生一個隨機樹種子數，在透過一系列演算法，由這個種子數向外推導隨機數字

假如同時用`rand()`產生10個隨機數，因為隨機數種子都是固定的，所以這串隨機數組就真的很不隨機

為了提升隨機性，我們利用`srand()`搭配時間函數，由系統劃分10個時間點，每個時間點都用`srand()`先產生不同的隨機數種子，在產生隨機數

## 定義Function

Function需要4個部分：

- 名稱
- 參數清單：包括參數的型別也要被定義
- 回傳值的型別
- 內容（使用大括弧包起來的部分）

function的範例（回傳值為int）
```cpp
int function_name(int a) {
	statements;
	return 0;
}
```

function的範例（回傳值為無）
```cpp
void function_name(int a) {
	statements;
	return; // 也可省略
}
```

由於當調用函數時，編譯器需要知道函數的詳細資訊，因此如果先呼叫函數，然後才定義函數的話，會發生編譯器錯誤

```cpp
int main() {
	say(); // compile error
	return 0;
}

void say() {
	cout << "Hello" << endl;
}
```

## Function Prototypes

由於C++必須在調用函數前，取得該函數的所有詳細資料，因此有2種做法可以實踐

1. 提前定義函數：適用於小型的程式開發，但是當程式規模變大，會變得難以維護，也不能允許兩個函示互call
2. 使用函數原型（Function Prototypes）

函數原型，又稱為forward declarations，用途是告訴編譯器，該函數的所有詳細資訊（而非定義函數），通常定義在C++文件的最上頭，或是header files

用法介紹：
function prototype只需要參數型別
```cpp
int function_name(int); // function prototype

int function_name(int a) {
	statements;
	return 0;
}
```

也可以連帶參數名稱也定義好，但是function prototype會忽略而已
```cpp
int function_name(int a); // function prototype

int function_name(int a) {
	statements;
	return 0;
}
```

## Function Parameters

參數（Parameters/Arguments）是隨函數傳遞的資料
- 當在函數定義部分，稱為Parameters
- 當進入了函數內部，稱為Arguments

```cpp
int add(int, int); // prototype

int main() {
	int result {0};
	result = add(100, 200); // call
	return 0;
}

// definition
int add(int a, int b) {
	return a + b; 
}
```

參數傳遞的機制：
C++當中，參數是pass-by-value，傳遞時，會先產生該參數的一個副本（argument），到函數結束為止，即使修改了函數參數（parameter）的值，該副本（argument）的值都不會被變動

- Formal parameters：定義的參數（parameter）
- Actual parameters：在呼叫函示時被使用的參數（argument）

```cpp
void fn(int formal) { // formal(形式參數)會拷貝一份actual(實際參數)的值
    cout << formal << endl; // 50
    formal = 100;
    cout << formal << endl;
}

int main() {
    
    int actual {50};
    cout << actual << endl; // 50
    
    fn(actual);
    
    cout << actual << endl; // still 50
    
    return 0;
}
```

## Default Argument Values

- 參數默認值可以定義在函數原型，也能定義在函數定義式中，但是不能同時定義在兩個地方，建議只定義在函數原型
- 參數默認值必須定義在參數清單的最後面

```cpp
double calc_cost(double cost, double tax = 0.06);

double calc_cost(double cost, double tax) {
    return cost += (cost * tax);
}

int main() {
    
    double cost {0};
    
    cost = calc_cost(200.0); // 使用預設參數tax的值：0.06
    cost = calc_cost(100.0, 0.08); // 覆蓋預設參數tax的值，改用0.08
    
    return 0;
}
```

## Overloading Functions

C++允許不同函數（帶有不同參數清單）共用同一個名字

```cpp
int add_numbers(int, int);
double add_numbers(double, double);

int add_numbers(int a, int b) {
    return a + b;
}

double add_numbers(double a, double b) {
    return a + b;
}

int main() {
    
    cout << add_numbers(10, 20) << endl; // 30
    cout << add_numbers(10.0, 20.0) << endl; // 30.0
    
    return 0;
}
```

## 傳遞Array

在參數中傳遞Array的寫法如下：

```cpp
void fn(int numbers[]);
```

- 在C++中，如果在函示傳遞Array時，他會拷貝這個Array的**位置**到arguments裡面去（Pass-by-reference）
- 傳遞Array時，通常需要連Array大小（size_t）也一起傳遞

因為Formal argument是Copy-by-reference，所以修改參數時，會連同Actual argument也被改動到！

```cpp
void zero_array(int numbers [], size_t size) {
    for (size_t i {0}; i < size; i++) {
        numbers[i] = 0;
    }
}

int main() {
    
    int numbers[] {1, 2, 3, 4, 5};
    
    zero_array(numbers, 5);
    
    // Actual argument會被改到！
    cout << numbers[0] << endl; // 0
    cout << numbers[1] << endl; // 0
    cout << numbers[2] << endl; // 0
    cout << numbers[3] << endl; // 0
    cout << numbers[4] << endl; // 0
    
    return 0;
}
```

避免actual argument被不小心修改的解決方法：定義const parameters

```diff cpp
-void zero_array(int numbers [], size_t size) {
+void zero_array(const int numbers [], size_t size) {
    for (size_t i {0}; i < size; i++) {
        numbers[i] = 0; // compile error
    }
}
```

定義const之後，如果嘗試在函數內修改參數，會導致編譯錯誤

## Pass by Reference (Reference Parameter)

在C++當中，只有函數參數是Array時，才會自動Pass by Reference，如果參數是int、C++字串、C-Style字串、Vector，都是Pass by Value

有時候，在Pass by Value的場合，會需要能夠修改外面的Actual Argument，這時需要讓函式，能夠抓到這個Argument的記憶體位置（使用`&`符號傳送參數）

```cpp
void scale(int &num); // prototype

// definition
void scale(int &num) {
    num = 100;
}

int main() {
    
    int number {10};
    
    cout << number << endl; // 10
    
    scale(number);
    
    cout << number << endl; // 100，Actual Argument被更改了！
    
    return 0;
}
```

當在參數清單中使用了`int &num`，表示使用了實體參數，`num`成為了`number`的別名

Vector參數的範例：Vector原本也是Pass by Value，但是複製一份Vector的成本很大，所以有時也會需要改成Pass by Reference

並且因為這樣做的目的是為了增進效能，實際上仍不希望實際參數被改到，所以在參數中加上了`const`，讓編譯器可以幫忙檢查

```diff cpp
#include <iostream>
#include <vector>

using namespace std;

-void print(vector<int> v); // prototype
+void print(const vector<int> &v); // prototype

// definition
-void print(vector<int> v) {
+void print(const vector<int> &v) {
    for(int num: v) {
        cout << num << endl;
    }
}

int main() {
    
    vector<int> number {1, 2, 3};
    
    print(number);
    
    
    return 0;
}
```

## Scope Rules 作用域

Local Scope（局部作用域）：由大括號`{`到`}`之中都是Local Scope範圍，局部作用域的變數無法跨平行作用域（函式）被保存，但是巢狀作用域結構（函式裡面包函式）能夠保存局部變數

能夠跨作用域被保存的局部變數：Static Local Variables，例外於一般的局部變數，靜態局部變數的壽命是程式開始執行到程式結束執行，但是他只會初始化一次（第一次調用此函數時）

Global Scopea（全域作用域）：可見於整個程式的任何部分

靜態局部變數：
```cpp
void fn() {
    static int num {100};
    num += 100;
    cout << num << endl;
}

int main() {
    
    fn(); // 200
    fn(); // 300
    
    return 0;
}
```

當fn第一次執行時，靜態變數`num`會被初始化為100，然後繼續執行程式內容：100+100=200，`num`作為200被保存了下來

而在fn第二次執行時，`num`不會再被初始化了，`num`接續先前儲存的值200，繼續執行程式：200+100=300，`num`成為了300

## Function Calls

函數調用的順序，依照函數調用堆疊（Function Calls Stack）的方式處理，順序為後進先出（LIFO），類似人們排隊的順序

堆疊楨（Static Frame）/激活紀錄（Activate Record）
1. 調用函數時，建立激活紀錄
2. 同時將此函數推送到函數調用堆疊棧上，
3. 當函數執行結束，移出函數調用堆疊棧外，處理返回值
4. 本地變數與函數參數會被分配在堆疊棧上

堆疊棧的空間是有限的，如果激活的功能太多，用完了堆疊棧的空間，會產生溢出錯誤（Stack Overflow）

內存分配策略，記憶體依照用途可分為4個區塊：
1. 程式代碼（Code Area）
2. 靜態變數（Static Variables）
3. 函數調用堆疊棧（Function Calls Stack）
4. 堆/自由存儲（Heap / Free Store）

函數調用範例：
```cpp
void func2(int &x, int y, int z) {
    x += y + z;
}

int func1(int a, int b) {
    int result {};
    
    result = a + b;
    func2(result, a, b);
    
    return result;
}

int main() {
    int x {10};
    int y {20};
    int z {};
    
    z = func1(x, y);
    
    cout << z << endl;
    
    return 0;
}
```

1. main()呼叫func1()
2. main()向堆疊棧推入空間（為了func1的return值）
3. main()向堆疊棧推入空間（為了func1的參數）
4. main()向堆疊棧推入func1的return值的記憶體位置
5. 轉移控制給func1()的（jump指令）

轉移到func1之後：
1. func1()推送先前激活紀錄的地址（移動堆棧指針）
2. 推送任何需要在調用函數前復原的寄存器
3. 執行func1()當中的代碼
4. 將寄存器恢復到main()原來的位置，恢復以前的激活紀錄
5. 儲存函數的result值（存在main()幫忙規劃好的return的記憶體位置）
6. 將控制權轉移給main()

再度轉移回main，因為func1已經結束了，開始清理不必要的資料：
1. 彈出（清除）func1參數
2. 彈出（清除）func1的return值

## Inline Functions

綜上所述，我們知道調用函數需要成本，有時候，一個很簡單的函數所花費的調用成本可能大於執行成本，為了避免此情況，我們可以選擇使用內聯函數（Inline Function），內聯函數的速度更快，但是如果此內聯函數重複了多次，會導致代碼膨脹（C++會自己產生一堆內聯）

```cpp
inline int add_numbers(int a, int b) { // definition
    return a + b;
}

int main() {
    
    int result {0};
    
    result = add_numbers(100, 200); // call
    
    return 0;
}
```

## Recursive Function（遞歸函數）

遞歸函數是函數自己call自己（直接或是間接都會），導致堆棧上存在兩組（或更多）相同的激活紀錄

適合用遞歸函數解決的問題：Factorial（階乘）、Fibonacci（費波那契數）、Fractals（分形）、Binary Search（二元搜尋）、Search Tree（搜尋樹）、DFS（深度優先搜尋）

階乘範例解說：首先來複習數學的階乘
```
0! = 1（例外）
1! = 1 = 1
...
4! = 4 * 3 * 2 * 1 = 24
5! = 5 * 4 * 3 * 2 * 1 = 120
```

化為公式則為
```
0! = 1（例外）
n! = n * (n - 1)!
```

階乘（Factorial）的時間複雜度是O(n!)

時間複雜度簡表
| Big O | 複雜度 | 處理速度（由快到慢）| 範例 |
| --- | --- | --- | --- |
| O(1) | constant | fast | |
| O(log n) | logarithmic | | |
| O(n) | linear | | iteration |
| O(n * log n) | log linear | | |
| O(n^2) | quadratic | | |
| O(n^3) | cubic | | |
| O(2^n) | exponential | | fibonacci |
| O(n!)	| factorial | slow | |

```cpp
unsigned long long factorial(unsigned long long n) {
    if (n == 0) {
        return 1; // base case 例外的0!
    } else {
        return n * factorial(n - 1); // recursive case 一般情況
    }
}

int main() {
    cout << factorial(8) << endl;
    
    return 0;
}
```

base case的設置非常重要，他讓整個演算法可以調出無限的遞歸，避開了內存溢出錯誤

費波那契數列解說

數學複習
```
數列：1, 1, 2, 3, 5, 8, 13, ...

第一個數字：就是1（例外 / Base Case）
第二個數字：1（也算例外 / Base Case）
第三個數字：1 + 1 = 2
第四個數字：1 + 2 = 3
第五個數字：2 + 3 = 5
第六個數字：3 + 5 = 8
...
```

抽象化：
```
Fib(0) = 1 (Base Case)
Fib(1) = 1 (Base Case)
Fib(n) = Fib(n - 1) + Fib(n - 2) (Recursive Case)
```

費波那契數列的時間複雜度是 O(2^n)

```cpp
unsigned long long fibonacci(unsigned long long n) {
    if (n <= 1) {
        return n;
    } else {
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
}

// n是第幾項的序號，從1開始到無限大，不會有0跟負數

int main() {
    cout << fibonacci(30) << endl;
    
    return 0;
}
```








