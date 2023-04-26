# Section 13: Classes and Objects

## Object-Oriented Programming

Procedural Programming（過程式編程）是函數的集合，他有一些缺點：像是函數需要知道數據結構、當程式龐大複雜時，難以手動追蹤成千上萬的函數、缺乏重用性

## Declaring Classes and Creating Objects

使用`class`關鍵字宣告class

```cpp
class Player {
	// attributes
	std::string name {"Default Name"}; // 初始化
	int health;
	int xp;

	// methods
	void tald(std::string text_to_say);
	bool is_dead();
};
```

用`Player`建立物件
```cpp
Player frank;
Player hero;
```

建立物件的指針：建立指針變數enemy，指向一個在heap上分配的Player物件
```cpp
Player *enemy {nullptr};
enemy = new Player;

delete enemy;
```

建立一個Array，裡面包含frank跟hero
```cpp
Player frank;
Player hero;

Player players[] {frank, hero};
```

建立一個Vector，裡面包含frank跟hero
```cpp
Player frank;
Player hero;

vector<Player> players_vec {frank};
players_vec.push_back(hero);
```

## Accessing Class Members

使用dot operator訪問
```cpp
Account frank_account;
    
frank_account.balance;
frank.account.deposit(100.00);
```

指針的物件對象，需要先反指針，在使用dot operator
```cpp
Account *frank_account = new Account();
    
(*frank_account).balance;
(*frank_account).deposit(100.00);
```

或者使用指針運算符：arrow operator
```cpp
Account *frank_account = new Account();
    
frank_account -> balance;
frank_account -> deposit(100.00);
```

## Class Member Access Modifiers

- public
- private：只有成員（members）或是朋友（friends）才能存取
- protected：跟private很像，但是在繼承物件對象上跟private不一樣

```cpp
class Player {
private:
    string name;
    int health;
    int xp;
    
public:
    void talk(string text_to_say);
    bool is_dead();
};
```

如果嘗試存取private的屬性，會出錯（讀取跟寫入都會錯）
```cpp
Player frank;
frank.name = "Frank"; // Compile Error
frank.health = 1000; // Compile Error
frank.talk("Ready to battle"); // OK

Player *enemy = new Player();
enemy->xp = 100; // Compile Error
enemy->talk("I will hunt you down") // OK

delete enemy;
```

## Member Methods

C++中有很多種方式可以做Class Method

Class宣告內定義：
```cpp
class Account {
    
private:
    double balance;
    
public:
    void set_balance(double bal) {
        balance = bal;
    }
    
    double get_balance() {
        return balance;
    }
    
};
```

Class宣告外定義，使用作用域解析運算符`::`
```cpp
class Account {
    
private:
    double balance;
    
public:
    void set_balance(double bal);
    double get_balance();
    
};

void Account::set_balance(double bal) {
    balance = bal;
}

double Account::get_balance() {
    return balance;
}
```

分離文件（Seperating Spec）定義

建立include header檔案：`Account.h`

```cpp:Account.h
class Account {
    
private:
    double balance;
    
public:
    void set_balance(double bal);
    double get_balance();
    
};
```

為了避免不小心引入太多次Account.h，採用Include Guards的預處理器寫法（就算不小心引入多次，都只會當成引入一次處理）

```diff cpp:Account.h

+#ifndef _ACCOUNT_H_
+#define _ACCOUNT_H_

class Account {
    
private:
    double balance;
    
public:
    void set_balance(double bal);
    double get_balance();
    
};

+#endif

```

Pragma Once也有同樣效果（但是要看使用的編譯器有沒有支援）

```diff cpp:Account.h

+#pragma once

class Account {
    
private:
    double balance;
    
public:
    void set_balance(double bal);
    double get_balance();
    
};

```

定義好Account.h後，再開一個新檔案：`Account.cpp`，定義method

```cpp:Account.cpp
#include "Account.h"

void Account::set_balance(double bal) {
    balance = bal;
}

double Account::get_balance() {
    return balance;
}
```

- include後面接尖括號的寫法（`#include <vector>`），用於引入系統頭文件
- include接雙引號（`#include "Account.h"`），用於引入本地頭文件


最後，回到主文件`main.cpp`

```diff cpp:main.cpp
#include <iostream>
+#include "Account.h"

using namespace std;

int main() {
	
+	Account frank_account;
+	frank_account.set_balance(1000.00);
+	double bal = frank_account.get_balance();
+	
+	cout << bal << endl; // 1000.00

	return 0;
}
```

