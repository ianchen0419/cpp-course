# Section 14: Operator Overloading

## Operator Overloading

先來看看不使用運算子多載的用法

- 使用函數：`Number result = multiply(add(a, b), divide(c, d));`
- 使用member method：`Number result = (a.add(b)).multiply(c.divide(d));`

使用運算子多載的寫法：
`Number result = (a+b)*(c/d);`

不能被重載的運算符：
- 作用域解析運算符：`::`
- 條件運算符：`:?`
- 指針運算符：`.*`
- 點運算符：`.`
- sizeof運算符：`sizeof`

```cpp
class Mystring {
private:
    char *str; // pointer to a char[] that holds a C-style string
    
public:
    Mystring(); // no-args constructor
    Mystring(const char *s); // overloaded constructor
    Mystring(const Mystring &source); // copy constructor
    ~Mystring(); // desctructor
    void display() const;
    int get_length() const;
    const char *get_str() const;
};

// no-args constructor，宣告一個空的Mystring，會放一個終止字符進去
Mystring::Mystring(): str{nullptr} {
    str = new char[1];
    *str = '\0';
}

// overloaded constructor
Mystring::Mystring(const char *s): str {nullptr} {
    if (s == nullptr) {
        str = new char[1];
        *str = '\0';
    } else {
        str = new char[strlen(s)+1];
        strcpy(str, s);
    }
}

// copy constructor - deep copy
Mystring::Mystring(const Mystring &source): str {nullptr} {
    str = new char[strlen(source.str) + 1];
    strcpy(str, source.str);
}

// desctructor
Mystring::~Mystring() {
    delete [] str;
}

// display method
void Mystring::display() const {
    cout << str << ": " << get_length() << endl;
}

// length getter
int Mystring::get_length() const {
    return strlen(str); // strlen回傳字串長度，但不包含結束字元
}

// string getter
const char *Mystring::get_str() const {
    return str;
}

int main() {
    
    Mystring empty; // no-args constructor
    Mystring larry("Larry"); // overloaded constructor
    Mystring stooge {larry}; // copy constructor
    
    empty.display();
    larry.display();
    stooge.display();
    
    
    return 0;
}

```

## Copy Assignment

接續上一章的Mystring標頭檔：

```cpp
Mystring s1 {"Frank"};
Mystring s2 = s1; // 不是賦值，因為這句跟 `Mystring s2 {s1}` 一樣，所以是初始化語句，因為s2還沒被建立

s2 = s1; // 賦值（assignment）
```

C++預設的重載賦值運算符（`=`），是成員級分配的淺層複製，如果成員沒有原始指針變數，那預設的就夠用了

但是，Mystring有原始指針成員，所以必須改寫為深層拷貝

Copy Assignment的語法是：
```cpp
Type &Type::operator=(const Type &rhs);
```

用法範例：
```cpp
class Mystring {
private:
	char *str;
public:
	Mystring &operator=(const Mystring &rhs);
	// ...
}
```

當我們定義了copy assignment後，C++會將原本的assignment語法（`s2 = s1`），腦補替換成`.operator=()`方法（`s2.operator=(s1)`）

```cpp
s2 = s1;
s2.operator=(s1); // 原本冗長的operator語法，也可改由上面簡單的等號運算符呼叫，提升了撰寫的便利性
```

class實現：
```cpp
// copy assignment
Mystring &Mystring::operator=(const Mystring &rhs) {
    if (this == &rhs) { // 檢查是否相等物件自己指派自己，例如 p1 = p1
        return *this; // 回傳current object
    }
    
    delete [] str; // 重新分配this->str的空間，因為要覆蓋他，如果沒有delete，那原本的那個指針會遺失，造成內存泄露
    
    str = new char[strlen(rhs.str) + 1]; // 深度拷貝
    strcpy(str, rhs.str);
    
    return *this;
}
```

實際應用：
```
Mystring a {"Hello"}; // overloaded constructor
Mystring b; // no-args constructor
b = a; // copy assignment, 等同於 b.operator=(a);

b = "This is a test"; // 等同於 b.operator=("This is a test");
```

## Move Assignment

```cpp
s1 = Mystring {"Frank"}; // move assignment，Frank作為臨時右值被建立，然後被move assignment到s1上
```

Move Assignment的prototype
```cpp
class Mystring {
public:
	Mystring(Mystring &&source); // move constructor
	Mystring &operator=(Mystring &&rhs); // move assignment
};
```

實際應用
```cpp
s1 = Mystring {"Frank"}; // 呼叫move assignment
s1 = "Joe"; // 呼叫move assignment
```

move assignment的實現
```cpp
// move constructor
Mystring::Mystring(Mystring &&source): str(source.str) {
    source.str = nullptr;
    cout << "Move constructor used" << endl;
}

// move assignment
Mystring &Mystring::operator=(Mystring &&rhs) {
    if (this == &rhs) {
        return *this;
    }
    
    delete [] str;
    str = rhs.str; // 偷取指針
    
    rhs.str = nullptr; // 清空rhs
    
    return *this; // 回傳現在物件
}
```

## Overloading Operators as Member Functions

Unary operators：`++`、`--`、`-`、`!`

```cpp
Number Number::operator-() const;
Number Number::operator++(); // pre-increment
Number Number::operator++(int); // post-increment
bool Number::operator!() const;

Number n1 {100};
Number n2 = -n1; // n1.operator-();
n2 = ++n1; // n1.operator++(); pre-increment
n2 = n1++; // n1.operator++(int); post-increment
```


範例：在Mystring加上`-`讓其轉小寫

prototype：
```cpp
class Mystring {
public:
	Mystring operator-() const; // unary overloading operator
};
```

實現：
```cpp
// unary overloading operator
Mystring Mystring::operator-() const {
    char *buff = new char[strlen(str + 1)];
    strcpy(buff, str);
    
    for (size_t i=0; i < strlen(bugg); i++) {
        buff[i] = tolower(buff[i]);
    }
    
    Mystring temp {buff};
    
    delete [] buff;
    
    return temp;
}
```

