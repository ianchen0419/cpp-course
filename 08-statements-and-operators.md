# Section 8: Statements and Operators

## Assignment Operator

這個範例中將num1改指派為100，`num1 = 100`的`num1`在這個範例中是用來指代num1的記憶體位置，亦即在這個記憶體位置改存100這個數字

```cpp
int num1 {10};
num1 = 100;
```

複雜範例：

```cpp
int num1 {10};
int num2 {20};

num1 = num2 = 1000;
```


這個範例中，首先C++會先解讀`num2 = 1000`，先將`num2`指派為`1000`，剩下再解釋`num1 = 1000`，再將`num1`指派為`1000`

## Increment and Decrement Operators

Increment Operator有兩種寫法：Prefix與Postfix

```cpp
++num; // prefix
num++; // postfix
```

Prefix：
`++`出現在`counter`之前，表示在使用`counter`之前，要先讓他加一，也就是`counter = counter + 1`，結果為`11`，然後再解讀`result = 11`，因此`counter`跟result都是11

```cpp
int counter {10};
int result {0};

result = ++counter;

cout << counter << endl; // 11
cout << result << endl; // 11
```

Postfix：
`++`出現在`counter`之後，代表在使用完`++`之後，要讓他加一，因此會先執行`result = counter`，先讓`result`等於`10`，之後，在進行`counter`的加一運算，因為運算完後沒有再指派回去`result`，所以`result`等於`10`，`counter`等於`11`

```cpp
int counter {10};
int result {0};

result = counter++;

cout << counter << endl; // 11
cout << result << endl; // 10
```

## Mixed Type Expression

基本上C++如果要做任何事情，兩端的變數（或是值）都需要是相同型別，但是有時，我們會不小心將不同型別的東西做操作，這時，C++會做隱性型別轉換，改變其中一方的型別，以完成運算結果

Higher and Lower Types：越高級的型別可以保存較大的值，越低級型別保存較小的值

如果將一些型別由高至低排序，則是：
`long double`→`double`→`float`→`unsigned long`→`long`→`unsigned int`→`int`

當型別由低轉高（Promotion），可以完美的轉換，不會喪失任何東西，但是由高轉低（Demotion）就會發生流失了

`short`與`char`型別會自動轉換為`int`

混合型別的運算：為了讓結果可以運作，所以會讓兩邊的型別相等，於是會發生將其中一方的較低型別，提升到一樣高的高級型別

`2`會先轉換成`2.0`，然後再計算`2.0 * 5.2`
```cpp
2 * 5.2; // 10.4
```

混合型別的指派：指派以左方變數的型別為準，會讓右方變數的型別變成跟左方一樣，如果左方較低，右邊會發生demotion，如果左方較高，右方發生promotion

```cpp
int result {0};
result = 100.2;

cout << result << endl; // 100;
```

顯性型別轉換：利用`static_cast<type>`語法

```cpp
int total_amount {100};
int total_number {8};
double average {0.0};

average = total_amount / total_number;

cout << average << endl; // 12

average = static_cast<double>(total_amount) / total_number;
cout << average << endl; // 12.5
```

舊C++的型別轉換：不論`total_amount`是什麼，都強制轉換為`double`
```cpp
average = (double)total_amount / total_number;
```

## `boolalpha`

`std::boolalpha`是Stream Manipulator

C++中針對布林值的cout`是以`0`或是`1`來輸出，不太方便看，因此可以用`cout << std:: boolalpha;`讓此行以後的布林值都輸出為`false`或是`true`

```cpp
bool result {false};

cout << result << endl; // 輸出0或是1

cout << boolalpha; // 表示這行以後布林值都輸出false或是true
cout << result << endl; // 輸出false或是true

cout << noboolalpha; // 改回0或是1的輸出
cout << result << endl; // 輸出0或是1
```

## Equality Operators

* `==`
* `!=`

## Relational Operators

* `>`
* `>=`
* `<`
* `<=`
* `<=>`：three-way comparion (C++20)

## Logical Operators

* `!`：not
* `&&`：and
* `||`：or

邏輯運算符的優先序：
not（`!`）優先順序高於and（`&&`），and（`&&`）高於or（`||`）

not是單元運算符（unary operator），and跟or是二元運算符（binary operator）

短路求值（short-circuit evaluation）
當C++看到一串邏輯運算符的表達式，只要其中一部分沒有通過，他就會停止，跳過檢查其他剩下的

```cpp
expr1 && expr2 && expr3;
// 一旦發現expr1是false，會直接中斷檢查，expr2跟expr3連看都不看
```

## Compound Assignment

| Operator | Example | Meaning |
| -------- | ------- | ------- |
| `+=`     | `lhs += rhs` | `lhs = lhs + rhs` |
| `-=`     | `lhs -= rhs` | `lhs = lhs - rhs` |
| `*=`     | `lhs *= rhs` | `lhs = lhs * rhs` |
| `/=`     | `lhs /= rhs` | `lhs = lhs / rhs` |
| `%=`     | `lhs %= rhs` | `lhs = lhs % rhs` |
| `>>=`     | `lhs >>= rhs` | `lhs = lhs >> rhs` |
| `<<=`     | `lhs <<= rhs` | `lhs = lhs << rhs` |
| `&=`     | `lhs &= rhs` | `lhs = lhs & rhs` |
| `^=`     | `lhs ^= rhs` | `lhs = lhs ^ rhs` |
| `|=`     | `lhs |= rhs` | `lhs = lhs | rhs` |








