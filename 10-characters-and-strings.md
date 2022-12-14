# Section 10: Characters and Strings

## Character Functions

C++有一個library可以用來測試跟character有關的函示，叫做`cctype`，主要功能如下

1. 測試character是否英文 / 數字 ...
2. 提供轉換大寫小寫的method

測試方法介紹：

| 方法 | 意涵 |
| ----- | ----- |
| `isalpha(c)` | 若`c`為字母，回傳true|
| `isalnum(c)` | 若`c`為字母或是數字，回傳true|
| `isdigit(c)` | 若`c`為數字，回傳true|
| `islower(c)` | 若`c`為小寫字母，回傳true|
| `isprint(c)` | 若`c`為可輸出的字母，回傳true|
| `ispunct(c)` | 若`c`為標點符號，回傳true|
| `isupper(c)` | 若`c`為大寫字母，回傳true|
| `isspace(c)` | 若`c`為空白或是tab符號等等空白字元，回傳true|

轉換大小寫方法介紹：

| 方法 | 意涵 |
| ----- | ----- |
| `tolower(c)` | 將`c`轉換成小寫|
| `toupper(c)` | 將`c`轉換成大寫|

## C-Style Strings

C-Style strings是一連串的字元（Character），像陣列般地排好，所以可以用陣列的所以方式來找到中間的字元



就像我們不能修改C++陣列的常數一樣，C-Style Strings也不能被修改，因此他是常量變數

宣告C-Style Strings：

```cpp
char name[] = {"Frank"};
```

如果指定C-Style字串數量的話，會發現明明"Frank"是5個字，卻必須要指定6個單位，這是因為第6個位置，要擺放標示結束的空字元`\0`

```cpp
char name[6] = {"Frank"};
```

C-Style strings使用空字符`\0`（null character）來標示結束，因為`null`代表著zero，因此C-Style strings又稱作zero/null terminated strings

錯誤範例：只宣告C-Style String的數量（長度），卻不初始化

```cpp
char name[5];

cout << name << endl; // 可能出現垃圾值
```

會無法預期C++將什麼資料寫入了`name`這個C-Style string裡面去，因此正確的宣告空C-Style字串的寫法應為：

```cpp
char name[5] {};
```

而這種一開始宣告空C-Style字串，並且在日後補字串的狀況，不能直些使用習以為常的等號`=`再賦值

```cpp
char name[5] {};

name = "Frank"; // 錯誤，因為"Frank"沒有終止字元，所以內存數量對不上
```

這種狀況，需要用`strcpy`函示處理

```cpp
char name[5] {};

strcpy(name, "Frank"); // 成功
```

## 引入`<cstring>`

`<cstring>`包含一些操作C-Style string的便利函示

- `strcpy()`：複製字串值給一個C-Style字串變數
- `strcat()`：連接C-Style字串變數跟字串
- `strlen()`：字串的長度
- `strcmp()`：比較兩個字串的長度

```cpp
char name[80];
    
strcpy(name, "Hello ");

cout << name << endl; // Hello

strcat(name, "there ");

cout << name << endl; // Hello there

cout << strlen(name) << endl; // 12

cout << strcmp(name, "Another") << endl; // 回傳值為7，7大於0，表示str1大於str2
```

## 引入`<cstdlib>`

包含將C-Style字符串（具有`\0`空白終止符）轉為其他類型的函示，例如integer、float、long等等

## C-Style字串 練習範例

本次練習需要引入3個函示庫

```cpp
#include <iostream>
#include <cstring>
#include <cctype>
```

```cpp
char name[20] {};

cout << "Your name: ";
cin >> name;

cout << "Your name is " << name << " and has " << strlen(name) << " characters." << endl;
```

`strlen()`會返回`size_t`，實際上是一個無符號整數，在32位元的電腦是`unsigned int`，在64位元的電腦是`unsigned long`

輸入帶有空格的內容：
在上面這個範例中，會發現如果輸入了`Harry Potter`，實際上`name`只會存下`Harry`，如果想要能夠輸入帶有空格的，需要使用`cin.getline()`

```cpp
char name[20] {};

cout << "Your name: ";
cin.getline(name, 20);

cout << "Your name is " << name << " and has " << strlen(name) << " characters." << endl;
```

`cin.getline()`的第一個參數填要存的變數名稱，第二個參數填入最大限制的字元數，比如上面範例填了`20`，那麼一旦當使用者輸入後按下Enter，或是輸入了超過20個字，`cin`就會停止存了

利用for迴圈將C-Style字串轉大寫

```cpp
char name[20] {};

cout << "Your name: ";
cin.getline(name, 20);

for(size_t i {0}; i < strlen(name); i++) {
    if(isalpha(name[i])) {
        name[i] = toupper(name[i]);
    }
}

cout << name << endl;
```