## Constrcutor

- 構造函數（constrcutor）是物件的特殊成員
- 在物件建立期間執行
- 適合用在初始化
- 跟class名稱同一個名字
- 無回傳值
- 可以被重載

構造函數重載：
```cpp
class Player {
private:
    string name;
    int health;
    int xp;
    
public:
    // overloaded constructors
    Player();
    Player(string name);
    Player(string name, int health, int xp);
};
```

析構函數（Destructor）
當物件被銷毀時，調用析構函數，可以用來釋放內存，無法被重載（只能有一個析構函數）

```cpp
class Player {
private:
    string name;
    int health;
    int xp;
    
public:
    // overloaded constructors
    Player();
    Player(string name);
    Player(string name, int health, int xp);
    
    // destructor
    ~Player();
};
```

析構函數被呼叫的時機(1)：作用域結束後
```cpp
{
Player p1;
Player p2("Frank", 100, 4);
Player p3("Hero");
Player p4("Villain");
} // 作用域一結束後，4個destructor被呼叫
```

析構函數被呼叫的時機(1)：delete + 指針變數

```cpp
Player *enemy = new Player("Enemy", 100, 0);

delete enemy; // destructor被呼叫
```

如果定義class時沒有特別指定constructor跟desctructor，C++將會定義默認的constructor跟desctructor，但他們都是空的

## Default Constructor
默認構造函數，也稱為無參數構造函數（no-args constructor），當沒有定義構造函數時，由系統自己生成

沒有定義構造函數：因為默認的構造函數什麼事也不會做，所以物件成員可能會有垃圾值
```cpp
class Account {
private:
    string name;
    double balance;
    
public:
    bool withdraw(double amount);
    bool deposit(double amount);
};

    
Account frank_account; // 使用默認構造函數
Account jim_account; // 使用默認構造函數

Account *mary_account = new Account; // 也使用默認構造函數
delete mary_account;

return 0;
```

有定義構造函數（推薦做法）：
```diff cpp
class Account {
private:
    string name;
    double balance;
    
public:
+   Account() {
+       name = "None";
+       balance = 0.0;
+   }
    
    bool withdraw(double amount);
    bool deposit(double amount);
};
```

注意，一旦我們有定義了自己的構造函數，C++就不會幫忙定義空的默認函數，如果需要空的那個，就要自己加上去
```
class Account {
private:
    string name;
    double balance;
    
public:
    Account(string name_val, double bal) {
        name = name_val;
        balance = bal;
    }
    
    bool withdraw(double amount);
    bool deposit(double amount);
};

Account frank_account; // 隱式調用：Error
Account bill_account {"Bill", 1000.00}; // 顯式調用：OK
```

## Overloaded Constructors

定義3種不同的構造函數
```cpp
class Player {
private:
    string name;
    int health;
    int xp;
    
public:
    // overloaded constructors
    Player();
    Player(string name_val);
    Player(string name_val, int health_val, int xp_val);
};

Player::Player() {
    name = "None";
    health = 0;
    xp = 0;
}

Player::Player(string name_val) {
    name = name_val;
    health = 0;
    xp = 0;
}

Player::Player(string name_val, int health_val, int xp_val) {
    name = name_val;
    health = health_val;
    xp = xp_val;
}
```

實例化：
```cpp
Player empty; // None, 0, 0
    
Player hero {"Hero"}; // Hero, 0, 0
Player villain {"Villain"}; // Villain, 0, 0

Player frank {"Frank", 100, 4}; // Frank, 100, 4

Player *enemy = new Player("Enemy", 1000, 0) // Enemy, 100, 0
delete enemy
```

## Constructor Initialization Lists

比前面的Constructor更高效，在物件建立的瞬間初始化物件成員屬性

```cpp
class Player {
private:
    string name;
    int health;
    int xp;
    
public:
    // overloaded constructors
    Player();
};
```

一般的constructor作法：先將成員初始化為垃圾值，再重新賦值
```cpp
Player::Player() { // 將name、health、xp初始化為垃圾值
    name = "None"; // 將name重新賦值為None
    health = 0; // 將health重新賦值為0
    xp = 0; // 將xp重新賦值為0
}
```

高效的constructor initialization list，直接將成員初始化為我們定義的值
```cpp
Player::Player(): name {"None"}, health {0}, xp {0} {
    
}
```

`name {"None"}, health {0}, xp {0}`的順序可以不用照定義Class成員的順序來，但是初始清單的順序，會決定物件初始化的順序

