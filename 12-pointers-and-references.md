# Section 12: Pointers and References

## Pointer

指針是一個變量，儲存另一個變量或函數的記憶體位置

## Declaring Pointer

宣告指針的方式有2種，2種的效果都一樣

第一種（星號擺在變數名稱之前）
```cpp
int *number;
```

第二種（星號擺在型別之後）
```cpp
int* number;
```

初始化指針
因為如果只宣告，沒有初始化指針的話，會有垃圾值，所以務必維持好習慣：宣告指針的同時初始化他

```cpp
int *number {nullptr};
```

指針的垃圾值：一個隨意產生的內存地址，可能指向某個變數的內存地址

指針的初始值：`nullptr`，表示「point nowhere」，在C++11當中，代表地址0，說明指針沒有指向任何地方

## 訪問指針地址

使用地址運算符`&`，返回該變數的指針地址

```cpp
int num {10};
    
cout << num << endl; // 10
cout << sizeof num << endl; // 4
cout << &num << endl; // 0x16fdff218
```

範例：
```cpp
int *p; // 錯誤示範：缺少初始化
    
cout << p << endl; // 垃圾值：0x100003d8c

cout << sizeof p << endl; // 8

p = nullptr; // 將指針設定為nowhere

cout << p << endl; // 0x0
```

指針變數的大小
所有型別的指針變數的大小都一樣（8）

```cpp
int *p1 {nullptr};
double *p2 {nullptr};
unsigned long long *p3 {nullptr};
vector<string> *p4 {nullptr};
string *p5 {nullptr};

cout << sizeof p1 << endl; // 8
cout << sizeof p2 << endl; // 8
cout << sizeof p3 << endl; // 8
cout << sizeof p4 << endl; // 8
cout << sizeof p5 << endl; // 8
```

類型化指針（Typed Pointers），有別於初始化為`nullprt`，直接初始化指針指向一個變數

```cpp
int score {10};
double high_temp {100.8};

int *score_ptr {nullptr};

score_ptr = &score; // OK，類型化指針

score_ptr = &high_temp; // compile error
```

`score_ptr`不論對上哪種型別，都是8位元，所以`score_ptr = &high_temp;`的問題不是內存大小不匹配，而是型別衝突

指針總結：
- 指針就是變數，所以他可以被改
- 指針可以為`null`
- 指針可以不初始化（建議都初始化比較好）

範例解析：

```cpp
int *p;
    
cout << p << endl; // 垃圾值
cout << &p << endl; // 指針變數p的地址
cout << sizeof p << endl; // 指針變數p的大小
```

宣告整數的指針變數「`p`」，指針變數，跟普通變數一樣，都會從內存劃分一塊位置，儲存內容，只是，指針變數的內容，是記憶體位置（存另一個變數的記體體位置）

本範例中，由於沒有初始化指針變數`p`，導致存放了垃圾值，也就是某個不知名變數的記憶體位置

`&p`會提示指針變數`p`他自己的記憶體位置

## Dereferencing a Pointer

反指針（Dereferencing），表示從一個記憶體位置推回這個變數存的值，反指針的操作符是`*`（聲明一個指針變數的符號也是`*`）

```cpp
int score {100};
int *score_ptr {&score};

// 反指針
cout << *score_ptr << endl; // 100

// 利用反指針特性，修改裡面的內容
*score_ptr = 200;

cout << *score_ptr << endl; // 200
cout << score << endl; // 200
```

調換指針變數的儲存內容（換成存另一個變數），然後用反指針查看值的改變

```cpp
double high_temp {100.7};
double low_temp {37.4};
double *temp_ptr {&high_temp};

cout << *temp_ptr << endl; // 100.7

// 改成存其他變數的指針位置
temp_ptr = &low_temp;

cout << *temp_ptr << endl; // 37.4
```

補充：反指標如果要跟其他method並用，可以用刮號匡起來

```cpp
vector<string> members {"A", "B", "C"};
vector<string> *members_ptr {nullptr};

members_ptr = &members;

cout << (*members_ptr).at(0) << endl; // 用括號製造更高的優先級
```

## Dynamic Memory Allocation

Dynamic Memory Allocation：在程式運行時從堆積（Heap，預留一大塊空間給未來使用）中分配儲存空間

使用`new`分配新的空間

```cpp
int *int_ptr {nullptr};
    
cout << int_ptr << endl; // 0x0

int_ptr = new int; // 在heap中分配一塊整數的空間

cout << int_ptr << endl; // 0x60000000c020
cout << *int_ptr << endl; // 垃圾值

*int_ptr = 100;

cout << *int_ptr << endl;

delete int_ptr; // 因為用完了，所以刪除指針，釋出heap的空間
```

使用`new[]`為Array分配新的空間