比較轉大寫的C-Style字串跟沒有轉大寫的C-Style字串：`strcmp()`可以比較兩個C-Style字串，判斷誰比較多，誰必較少，如果兩個字串完全一樣，則返回`0`。
但是兩個字串，字母都一樣，一個都是大寫，另一個都是小寫的話，`strcmp()`會依照ASKII排序給積分

```cpp
char name[20] {};

cout << "Your name: ";
cin.getline(name, 20);

// 暫存小寫字串
char temp[20] {};
strcpy(temp, name);


// 轉大寫
for(size_t i {0}; i < strlen(name); i++) {
    if(isalpha(name[i])) {
        name[i] = toupper(name[i]);
    }
}
    
cout << strcmp(temp, name) << endl; // 32
cout << strcmp(name, temp) << endl; // -32
```

## C++ String

- C++ String因為是標準函式庫，所以型別全名是`std::string`
- 當使用C++ String時，需要引入`<string>`函示庫
- 跟C-Style字串一樣是連續地存放在記憶體中
- 與C-Style字串不同的是，C-Style字串是固定記憶體大小（像陣列一樣不能事後改長度），但是C++字串是動態記憶體大小
- 可以與常用的操作符連用，例如`+`、`=`、`+=`
- 可以轉換型別成C-Style字串

## 宣告與初始化C++字串
在宣告字串前需要先設定這些：

```cpp
#include <iostream>
#include <string>

using namespace std;
```

自動被初始化：與C-Style字串不同，C++字串會自動被初始化，會分配空的值進去，所以不用擔心垃圾值的問題
```cpp
string name; // 沒有垃圾值
```

宣告字串與複製字串（到不同記憶體）
```cpp
string name {"Frank"}; // Frank
string copy {name}; // Frank
```

擷取
```cpp
string wholeName {"Frank"};

string name {"Frank", 3}; // Fra
string name2 {wholeName, 0, 2}; // Fr
```

重複
```cpp
string name(6, "X"); // XXXXXX
```

## C++字串賦值

一開始宣告為空，之後再賦值

```cpp
string s1;
s1 = "C++ Rocks!";
```

一開始宣告當下就賦值

```cpp
string s2 {"Hello"};
```

## C++字串的連結

用等號`=`串連

## C++字串的索引定位

跟向量一樣，有2種方式可以做

```cpp
string name {"Frank"};

cout << name[0] << endl; // F
cout << name.at(0) << endl; // F
```

搭配Range-based for loop實作的小練習

```cpp
string name {"Frank"};
    
for(char letter: name) {
    cout << letter << endl;
}

// F
// r
// a
// n
// k
```

## C++字串的比較

直接使用操作符比較：`==`、`!=`、`>`、`>=`、`<`、`<=`

除了可比較兩個C++字串，也能比較C++字串與C-Style字串

## C++字串的`substr()`

可以從字串中擷取想要的

```cpp
string s1 {"This is a test"};
    
cout << s1.substr(0, 4) << endl; // This
cout << s1.substr(5, 2) << endl; // is
cout << s1.substr(10, 4) << endl; // test
```

## C++字串的`find()`

```cpp
string s1 {"This is a test"};
    
// 查詢字串
cout << s1.find("This") << endl; // 0
cout << s1.find("test") << endl; // 10

// 從第4個字元開始查
cout << s1.find("is", 4) << endl; // 5

// 如果查無此人，返回no position
cout << s1.find("XX") << endl; // string::npos
```

## C++字串的`erase()`跟`clear()`

```cpp
string s1 {"This is a test"};
    
cout << s1.erase(0, 5) << endl; // is a test

s1.clear(); // empty string
```

## C++字串的`length()`

返回長度

```cpp
string s1 {"This is a test"};
    
cout << s1.length() << endl; // 14
```

## C++字串的input

cin的input用法
```cpp
string name;
    
cout << "Please input your name: ";
cin >> name;

cout << name << endl;
```

直接用cin一樣會用如果輸入空白後面的文字就遺失的問題，為了避免，可以使用`getline()`提取輸入

```cpp
string name;
    
cout << "Please input your name: ";
getline(cin, name);

cout << name << endl;
```

`getline()`的第二種變體：終止字元，第三個變數是終止字元，如果用戶輸入到那個字元，則cin就會結束提取

```
string name;
    
cout << "Please input your name: ";
getline(cin, name, 'x'); // 輸入「Harry Potterx」

cout << name << endl; // Harry Potter
```


## C++字串的範例

本節範例需要引入`<iostrean>`與`<string>`與namespace

```cpp
#include <iostream>
#include <string>

using namespace std;
```

查詢輸入的單字
```cpp
string wholeText =
    "Lorem Ipsum is simply dummy text of the printing and typesetting industry. \nLorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. \nIt has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. \nIt was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.";
    
string searchWord;

cout << "Please input your search word: ";
cin >> searchWord;

size_t position = wholeText.find(searchWord);

if(position == string::npos) {
    cout << "Sorrt, we don't found " << searchWord << endl;
} else {
    cout << "Found" << searchWord << "at position of " << position << endl;
}
```