範例：改寫前面3個構造函數
```diff cpp
-Player::Player() {
-    name = "None";
-    health = 0;
-    xp = 0;
-}

+Player::Player(): name {"None"}, health {0}, xp {0} {
+    
+}

-Player::Player(string name_val) {
-    name = name_val;
-    health = 0;
-    xp = 0;
-}

+Player::Player(string name_val): name {name_val}, health {0}, xp {0} {
+
+}

-Player::Player(string name_val, int health_val, int xp_val) {
-    name = name_val;
-    health = health_val;
-    xp = xp_val;
-}

+Player::Player(string name_val, int health_val, int xp_val): name {name_val}, health {health_val}, xp {xp_val} {
+
+}
```

## Delegating Constructors

委託構造函數（Delegating Constructors）：構造函數可以在初始化列表中呼叫另一個構造函數，以期減少重複代碼

這是原本的Player的初始化列表，彼此之間非常相像，只差在值不同
```cpp
Player::Player(): name {"None"}, health {0}, xp {0} {
    
}

Player::Player(string name_val): name {name_val}, health {0}, xp {0} {
    
}

Player::Player(string name_val, int health_val, int xp_val): name {name_val}, health {health_val}, xp {xp_val} {
    
}
```

首先，將最複雜的構造函數移到最上方，利用他來完成其他委託構造函數
```diff cpp
+Player::Player(string name_val, int health_val, int xp_val)
+	: name {name_val}, health {health_val}, xp {xp_val} {
+    
+}

Player::Player(): name {"None"}, health {0}, xp {0} {
    
}

Player::Player(string name_val): name {name_val}, health {0}, xp {0} {
    
}

-Player::Player(string name_val, int health_val, int xp_val): name {name_val}, health {health_val}, xp {xp_val} {
-    
-}
```

接者，將後面兩個構造函數，改寫為委託構造函數（利用第一個構造函數的呼叫，達成同樣目的）

```cpp
Player::Player(string name_val, int health_val, int xp_val)
    : name {name_val}, health {health_val}, xp {xp_val} {
    
}

-Player::Player(): name {"None"}, health {0}, xp {0} {
+Player::Player(): Player {"None", 0, 0} {
    
}

-Player::Player(string name_val): name {name_val}, health {0}, xp {0} {
+Player::Player(string name_val): Player {name_val, 0, 0} {
    
}
```

## Constructor Parameters and Default Values

構造函數的預設參數，宣告在Class裡面

```cpp
class Player {
private:
    string name;
    int health;
    int xp;
    
public:
    // 有默認值的構造函數
    Player(string name_val = "None", int health_val = 0, int xp_val = 0);
};

// 構造函數的初始化，不用寫入默認值，編譯器會自己幫忙帶入
Player::Player(string name_val, int health_val, int xp_val)
: name {name_val}, health {health_val}, xp {xp_val} {
    
}
```

## Copy Constructor

當Object被複製時，C++會建立一個全先的Object，然後拷貝所有資料過去，因此會調用Copy Constructor（拷貝構造函數），我們可以定義自己的拷貝構造函數，或如果沒有定義的話，C++會使用預設的拷貝構造函數

物件被拷貝的情境(1)：Pass object by value
函數參數預設是pass-by-value

```cpp
void display_player(Player p) {
    // p是hero的一份拷貝
    // use p
    // p的descructor會被呼叫
}

int main() {

    Player hero {"Hero", 100, 20};
    
    display_player(hero);
    
    return 0;
}
```

物件被拷貝的情境(2)：Return object by value

```cpp
Player create_super_enemy() {
    Player an_enemy {"Super Enemy", 1000, 1000};
    return an_enemy; // an_enemy的拷貝被回傳
}

int main() {
    
    Player enemy;
    
    enemy = create_super_enemy();
    
    return 0;
}
```

物件被拷貝的情境(3)：Constructor one object based on another

```cpp
Player hero {"Hero", 100, 100};
    
Player another_hero {hero}; // hero的拷貝被做出來
```

預設的拷貝構造函數：如果沒有特別定義的話，拷貝物件時，會自動使用預設拷貝，他會遍歷所有物件成員，然後複製到目標對象（memberwise）

如果某個物件成員是一個原始指針，預設拷貝會複製指針去新的目標，而不是複製它所指向的數據（淺層拷貝）

建議做法：
- 如果Class的成員有用到指針（raw data pointer member），寫自己的拷貝構造函數，不要用預設的
- 用`const reference`實現拷貝構造函數
- 使用標準庫模板STL Classes
- 避免在物件成員中定義原始指針，或是只用智能指針替代

