# Section 5: Structure of a C++ Program

## Overview of Structure

`<<`：stream insertion operator，將右方的東西放入左邊的stream裡面

```cpp
std:cout << "Hello World";
```

`>>`：string extraction operator，從Console面板中捕獲用戶輸入值，並存入右邊的變數中

```cpp
std:cin >> favorite_number;
```

`::`：scope resolution operator

`cin`在必需接著`cout`之後使用，如果在一開始就使用了`cin`，雖然程式能夠成功執行，但是會當掉，建議先一開始用`cout`敘述輸入的題目，再接著用`cin`


## Preprocessor Directives 預處理器指令

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

## The main() function

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

## Namespaces

命名空間是為了解決變數命名重複的問題，比如A公司跟B公司同時維護同一份C++程式，這兩間公司都命名到了`people`這個變數名稱，則會造成變數污染跟衝突，所以利用命名空間，讓A公司定義`A:people`，而B公司定義`B:people`，就不會衝突了

`using namespace`可以用來簡化命名空間的寫法
```diff cpp
#include <iostream>

+using namespace std;

int main() {

-	std::cout << "Hello" << std::endl;
+	cout << "Hello" << endl;
	
	return 0;
}
```

但是，如果是在維護大型專案，上面的方法仍然可能會造成命名衝突，因此可以更精確地使用`using`：
```diff cpp
#include <iostream>

-using namespace std;
+using std::cout;
+using std::endl;

int main() {

	cout << "Hello" << endl;
	
	return 0;
}
```

## Input and Output

`cin`的buffer：在下面這個範例，執行程式後，會跳出字串：「Enter a first interger: 」，此時輸入`100 200`，則程式會立刻終止，並且回傳：「Enter a second interger: You entered 100 and 200」

```cpp
#include <iostream>

using namespace std;

int main(int argc, const char * argv[]) {
	int num1, num2;

	cout << "Enter a first interger: ";
	cin >> num1;

	cout << "Enter a second interger: ";
	cin >> num2;

	cout << "You entered " << num1 << " and " << num2 << endl;


	return 0;
}
```

原因是，當使用者輸入了`100 200`，因為Input Stream解析整數的分隔符號是空格，所以Input Stream實際上接收到了兩個整數：`100`跟`200`，並且它將這兩個整數，都存入緩衝區裡，而當按下Enter鍵，讓程式繼續執行後，`cin`會從緩衝區找出第一個整數：`100`，並且存入`num1`變數中，然後再繼續從緩衝區找出第二個整數：`200`，存入`num2`變數中

再來看另一個範例：

```cpp
#include <iostream>

using namespace std;

int main(int argc, const char * argv[]) {
    
    int num1, num2;
    
    cout << "Enter 2 intergers seperated with a space: ";
    cin >> num1 >> num2;
    
    cout << "You entered " << num1 << " and " << num2 << endl;
    
    
    return 0;
}
```

在這個範例中，只要輸入了由空格區分開的兩組數字，比如`100 200`，C++就會將這兩組數字分別存入`num1`與`num2`當中
