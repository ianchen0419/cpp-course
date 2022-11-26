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
但是兩個字串，字母都一樣，一個都是大寫，另一個都是小寫的話，`strcmp()`會依照字典排序給積分

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