Copy Constructor：
使用reference（`&source`）的目的是為了跟默認拷貝區別，默認拷貝是by-value
使用常量宣告（`const Player`），因為目的是複製，不是修改源碼，使用const防止對原始對象進行修改

```cpp
Player::Player(const Player &source)
: name {source.name}, health {source.health}, xp {source.xp} {
    
}
```

甚至可以用delegate定義copy constructor
```diff cpp
Player::Player(const Player &source)
-: name {source.name}, health {source.health}, xp {source.xp} {
+: Player {source.name, source.health, source.xp} {
    
}
```

## Shallow Copying with Copy Constructor

淺層拷貝是預設的copy constructor的行為，他會對所有成員進行複製，如果其中一個成員是指針變數，複製過去一樣指向同一個內存位置，當原始物件or拷貝過去的新物件當中任一被銷毀時，另一個也會遭殃

```cpp
class Player {
public:
    string name;
    int health;
    int xp;
    string *name_ptr;
    
    Player(string name_val = "None", int health_val = 0, int xp_val = 0);
};


Player::Player(string name_val, int health_val, int xp_val)
: name {name_val}, health {health_val}, xp {xp_val} {
    name_ptr = &name;
}



int main() {
    
    Player hero {"Hero", 100, 100};
    
    cout << &hero.name << endl; // 0x16fdff1f0
    cout << hero.name_ptr << endl; // 0x16fdff1f0
    
    Player another_hero {hero}; // hero的拷貝被做出來
    
    cout << &another_hero.name << endl; // 0x16fdff1a0
    cout << another_hero.name_ptr << endl; // 0x16fdff1f0
    
    return 0;
}
```

範例：`data(source.data)`是memberwise copy，也能夠寫成`data {source.data}`
```cpp
class Shallow {
public:
    int *data; // 指針變數
//public:
    Shallow(int d); // constructor
    Shallow(const Shallow &source); // copy constructor
    ~Shallow(); // descructor
};

// implement - constructor
Shallow::Shallow(int d) {
    data = new int; // allocate storage
    *data = d;
}

// implement - descructor
Shallow::~Shallow() {
    delete data; // free storage
    cout << "Destructor freeing data" << endl;
}

// implement - copy，雖然我們自己寫的，但是跟預設拷貝是一樣的，
Shallow::Shallow(const Shallow &source): data(source.data) {
    cout << "Copy constructor - shallow" << endl;
    cout << source.data << endl;
}

int main() {
    
    Shallow s {100};
    Shallow b {s}; // copying
    
    // 解構b時會Crash
    
    return 0;
}
```

## Deep Copying with Copy Constructor

Deep copy：複製指針變數所指向的內存位置的**值**，需要先為深度拷貝分配儲存空間，再執行拷貝

```cpp
class Deep {
private:
    int *data; // pointer
    
public:
    Deep(int d); // constructor
    Deep(const Deep &source); // copy constructor
    ~Deep(); // destructor
};

// implement - constructor
Deep::Deep(int d) {
    data = new int; // allocate storage
    *data = d;
}

// implement - destructor
Deep::~Deep() {
    delete data; // free storage
    cout << "Destructor freeing data" << endl;
}

// implement - constructor
Deep::Deep(const Deep &source) {
    data = new int; // allocate storage
    *data = *source.data;
    cout << "Copy constructor - deep" << endl;
}
```

深度拷貝的拷貝構造函數，也能用delegate改寫：
```
// implement - constructor
Deep::Deep(const Deep &source): Deep {*source.data} {
    cout << "Copy constructor - deep" << endl;
}
```

深度拷貝的Class，複製之後經過銷毀，不會Crash
```cpp
Deep obj1 {100};
Deep obj2 {obj1};

// won't crash
```

## Move Constructors

移動構造函數（move constructor）是C++11引入的新特性

L值跟R值的複習：
```cpp
int total {0};
total = 100 + 200;
```

1. `100 + 200`被算成`300`，存在無名臨時值（unamed temp value，不可尋址，為R值）
2. `300`被賦值到變數`total`中
3. 暫存變數銷毀

如果臨時值只是數字的計算，那成本還好，但是如果是物件的話，一遍又一遍的做copy constructor，會花費大量成本，如果又寫成深度拷貝，那開銷更大了

為了解決這個問題，移動構造函數誕生了，R值是移動語義地址的對象