```cpp
int *array_ptr {nullptr};
size_t size {};

cout << "How big do you want the array?";
cin >> size;

array_ptr = new int[size];

delete [] array_ptr;
```

因為這種操作，沒有建立正常的變數名稱，所以一定要存好指針變數，一旦失去了指針（例如，再分配結束後再次將臨時指針初始化為`nullptr`），會造成內存泄露

`delete`只是釋出堆的空間，臨時指針仍然指向內存位置

如果對分配內存做無限循環（錯誤示範），程式會一直一直分配，最終用完內存與堆的空間，跳出「std::bad_alloc」的錯誤訊息
```cpp
int *array_ptr {nullptr};
size_t size 999;

while(true) {
	array_ptr = new int[size];
}
```

## Array跟Pointer之間的關係

- 陣列變數名稱的值，表示該陣列第一項的內存位置
- 指針變數名稱的值，是一個內存位置
- 在相同型別下，陣列變數跟指針變數幾乎是可互換的（唯一差別是陣列是不可變的變量，不能事後修改）

```cpp
int scores[] {1, 2, 3};
    
cout << scores << endl; // 0x16fdff208
cout << *scores << endl; // 第一項的值，1，scores雖然是陣列，但是也可以反指針（！）

int *score_ptr {scores};

cout << score_ptr << endl; // 0x16fdff208
cout << *score_ptr << endl; // 1

// scores跟score_ptr幾乎可以說是一樣的東西
```


甚至，陣列的指針，也能操作索引
```cpp
int scores[] {1, 2, 3};
int *score_ptr {scores};

cout << score_ptr[0] << endl; // 1
cout << score_ptr[1] << endl; // 2
cout << score_ptr[2] << endl; // 3
```

利用儲存元素（本範例為整數）的大小空間，反推其他元素的內存位置

```cpp
int scores[] {1, 2, 3};
int *score_ptr {scores};

cout << score_ptr << endl; // 0x16fdff208
cout << (score_ptr + 1) << endl; // 0x16fdff20c
cout << (score_ptr + 2) << endl; // 0x16fdff210

// 取得內存位置後，就能做反指針
cout << *score_ptr << endl; // 1
cout << *(score_ptr + 1) << endl; // 2
cout << *(score_ptr + 2) << endl; // 3
```

| Type | Subscript Notation | Offset Notation |
| ---- | ----- | ------- |
| **Array** | scores[index] | *(scores + index) |
| **Pointer** | scores_ptr[index] | *(scores_ptr + index) |


## 指針算法

`++`讓指針指向下一個元素，`--`讓指針指向前一個元素

```cpp
int scores[] {1, 2, 3};
int *score_ptr {scores};

cout << score_ptr << endl; // 0x16fdff20c，存放1的內存位置
cout << ++score_ptr << endl; // 0x16fdff210，存放2的內存位置
```

返回兩個指針之間的元素數量
```cpp
int n = int_ptr2 - int_ptr1;
```

指針的比較：不檢查值是否相等，只檢查地址是否相等

```cpp
string s1 {"Frank"};
string s2 {"Frank"};

string *p1 {&s1};
string *p2 {&s2};
string *p3 {&s1};

cout << (p1 == p2) << endl; // false
cout << (p1 == p3) << endl; // true
```

比較反指針：

```cpp
string s1 {"Frank"};
string s2 {"Frank"};

string *p1 {&s1};
string *p2 {&s2};
string *p3 {&s1};

cout << (*p1 == *p2) << endl; // true
cout << (*p1 == *p3) << endl; // true
```

範例
```cpp
int scores[] {100, 95, 89, 68, -1};
int *score_ptr {scores};

while (*score_ptr != -1) {
	cout << *score_ptr << endl;
	score_ptr++; // 每次循環結束後，往後移動一個指針
}

// 100
// 95
// 89
// 68
```

範例（改成一行）
```
int scores[] {100, 95, 89, 68, -1};
int *score_ptr {scores};

while (*score_ptr != -1) {
	cout << *score_ptr++ << endl;
}

// 100
// 95
// 89
// 68
```

## Const and Pointers

Pointers to constant（指向常數）
`const`寫在最前面：反指針無法被修改，指針變數本身可以指派給其他位址

```cpp
int high_score {100};
int low_score {65};

const int *score_ptr {&high_score};

*score_ptr = 86; // ERROR
score_ptr = &low_score; // OK
```

Constant pointers（常量的指針）
`const`寫在`*`的後面，反指針能夠被修改，指針變數本身無法指派給其他位址

```cpp
int high_score {100};
int low_score {65};

int *const score_ptr {&high_score};

*score_ptr = 86; // OK
score_ptr = &low_score; // ERROR
```

Constant pointers to constants（常量的指針指向常數）
最前面跟`*`的後面都寫`const`，反指針不能被修改，指針變數本身也不能重新指派

