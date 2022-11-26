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

C-Style strings使用空字符`\0`（null character）來標示結束，因為`null`代表著zero，因此C-Style strings又稱作zero/null terminated strings

就像我們不能修改C++陣列的常數一樣，C-Style Strings也不能被修改，因此他是常量變數

宣告C-Style Strings：

```cpp
char name[] = {"Frank"};
```