在程式執行過程中，物件的拷貝會經常發生，每次發生時，都會自動調用拷貝構造函數
如果Class有原始指針成員，那需要提供自製深度拷貝的拷貝構造函數，但深度拷貝很貴

移動構造函數，移動物件，而非複製他

如果沒提供移動構造函數，會調用複製函數，但是如果用了原始指針成員，建議提供移動函數

執行Debugger時可能會看不到移動函數的運作，因為編譯器的優化：Copy ellision（複製錯覺），在編譯過程中，編譯器消除不必要的複製，例如提供返回值時，會先在stack中建立返回值的副本，然後再賦值過去，而編譯器會省略建立返回值副本這件事（RVO - Return value optimization）

R值引用（R-value reference）
R值引用對臨時值對象的引用，符號是`&&`

（複習：L值引用是異名的概念）

```cpp
int x {100};

int &l_ref = x; // L值引用

l_ref = 10; // l_ref是x的異名，將x改成10

int &&r_ref = 200; // R值引用
r_ref = 300; // 將r_ref改成300

int &&x_ref = x; // 將L值賦給R值引用，error
```

R值引用參數：
```cpp
int x {100}; // x是L值
    
void func(int &num);

func(x); // 呼叫func函數，x是L值
func(200); // Error - 200是R值
```

解決辦法，重載func函數
```diff cpp
int x {100}; // x是L值
    
void func(int &num); // Pattern A
+void func(int &&num); // Pattern B

func(x); // 呼叫func函數 - pattern a，x是L值
func(200); // 呼叫func函數 - pattern b，200是R值
```

低效的拷貝：
```cpp
class Move {
private:
    int *data; // raw pointer
public:
    void set_data_value(int d) {
        *data = d;
    }

    int get_data_value() {
        return *data;
    }

    Move(int d); // constructor
    Move(const Move &source); // copy constructor
    ~Move(); // desctructor
};

// implement - constructor
Move::Move(int d) {
    data = new int; // allocate storage
    *data = d;
    cout << "Creating " << d << endl;
}

// implement - copy constructor (deep)
Move::Move(const Move &source) {
    data = new int;
    *data = *source.data;
    cout << "Copying " << *data << endl;
}

// implement - descructor
Move::~Move() {
    cout << "Destructing " << *data << endl;
    
    delete data;
}
```

建立一個vector使用Move物件
```cpp
vector<Move> vec;
    
vec.push_back(Move{10}); // Move{10}會建立臨時值R值，然後拷貝到vec裡面去

cout << "---------" << endl;

vec.push_back(Move{20}); // Move{20}會建立臨時值R值
```

Log輸出如下：
```
Creating 10
Copying 10
Destructing 10
---------
Creating 20
Copying 20
Copying 10
Destructing 10
Destructing 20
Destructing 20
Destructing 10
```

移動構造函數：
不進行深度拷貝，只是移動heap中的資料，複製源對象的地址到目前對象

```diff cpp
class Move {
private:
    int *data; // raw pointer
public:
    void set_data_value(int d) {
        *data = d;
    }

    int get_data_value() {
        return *data;
    }

    Move(int d); // constructor
    Move(const Move &source); // copy constructor
+   Move(Move &&source); // move constructor
    ~Move(); // desctructor
};

+// implement - move constructor
+Move::Move(Move &&source): data {source.data} {
+    source.data = nullptr; // 偷走data，然後清空source pointer
+}

// implement - constructor
Move::Move(int d) {
    data = new int; // allocate storage
    *data = d;
    cout << "Creating " << d << endl;
}

// implement - copy constructor (deep)
Move::Move(const Move &source) {
    data = new int;
    *data = *source.data;
    cout << "Copying " << *data << endl;
}

// implement - descructor
Move::~Move() {
    cout << "Destructing " << *data << endl;
    
    delete data;
}
```

## `this` Pointer

- `this`是保留字，是當前物件的地址，他是一個指針，指向目前的物件
- `this`，只能用在Class Scope內

大部分情況下，this被省略了，例如：

```cpp
class Account {
private:
    double balance {0.0};
public:
    void set_balance(double bal);
};

void Account::set_balance(double bal) {
    // (*this).balance = bal;
    // this->balance = bal;
    balance = bal;
}
```

`balance = bal`，實際上原文是`this->balance = bal`，省略了`this->`部分（編譯器會幫忙補上，Scope Rules）

