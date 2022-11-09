# cpp-course

## Section 5: Structure of a C++ Program

### Overview of Structure

`<<`：stream insertion operator，將右方的東西放入左邊的stream裡面

```cpp
std:cout << "Hello World";
```

`>>`：string extraction operator，從Console面板中捕獲用戶輸入值，並存入右邊的變數中

```cpp
std:cin >> favorite_number;
```

`::`：scope resolution operator


### Preprocessor Directives 預處理器指令

由`#`開頭的關鍵字叫做預處理器指令，例如：

```cpp
#include <iostream>
#include "myfile.h"

#if
#elif
#else
#endif

#ifdef
#ifndef
#define
#undef

#line
#error
#pragma
```

最常用的預處理器指令是`#include`

### The main() function

C++程式一定要有main() function，當電腦執行C++程式時，系統會調用main() function，執行main() function裡面的程式，如果main() function的return為`0`，則系統會停止執行C++

常見的C++程式有兩種版本：

第一種版本在執行C++程式時，不需要帶任何參數

```cpp
int main() {
  // code
  
  return 0;
}
```

```zsh
program.exe
```

第二種版本，在執行C++程式時，需要指定參數，`argc`是argument count，`argv`是argument vector

```cpp
int main(int argc, char *argv[]) {
  // code
  
  return 0;
}
```

```zsh
program.exe argument1 argument2
```
