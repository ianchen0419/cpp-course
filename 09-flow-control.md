# Section 9: Flow Control

## If Statement

如果內容只有一句，可以用縮排
```cpp
if(num > 10)
	num++;
```

如果內容超過一句，可以加大括號
```cpp
if(num > 10) {
	num++;
	cout << "foo";
}
```

## Block Statement

用大括號包起來的述句，如果在Block裡面宣告變數，則變數會變成區域變數，無法從外部（Global）
```cpp
{
	// local variable
}
```

## If-else Statement

如果內容只有一句：
```cpp
if (num > 10)
	num++;
else
	num--;
```

## Switch Statement

一般用法，建議每個條件下面都接`break;`

```cpp
enum Color {
	red, green, blue
};

Color screen_color {green};

switch (screen_color) {
	case red:
		cout << "red" << endl;
		break;

	case blue:
		cout << "blue" << endl;
		break;

	case green:
		cout << "green" << endl;
		break;

	default:
		cout << "should never execute" << endl;
}
```

兩個條件合併輸出相同：
```cpp
switch (screen_color) {
	case red:
	case blue:
		cout << "red or blue" << endl;
		break;
	

	case green:
		cout << "green" << endl;
		break;

	default:
		cout << "should never execute" << endl;
}
```

## Conditional Operator

```cpp
int a {10};
int b {20};
int result {};

result = (a < b) ? a : b;
```

## for Loop

```cpp
int i {0};
for (i = 1; i <= 5; ++i)
	cout << i << endl;
```

comma operator，較不常見

```cpp
for (int i {1}, j {5}; i <= 5; ++i, ++j) {
	cout << i << " * " << "j" << ": " << i * j << endl;
}
    
//    1 * j: 5
//    2 * j: 12
//    3 * j: 21
//    4 * j: 32
//    5 * j: 45
```

與Vector搭配利用，因為Vector不存在負數的索引，所以`for`宣告使用`unsigned`更有效率

```cpp
vector<int> nums {10, 20, 30, 40, 50};
    
for(unsigned i {0}; i < nums.size(); ++i) {
	cout << nums[i] << endl;
}
```

## Range-based for Loop

C++11添加的新特性

```cpp
int scores [] {100, 90, 97};
    
for (int score: scores) {
	cout << score << endl;
}

// 100
// 90
// 97
```

也可以省略宣告變數的類型，改用`auto`關鍵字宣告
```diff cpp
-for (int score: scores) {
+for (auto score: scores) {
	cout << score << endl;
}
```

## while Loop

while loop是pre-test loop，會在執行本體之前檢查條件

```cpp
int i {1};
    
while (i <= 5) {
	cout << i << endl;
	++i;
}

// 1
// 2
// 3
// 4
// 5
```

## do while Loop

do while lop會在執行本體後再檢查條件，所以程式至少會執行一次

```cpp
int number {0};
    
do {
	cout << "Enter an interger between 1 and 5: ";
	cin >> number;
} while (number <= 1 || number >= 5);

cout << "Thanks" << endl;
```

## continue and break

continue跟break可以用在任何C++的loop內

continue：跳過後面的內容，直接跑到下一個loop的頭執行
break：跳過後面的內容，並且終止整個迴圈

```cpp
vector<int> values {1, 2, -1, 3, -1, -99, 7, 8, 10};
    
for(auto val: values) {
	if (val == -99) {
		break;
	} else if (val == -1) {
		continue;
	} else {
		cout << val << endl;
	}
}

// 1
// 2
// 3
```

## Infinite Loop

for的無限循環的範例：因為`for()`括號內的3個陳述句，不是必填，所以寫成空的是可以的
```cpp
for(;;) {
	cout << "forever loop" << endl;
}
```

while的無限循環範例
```cpp
while(true) {
	cout << "forever loop" << endl;
}
```






