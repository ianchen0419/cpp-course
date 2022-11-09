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