```cpp
int high_score {100};
int low_score {65};

const int *const score_ptr {&high_score};

*score_ptr = 86; // ERROR
score_ptr = &low_score; // ERROR
```

## Passing Pointer to Functions

透過指針變數傳遞函數的參數（Pass-by-reference）
```cpp
void fn(int *int_ptr); // prototype

void fn(int *int_ptr) {
    *int_ptr *= 2; // 取消引用指針，將值乘以2，再重新賦值
}

int main() {
    
    int value {10};
    
    cout << value << endl; // 10
    
    fn(&value);
    
    cout << value << endl; // 20
    
    int_ptr = &value;
    
    fn(int_ptr);
    
    cout << value << endl; // 40

    return 0;
}
```


swap範例：
```cpp
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main() {
    
    int x {100}, y {200};
    
    cout << x << endl; // 100
    cout << y << endl; // 200
    
    swap(&x, &y);
    
    cout << x << endl; // 200
    cout << y << endl; // 100
    
    
    return 0;
}
```

1. `void swap(int *a, int *b)`的部分宣告這個函數的參數`a`跟`b`都是指針變數，用來儲存地址，在stack上挖出兩個變數的空格，用來存位置
2. `swap(&x, &y)`，因為`x`跟`y`用了參照操作符，所以會帶入`x`跟`y`的地址，因此這行變成`swap(0x16fdff218, 0x16fdff214)`
3. 搭配先前在stack上挖出的空格，參數`a`跟`b`填入`0x16fdff218`跟`0x16fdff214`這兩個位置的值
4. `int temp = *a`，是Pass-by-reference，指向在main宣告的`int x {100}`這個`x`，直接指向他
5. `*a = *b`，一樣先找到main裡面的`int y {200}`的這一個變數，然後讓main裡面的的`int x`等於`y`

vector範例：
```cpp
void display(vector<string> *v) {
    for (auto str: *v) {
        cout << str << ", ";
    }
    
    cout << endl;
}

int main() {
    
    vector<string> people {"A", "B", "C"};
    
    display(&people);
    // A, B, C
    
    return 0;
}
```

`for (auto str: *v)`的`v`，存放了people的內存位置，也就是0x16fdff1b0，v不是向量，他只是一個指針，但是我們為了跑回圈，需要得到一個真的vector，因此對著指針變數v做了反指針：`*v`


搭配const，讓display禁止修改vecotr：

```diff cpp
-void display(vector<string> *v) {
+void display(const vector<string> *v) {
	(*v).at(0) = "Funny"; // Error

	for (auto str: *v) {
		cout << str << ", ";
	}
    
	cout << endl;
}
```

指針參數搭配const，禁止修改指針參照：
```diff cpp
-void display(vector<string> *v) {
+void display(vector<string> *const v) {

	for (auto str: *v) {
		cout << str << ", ";
	}
    
	cout << endl;
	
	v = nullptr; // Error
}
```

array範例：

`sentinel`是哨兵的意思，當看到這個值，就觸發某件事

```cpp
void display(int *array, int sentinel) {
    while (*array != sentinel) {
        cout << *array++ << " ";
    }
    
    cout << endl;
}

int main() {
    
    int scores[] {100, 99, 98, 97, -1};
    display(scores, -1);
    
    // 100 99 98 97
    
    return 0;
}
```

## Returning a Pointer from a Function

C++的函數可以返回指針

int *largest_int(int *ptr1, int *ptr2) {
    if (*ptr1 > *ptr2) {
        return ptr1;
    } else {
        return ptr2;
    }
}

int main() {
    int a {100}, b {200};
    int *largest_ptr {nullptr};
    
    largest_ptr = largest_int(&a, &b);
    
    cout << largest_ptr << endl; // 0x16fdff214
    
    return 0;
}
```

返回動態分配的內存：


```cpp
int *create_array(size_t size, int init_value = 0) {
    int *new_storage {nullptr}; // 宣告內存位置為0x0的指針變數
    new_storage = new int[size]; // 從heap上挖出一塊空間（heap跟stack在不同區域），因為init_value的呼叫參數是100，所以挖出100個空間，並且任new_storage指向那塊heap的空間
    
    // 小心不要弄丟了指向heap的指針參數，不然會找不到heap


    // heap雖然幫忙排了100個空間，但是100個都是垃圾值，所以要重新賦值
    for (size_t i {0}; i < size; ++i) {
        
        *(new_storage + i) = init_value; // 使用Offset Notation
        //new_storage[i] = init_value // 也可使用Subscript Notation
    }
    
    return new_storage; // 返回指針變數new_storage，指向heap的位置，heap經過上面的操作，已經有一塊長度100的陣列了
}