但是，如果method參數跟class member property都用到同一個名稱`balance`的話，編譯器會搞混，這時就要顯式使用`this->balance`了
```cpp
void Account::set_balance(double balance) {
    this->balance = bal;
}
```

`this`還有另一個功用：比較是否相同對象，快速檢查，提高性能

```cpp
void Account::compare_balance(const Account &other) {
    if (this == &other) {
        cout << "The same object" << endl;
    }
}
```

## Using const with Classes

物件一旦經`const`宣告後，便不能修改成員

```cpp
class Player {
public:
    string name;
    int health;
    int xp;
};

const Player villain {"Villian", 100, 55};
    
villain.name = "Y"; // Error
```

嘗試在method中使用const變數：
`display_player_name()`是一個矛盾的情境，參數的`const Player &p`宣告這個p為常量，不能被修改任何內容，但是他呼叫了自己的method get_name()，也許get_name()裡面放了什麼程式能夠修改自己的property，C++無法確認，所以直接讓他編譯失敗
```cpp
class Player {
private:
    string name;
    int health;
    int xp;

public:
    void get_name() {
        cout << name << endl;
    }
    void display_player_name(const Player &p) {
        p.get_name(); // compile error
    }
};
```

解決方法：告訴C++，`get_name()`不會修改自己的成員（const methods）

```diff cpp
class Player {
private:
    string name;
    int health;
    int xp;

public:
-   void get_name() {
+   void get_name() const {
        cout << name << endl;
    }
    void display_player_name(const Player &p) {
        p.get_name(); // OK
    }
};
```

## Static Class Members

Static表示該成員是屬於Class，而非屬於Object

```cpp:Player.h
class Player {
private:
    string name;
    int health;
    int xp;
    // static int num_players {0}; // 如果這樣寫會出錯，static只能在implement被初始化
    static int num_players;
    
public:
    static int get_num_players();
    Player(string name_val, int health_val = 100, int xp_val = 0); // constructor
    ~Player(); // destructor
};
```

```cpp:Player.cpp
#include "player.h"

int Player::num_players = 0;

int Player::get_num_players() {
    return num_players;
}

//constructor
Player::Player(string name_val, int health_val, int xp_val)
: name {name_val}, health {health_val}, xp {xp_val} {
    ++num_players;
}

// destructor
Player::~Player() {
    --num_players;
}
```

定義完成，開始使用
```cpp:main.cpp
#include "Player.cpp"

void display_active_players() {
    cout << Player::get_num_players() << endl;
}

int main() {
    
    display_active_players(); // 0
    
    Player obj1 {"Frank"};
    
    display_active_players(); // 1
    
    {
        Player hero {"Hero"};
        display_active_players(); // 2
    }
    
    display_active_players(); // 1
    
    Player *enemy = new Player("Enemy", 100, 100);
    
    display_active_players(); // 2
    
    delete enemy;
    
    display_active_players(); // 1
    
    return 0;
}
```

## Structs vs Classes

Class預設成員是private
```cpp
class Person {
    string name;
    string get_name();
};


int main() {
    
    Person p;
    p.name = "Frank"; // error - private member
    cout << p.get_name() << endl; // error - private member
    
    return 0;
}
```

Struct預設成員是public

```cpp
struct Person {
    string name;
    string get_name();
};


int main() {
    
    Person p;
    p.name = "Frank"; // error - private member
    cout << p.get_name() << endl; // error - private member
    
    return 0;
}
```

## Friends

friend member只明明不是隸屬於某class的成員，卻能夠訪問該class的private member

friends的規則：
- 如果A是B的朋友，不代表B也是A的朋友
- 如果A是B的朋友，B是C的朋友，不代表A是C的朋友

將函數聲明為friend
```cpp
class Player {
    friend void display_player(Player &p); // 可以訪問Player所有成員
    string name;
    int health;
    int xp;
public:
    // ...
};

// display_player能夠訪問，也能修改private屬性
void display_player(Player &p) {
    cout << p.name << endl;
    cout << p.health << endl;
    cout << p.xp << endl;
}
```

將另一個class的method聲明為friend
```cpp
class Other_class {
  // ...
public:
    void display_player(Player &p) {
        cout << p.name << endl;
        cout << p.health << endl;
        cout << p.xp << endl;
    }
};

class Player {
    friend void Other_class::display_player(Player &p);
    string name;
    int health;
    int xp;
public:
    // ...
};
```

將整個class聲明為friend
```cpp
class Player {
    friend void Other_class;
    string name;
    int health;
    int xp;
public:
    // ...
};
```