int main() {
    
    int *my_array;
    
    // 讓區域變數my_array，指向heap的陣列，當create_array結束後，new_storage會被彈出（清除），我們失去了找到heap的陣列的指針，但是透過回傳值，讓指針跟my_array串起來
    my_array = create_array(100, 20);
    
    delete [] my_array;
    
    return 0;
}
```

錯誤示範：返回函數的本地指針

以下兩個範例都有返回本地指針，為不良示範，因為函數執行期間有期限，當他結束後，區域變數都會消失，堆棧被釋放，返回的本地指針會不知道被指到哪

```cpp
int *dont_do_this() {
    int size {};
    // ...
    
    return &size;
}

int *or_this() {
    int size {};
    int *int_ptr {&size};
    // ...
    
    return int_ptr;
}
```

## 會造成問題的指針用法


未初始化的指針
```
int *int_ptr; // 可以指向任何地方
// ...
*int_ptr = 100; // 因為不清楚指針指向哪，如果嘗試修改反指針，可能導致Crash（改到不能改的變數之類）
```

懸空指針（Dangling Pointer）
- 指針指向到已經被釋放的記憶體位置，例如：A與B兩個指針變數指向同一個位置，A指針變數刪除本體（`delete`），B變數成為了懸空指針
- 指針指向無效記憶體：回傳區域變數內存位置的函數

沒有檢查`new`是否失敗
沒有透過exception handling檢查`new`是不是失敗，容易做出一個空指針，造成之後反指針空指針的問題

內存泄露
- 重新分配記憶體時，忘記用`delete`釋放heap的空間
- 遺失了指向heap空間的指針變數

## Reference

引用變數，看起來跟指針變數很像，但是是不一樣的：

- 是變數的一個異名
- 宣告reference時，必須被初始化為變數
- 不能為空值（`null`）
- 一旦被初始化後，不能在引用到另一個變數中
- 可以將reference想像為一個自動反指針的不可變指針（constant pointer）

reference適合用在函數參數中

```
void fn(int &a) {
    a = 100;
}

int main() {
    
    int num {2};
    
    cout << num << endl; // 2
    
    fn(num);
    
    cout << num << endl; // 100
    
    return 0;
}
```

另一個reference的用法：range-based for loop

本範例中可以看到，嘗試透過for迴圈修改people vector，但是卻只能夠修改拷貝的
`str`，不會影響到本體

```cpp
vector<string> people {"A", "B", "C"};
    
for (auto str: people) {
	str = "Funny"; // 修改拷貝，不影響本體
}

for (auto str: people) {
	cout << str << endl; // A, B, C
}
```

透過將for迴圈的變數，改為引用後，`str`直接指向people的項目，就能直接修改本體了

```diff cpp
vector<string> people {"A", "B", "C"};
    
-for (auto str: people) {
+for (auto &str: people) {
	str = "Funny"; // 修改本體
}

for (auto str: people) {
	cout << str << endl; // Funny, Funny, Funny
}
```

如果for的引用參數搭配了`const`，再去嘗試修改str，會導致編譯錯誤

```cpp
vector<string> people {"A", "B", "C"};
    
for (auto const &str: people) { // 雖然提供了引用，讓str直接指向本體，但是加上了`const`上str成為不可修改
	str = "Funny"; // 修改str，編譯錯誤
}
```

reference，除了能夠直接修改本體vector，也能減少每次複製向量元素的成本，推薦在for使用reference


reference的簡單用法：
```cpp
int num {100};
int &ref {num};

cout << num << endl; // 100
cout << ref << endl; // 100，ref為num的別名

num = 200;

cout << num << endl; // 200
cout << ref << endl; // 200，num修改後，ref也會跟著被改
```

## L-values and R-values

L-values（左值）

- 具有變數名稱與內存位置的值
- 可以修改（當不是被宣告為`const`的時候）

```
int x {100}; // x是l-value
x = 1000;
x = 1000 + 20;

string name; // name是l-value
name = "Frank";
```

R-values（R值，非左值）

R值通常位於述句的右側，通常為臨時值

```
int x {100}; // 100是r-value
int y = x + 20; // (x + 20)是r-value

string name;
name = "Frank"; // "Frank"是r-value

int max_num = max(20, 30); // max(20, 30)是r-value
```

R值可顯性指派給L值

```cpp
int x {100};
int y {0};

y = 100; // R值(100)指派給L值(y)

x = x + y; // R值(x+y)指派給L值(x)
```

L值引用（L-values reference）
目前我們做的所有reference都是L值引用

```cpp
int x {100};
    
int &ref1 = x; // ref1參照L值(x)
ref1 = 1000;

int &ref2 = 200; // Error，因為200是R值
```

同樣，R值引用也不能發生在函數參數中
```cpp
int square(int &n) {
    return n*n;
}

int main() {
   
    int num {10};
    
    suuare(num); // OK
    
    square(5); // Error，5不能被引用
    
    return 0;
}
```


