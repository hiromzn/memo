# reference

- http://cpp-lang.sevendays-study.com/

## tutorial

- hello world

```
#include <iostream>

using namespace std;

int main(){
    cout << "HelloWorld." << endl;
    return 0;
}
```
```
#include <iostream>

int main(){
    std::cout << "HelloWorld." << std::endl;
    return 0;
}
```

## name space : 名前空間

```
using namespace <namespace_name>;
using namespace std;    // standard namespace
```
- 名前空間の利用①
  - using namespace (名前空間名);
- 名前空間の利用②
  - (名前空間名)::(変数名・クラス名など）

## i/o

- cin
- cout

```
    int a;
    cin >> a;
    cout << "a=" << a << endl;
```

## string

```
演算子	意味
+	    文字列同士の足し算
+=	    文字列同士の足し算（代入演算）
==	    比較演算(内容が同等)
!=	    比較演算(内容が異なる)
>, <    比較演算(文字列の大きさ)
```

```
#include <iostream>
#include <string>

using namespace std;

int main(){
    string s,t;
    t ="入力された文字は、";
    cout << "文字列を入力：";
    cin >> s;
    cout << t+s << "です。" << endl;
    return 0;
}

int main(){
    string s;
    s = "This is a";    //  最初の文字列
    s.append(" pen.");  //  文字列の追加
    cout << s << endl;
    cout << "文字列の長さ：" << s.length() << endl;
    //  printfで表示
    printf("char*:%s\n",s.c_str());     // c_str()で、string型をconst char型に変換
    return 0;
}
```

## class

```
class CSample
{
public:
    void function();
    int  function2();
private:
    int m_num;
    char* m_str;
};
```

- メンバ変数・メンバ関数
- アクセス修飾子
  - public
  - private

## object / instance

```
// オブジェクトの生成
CSample obj; // CSampleクラスの変数objを宣言
```
- インスタンス
  - クラスのインスタンスを生成することをインスタンス化、もしくはインスタンスの生成
  - 「クラス」はそのままでは単なる「型」に過ぎ ないので、使うには実体化する必要がある

## アクセス指定子
```
アクセス指定子    意味
public          すべての範囲から呼び出し・読み出し可能
private         同一クラスまたは同一インスタンス内でのみアクセス可能
protected       同一クラスまたは同一インスタンスおよび、サブクラスおよびそのインスタンス内でのみアクセス可能
```

## カプセル化とアクセスメソッド、セッター、ゲッター
- メンバへのアクセスを、そのクラスのメンバ関数からだけしかできないように制限することをカプセル化（隠蔽）
- メンバ変数をprivateにして、メンバ変数を操作するメソッドを提供することで実現する。
- setter / getter
  - setName(), getName()

## コンストラクタ / デストラクタ

- コンストラクタ
  - クラスをインスタンス化したときに、自動的に呼び出される特別なメンバ関数
  - コンストラクタの名前は、クラス名と同じ
  - 戻り値がない
  - メンバ変数の初期化処理
    - クラス名::クラス名() : メンバ変数1(初期値1),メンバ変数2(初期値2)…
    - ex)
      - CCar::CCar() : member1(0),member2(0)
      - member1およびmember2に0を代入
    - メンバ変数の並び順は、ヘッダファイルでの定義順にすることが推奨

- デストラクタ
  - クラスのインスタンスが解放されるときに、 解放の直前で自動的に呼び出される関数
  - 解放されるタイミングは、そのインスタンスのスコープを抜けるとき
    - 例えば、ある関数内でインスタンス化した場合、その関数を抜ける段階で解放される。
  - デストラクタの名前は、クラス名の先頭に ~(チルダ) を付けたもの

## new / delete

- new演算子
  - 書式
    - new コンストラクタ名()
  - new演算子は、コンストラクタを呼び出し、インスタンスを生成する。
  - その直後に書いた型の領域を確保する。

- delete演算子
  - 書式
    - delete インスタンス名
  - deleteされたインスタンスは、デストラクタが実行され、消去されます。
  - newで確保した領域を指すポインタを、deleteの直後に記述する。
  - newで確保した領域を解放する。

- malloc、freeを使わない理由
  - C++では通常、malloc関数（calloc、reallocも同様）および、free()を使わない。
  - 理由
    - malloc関数ではコンストラクタを呼び出すことができないから。
    - free関数では デストラクタが呼び出されないから。

- ※　malloc関数で確保した領域をdeleteで解放してはいけません。

- 配列のnew/delete
  - 配列を確保するには、newの型指定の直後に[]を使って、配列に含まれる要素数を指定する。
  - delete演算子にも[]をつける必要がある。
    - これを付けないと正しく解放できない。
    - この[]の付け忘れは非常によくある間違いです。コンパイルは普通に成功してしまうので気をつけて下さい。

## static member

- 静的メンバは、インスタンスを生成することなく利用するできるメンバ変数、およびメンバ関数
  - これに対し、通常のインスタンスの生成を必要とするメンバのことを、インスタンスメンバと呼ぶ

- 静的メンバ変数
  - 静的メンバ変数は、インスタンスを生成しなくても利用可能
  - 静的メンバ変数の定義
    - static (型） (変数名);
  - 静的メンバ変数の初期値の定義
    - (型）(クラス名):: (変数名);
  - ex)
    - static int m_count;
    - int CSample::m_count=0;

- 静的メンバ関数
  - インスタンスを生成しなくても、呼び出すことが可能
  - 静的メンバ関数の定義
    - static (戻り値の型） (関数名)(引数1,引数2,…);
  - 静的メンバ関数の呼び出し
    - (ClassName)::(FunctionName)();
  - instance->func()のように、インスタンスから呼び出してもかまわないが、静的メンバの性質上、インスタンスを生成しなくても利用できることから、このような使い方をするのが一般的

- static / instance 利用可否

     kind of method | static var | static method | instance var | instance method
    ----|:-:|:-:|:-:|:-:
    static method   | O | O | X | X
    instance method | O | O | O | O

## 継承 / inheritance

- 基本となるクラスの性質を受け継ぎ、独自の拡張をすること
- 親クラス、スーパークラス
  - 継承の元となるクラス
- 子クラス、サブクラス
  - 親クラスの機能を継承し、独自の機能を実装したクラス

- 親クラスを継承した、子クラスの定義の方法
  - class 子クラス名 : public 親クラス名

- メンバの利用  
  - 親クラスのpublicメンバは子クラスで利用可能
  - 親クラスのprivateメンバ変数は、子クラスから直接アクセス出来ない。
  - 子クラスの独自のメンバ変数は、親クラスでは利用できない。

- 親と子クラスのコンストラクタとデストラクタ
  - サブクラスの生成時
    - 親クラスのコンストラクタの実行
    - 子クラスのコンストラクタの実行
  - サブクラスのインスタンスの破棄時
    - 子クラスのデストラクタの実行
    - 親クラスのデストラクタの実行

  - 継承を使う場合、親および子クラスのデストラクタには、virtualをつけることが推奨されている。

- 多重継承
  - format:
    ```
    class Sub : public SupA,public SupB{
    …
    }
    ```
  - 多重継承を用いるのは技術的な問題点も多く推奨されていない。  

## protected

- サブクラスからのアクセスだけを許可する場合にprotectedを利用する。（クラス外からのアクセスは拒否）

scope | parent public | parent protected | parent private
-|:-:|:-:|:-:
parent class | O | O | O
sub class    | O | O | X
Outside      | O | X | X

## overload / override

- オーバーロードの概念
  - 引数、および戻り値が違う複数のメソッドを定義、利用すること。

- オーバーロードのメンバ関数の使い分け
  - オーバーロードされたメンバ関数は、引数の与えられ方などによって使い分けられる。

- コンストラクタにもオーバーロードが使える。

- デフォルトコンストラクタ
  - 引数が付いていないコンストラクタ
  - 注意
    - 引数つきのコンストラクタを定義した場合、デフォルトコンストラクタは省略できない。

- オーバーライドの概念
  - 親クラス、子クラスに同じ名前、同じ戻り値の型、同じ引数をとるメンバ関数が存在する場合、子クラスのメソッドは、親クラスのメソッドをオーバーライドすると言う。
  - オーバーライドされたメソッドは、親クラスが同じメソッドを持っていても、原則的に子クラスに定義されたものを実行する。

- overload / override の意味
  - メンバ変数の名前が統一されることにより、
    - 名前を覚える必要がなくなる
    - 記述ミスを減らせる
  - オブジェクト指向では、原則的に同じ機能には同じ名前をつけることが好ましい
  - 同じような処理でも少しずつふるまいの違うメンバ関数に同じ名前をつけることにより、処理に統一感を持たせることが可能になる。

- ポリモーフィズム(Polymorphism)
  - オーバーライド、オーバーロードのことの総称
  - 日本語：　「多態性（たたいせい）」、「多様性（たようせい）」

## CとC++の違い

### 参照渡し

```
int i=3;
ref_func( i );
print( i ); // i is 1 !

void ref_func( int &n )
{
  n = 1;
}
```
- 参照渡しによる引数の定義
  - void ref(int& n)
- 変数を引数として渡すと、そのアドレスへの参照が渡される。
  - これにより、参照渡しの引数を扱う関数内では、参照渡しされた元の変数の値を変更することが可能になる。
- ポインタ渡しと参照渡しの違い
  - ポインタ渡しは、呼び出す際に引数もポインタ、もしくは変数のアドレスを与えなくてはならない。
  - 参照渡しの場合、変数の名前をそのまま記入すればよい。

### local変数の定義

- C言語：　　ローカル変数は関数の処理の先頭で宣言する必要がある。
- C++言語：　いずれの場所でも宣言できる。

```
// main.c
int main() {
  int i;
  cout << "START" << endl;
  for ( i=0; i<5; i++ ) {
    ...
  }
}

// main.cpp
int main() {
  cout << "START" << endl;
  for ( int i=0; i<5; i++ ) {
    ...
  }
}
```

### 構造体の利用

- C++では、structの定義を簡素化することが可能

```
// main.c
typedef struct {
  int n;
  char *p;
} data;
data x;
x.n = 10;
x.p = "abc";

// main.cpp
struct data {
  int n;
  char *p;
};
data x;
x.n = 10;
x.p = "abc";
```
### C++特有のデータ型（bool）

- C++でbool代数のbool型が利用可能になった。
- 値：　true / false

## クラスの相互参照

#### AとB、2つのクラスが相互参照している場合のヘッダー定義の方法

- 解決策
  - class <class_name>; と記述し、<class_name>というクラスを使うことだけを宣言することで、互いにincludeする問題を回避する。

```
// A.h
class B;    // 下記でクラスBを参照するため、クラスBの名前だけ宣言する。
            // #include "B.h"と記述すると、A,B互定にincludeするため無限ループしてしまう。。。
class A {
  B* class_b;
  ...
}

// B.h
class A;    // 下記でクラスAを参照するため、クラスAの名前だけ宣言する。
            // #include "A.h"と記述すると、A,B互定にincludeするため無限ループしてしまう。。。
class B {
  A* class_a;
  ...
}
```

## this

- thisポインタ = インスタンス自分自身を表すポインタ

## const

- 値が変更されないことを保証する仕組み
- 宣言場所により保証する対象がことなる。

- const 変数
  - const宣言された定数が変更されないことを保証する。
    - const int max = 120;	//	定数の定義（以後、値は変更できない)
    - max = 130; // コンパイル エラー(constの定数の値を変更しようとしたので)

- 引数のconst
  - const宣言したポインタ引数、参照引数で値を変更されないことを保証する。
    - void foo(const A* pA);	//	Aはクラス名（ポインタの場合）
    - void bar(const A& pA);	//	Aはクラス名（参照の場合）
    - AクラスのインスタンスpAの値が変更されないことを保証する。
      - もし値を変更するような処理をした場合、エラーが発生する。

- constメンバ関数
  - const宣言されたメンバ関数内で、同インスタンス内のメンバ変数を変更しないことを保証する。
    - int getNum() const;	//	constメンバ関数

使用場所        | 使用例                      | 意味              | 解説
---------------|-----------------------------|------------------|--------------------------
変数の前        | const int a = 100;          | 定数の定義        | 変数の値を変更できない
メンバ関数の引数 | void setNum(const int a);  | 引数の変更禁止      | 関数内では、引数の状態が変化しない
メンバ関数の後ろ | int getNum() const;        |メンバ変数の変更禁止	| 関数内では、メンバ変数の状態が変化しない

## template

- テンプレートとは
  - 演算子が同じであれば、型が何であっても使えるようにする仕組み
- 実現できること
  - 型をパラメータ化することで、クラスと関数を、型から分離して記述できる。
  - クラス定義の再利用、関数定義の再利用を可能とする。
  - ジェネリックプログラミングの実現
    - 型を決定しないクラスと関数の断片を用意しておき、これらの組み合わせでプログラミングする。
- テンプレートの種類
  - 関数に用いるテンプレート関数
  - クラスに用いるテンプレートクラス

- study ref
  - https://qiita.com/a-beco/items/7709ab80a05ea2d09ea8
  - http://www.cplusplus.com/doc/oldtutorial/templates/
  - http://www.cplusplus.com/

- 型をパラメータ化したコンテナ
  - std::vector
  - std::list
  - std::map
  - std::queue
- 型をパラメータ化した関数
  - std::min, std::mac
  - std::swap
  - std::find
  - std::sort
  - イテレータという仕組みによって、具体的なコンテナではなく抽象的な値の列を扱う。

#### テンプレート関数

- 関数テンプレートの定義
  ```
  //  テンプレート関数
  template <typename T>
  T add(T x, T y){
      return x + y;
  }
  ```

  - T
    - 変数の型を表すテンプレート引数
    - データ型およびクラスに対応させて、変数型の異なる関数まとめて定義する。

- 関数テンプレートの利用

  - template_func_name<Type>( arg1, arg2, ...);
  - template_func_name( arg1, arg2, ...); // 引数の型が明確な場合、Typeを省略可能

  ```
  add<int>(4,3);  // -> add( int 4, int 3 );
  add<string>("ABC", "DEF");  // -> add( string "ABC", string "DEF" );

  int a, b;
  add( a, b ); // <int>を省略可能
  ```

#### テンプレートクラス

- テンプレートクラスの実装はヘッダファイルにすることが推奨されている

- テンプレートクラスの実装

  - テンプレートクラスの宣言
    - template<typename T> class CCalc
    - テンプレートTをクラスとして、利用するということを宣言

  - テンプレートクラスのインスタンスの生成
    - template_class_name<type> class_instance_var;

```
template<typename T> class CCalc{
private:
    T m_a;
    T m_b;
public:
    inline void set(const T a, const T b) { //  引数のセット
      m_a = a; m_b = b;
    }
    inline T add() const{ //  計算結果
      return m_a + m_b;
    }
};

CCalc<int> i_int;
CCalc<string> i_string;

i_int.set( 1, 2 );
i_string.set( "ABC", "DEF" );
cout << i_int.add() << i_string.add() << endl;
```

#### 複数のテンプレートの宣言

- template<typename T, typename S>

- 利用例：
  - void add<int, double>(1,2.3);

## inline関数

- inline修飾子
  - 関数にこの修飾子がつくと、その関数はコンパイル時にインライン展開される
  - inline関数を用いると、関数が直接埋め込まれるため、処理呼び出しなどのオーバーヘッドが少なくなる。

- inline関数を宣言する場は、必ずヘッダーファイルに記述する（推奨）

- inline関数のデメリット
  - 関数の利用回数が多く、関数の処理が長い場合には、生成されたソースコードおよび実行ファイルが大きくなりすぎる。
  - コンパイラに依存する分部が多いため、コードの種類によっては効果が出ないこともある。

- inlineを用いるのが好ましい場合
  - セッター・ゲッターのようなケースのように、オーバーヘッドが少ない上に、頻繁に使用される処理。
  - このような場合に、constなどと併せて用いると、C++のプログラムのパフォーマンスの向上が期待できる。

## STL

- Standard Template Library
- テンプレートを用いた標準的なC++のライブラリ

名前 | 役割
-----|-------
vector | 動的配列
list | 双方向リスト
map | 連想配列
set | 集合
stack | スタック
queue | キュー

#### vector

- 動的配列
- サイズを意識せずに使える配列
- 要素はインデックスで管理

- member function
  - push_back()	要素の追加
  - clear()	要素のクリア
  - size()	配列の大きさを得る関数
  - capacity()	動的配列に追加できる要素の許容量
  - empty()	要素が空かどうかを調べる

```
#include <vector>

vector<int> v1;
v1.push_back(1);
v1.push_back(2);
for ( int i = 0; i < v1.size(); i++ )
{
  cout << "v1[" << i << "]=" << v1[i] << endl;
}
```

#### list

- 要素はイテレータで管理

- イテレータ
  - 宣言
    - list<int>::iterator itr;
  - リストの先頭に
    - itr = list_instance.begin();
  - イテレータの次に値を挿入
    - insert( イテレータ, 値 );

- member function
  - push_front()	先頭に要素を追加する。
  - push_back()	末尾に要素を追加する。
  - pop_front()	先頭の要素を削除する。
  - pop_back()	末尾の要素を削除する。
  - insert()	要素を挿入する。
  - erase()	要素を削除する。
  - clear()	全要素を削除する。

```
#include <list>
using namespace std;

    list<int> li;

    li.push_back(1);    //  後ろにデータを挿入
    li.push_back(2);    //  後ろにデータを挿入
    li.push_front(3);   //  前にデータを挿入

    list<int>::iterator itr;

    //  データの挿入
    itr = li.begin();   //  イテレータを先頭に設定
    itr++;              //  一つ移動
    li.insert(itr,4);   //  値の挿入

    //  データの表示
    for (itr = li.begin(); itr != li.end(); itr++){
        cout << *itr << " ";
    }
    cout << endl;
```

## vectorとlistの使い分け

- 共通点
  - どちらもイテレータ利用可能

- 相違点
  - removeは、listで利用可能（vectorには存在しない。）

- vectorとlistの特徴
  - vector
    - 配列の延長上の概念
    - サイズを最初に指定しなくても使えることが主眼
  - list
    - 双方向連結リスト
    - 任意の場所の要素が削除されたり、挿入されたりするような使用方法を想定
    - インデックスで管理することはできない
    - イテレータで管理する

#### Map

- 連想配列
- キーを使って、要素を指定する。
- mapの型の宣言
  - map <キーの型, 値の型>

- member function
  - clear()	全ての要素をクリアする。
  - empty()	マップが空であるときに true を返し、 そうでないときに false を返す。
  - erase()	指定した要素をクリアする。
  - size()	マップの中の要素数を返す。
  - find()	マップから指定したキーが一致する要素を探し、 イテレータ を返す。

```
#include <map>
using namespace std;

int main() {
    map <string, int> score;  // map のデータ構造を用意する。
    string names[] = { "Tom","Bob","Mike" };
    score[names[0]] = 100;          //  キーと値の関連付け① Tom : 100
    score[names[1]] = 80;           //  キーと値の関連付け② Bob : 80
    score[names[2]] = 120;          //  キーと値の関連付け③ Mike : 120
    for( int i = 0; i < 3; i++ ){
            cout << names[i] << ":" << score[names[i]]  << endl;
    }
    return 0;
}
```

#### set
- 集合
- 要素が重複なく登録される = 同じデータは、必ずひとつしか登録できない
- データの有無の調査
  - it = names.find(n[i]);

```
#include <set>
using namespace std;

int main() {
    set<string> names;  // set のデータ構造を用意する。
    names.insert("Tom");
    names.insert("Mike");
    names.insert("Mike");   //  同じ名前をinsertしても登録は1つだけ
    names.insert("Bob");
    //  登録されている全データを表示
    set<string>::iterator it; //  イテレータを用意
    for( it = names.begin() ; it != names.end(); it++ ){
        cout << *it << endl;
    }
    //  Bob,Steveがデータ内に存在するか調べる
    string n[] = {"Bob","Steve"};
    int i;
    for(i = 0; i < 2;i++){
        it = names.find( n[i] );
        if(it == names.end()){ //  データが、set内に存在しなしい
            cout << n[i] << " is not in a set." << endl;
        }else{                 //  データがset内に存在する。
            cout << n[i] << " is in a set." << endl;
        }
    }
    return 0;
}
```

## 仮想関数 virtual

- 親クラスから、子クラスで個別に定義された共通メソッドを呼び出すための仕組み

- 純粋仮想関数(じゅんすいかそうかんすう)
  - メソッドそのものは存在するけれども、実装がないクラスです。
  - 実装は、このクラスを継承した子クラスにされることが前提。
  - 宣言
    - virtual void sing()=0;
  - 純粋仮想関数を呼び出そうとするとエラーになる。（純粋仮想関数ではない、普通の仮想関数は呼び出し可能）

## 仮想デストラクタ

- デストラクタにvirtualをつける理由
  - 親クラスのデストラクタにvirtualをつけると、delete時にサブクラスのデストラクタが実行されたのちに、親クラスのコンストラクタが実行される。
  - 親クラスのデストラクタにvirtualがついていない場合、delete時には親クラスのデストラクタのみが実行され、サブクラスのデストラクタは実行されない。

## interface

- インターフェースとは、
  - 完全仮想関数のみからなるクラス。
  - 純粋仮想関数が記述されている関数しか利用できない仕組みを提供するクラス。

- インターフェースの効能
  - インターフェースを利用することのメリットは、相手のクラスに対して機能を制限したい場合に有効
  - 多重継承は推奨されないが、インターフェースでは多重継承が可能であり、現在のオブジェクト指向プログラミングにおいて、主流の考え方になっている。

```
// interface class
class IInterface1{
public:
    virtual void func1() = 0;
    virtual void func2() = 0;
};

class IInterface2{
public:
    virtual void func3() = 0;
    virtual void func4() = 0;
};

class CSample : public IInterface1, public IInterface2 {
public:
    void func1(){ cout << "func1" << endl; }
    void func2(){ cout << "func2" << endl; }
    void func3(){ cout << "func3" << endl; }
    void func4(){ cout << "func4" << endl; }
};
```

## 演算子のオーバーロード

- 通常の演算子の機能を拡張して、さまざまなクラスに演算処理を実装したり、もともとある機能を変更する仕組み
  
- 演算子のオーバーロードの宣言
  - 戻り値の型 operator演算子(引数…)
  - ex)
    - Vector2 operator+(const Vector2&, const Vector2&);

## lvalue, rvalue

- https://cpplover.blogspot.com/2009/11/rvalue-reference_23.html

- lvalue
  - 特定のメモリ位置を指すもの
  - 変数として存在するので長寿命
  - コンテナ
  - 明示的に実体のある名前付きのオブジェクト
- rvalue
  - どこも指し示さないもの
  - 一次的なので短命
  - コンテナに含まれるもの
  - 一時的に生成される無名のオブジェクト

### Understanding the meaning of lvalues and rvalues in C++
- https://www.internalpointers.com/post/understanding-meaning-lvalues-and-rvalues-c

#### Lvalues and rvalues: a friendly definition

- int x = 666; // OK
  - 666 : rvalue : 数値であり厳密にはリテラル定数。レジスターを除くと、特定のメモリアドレスは持たない。
  - x : lvalue : 変数：特定のメモリを持っている。

- int * y =＆x; // OK
  - アドレス演算子＆を使ってxのメモリアドレスを取得し、それをyに入れる。
  - 代入の左側にはlvalue（変数）があり、右側にはaddress-of演算子によって生成されたrvalueがある。

- int y;
- 666 = y; // ERROR!!
  - ERRORは明白
  - 技術的な理由は、666は文字通りの定数であり右辺値なので、特定のメモリ位置を持たないこと。どこにもyを割り当てていません。
  - GCCは以下のエラーを出力する。
    - error: lvalue required as left operand of assignment
    - 意味：　代入の左のオペランドは常にlvalueが必要
  - 上記例では、左のオペランドにrvalueの666を使っているためにエラーとなる。

- int* y = &666; // error!
  - GCC says:
    - error: lvalue required as unary '&' operand`
    - 意味：　&オペランドとしてlvalueが必要
  - 理由：　lvalueだけが、＆を処理できるアドレスを持っているため。

#### Functions returning lvalues and rvalues

- 代入の左のオペランドはlvalueである必要があります。
- よって、次のような関数は、確実に「lvalue required as left operand of assignment」エラーになる。

```
int setValue()
{
    return 6;
}

// ... somewhere in main() ...

setValue() = 3; // error!
```

- setValue（）はrvalue（一時的な数6）を返す。これは代入の左オペランドにはできない。
- 関数がlvalueを返す方法は以下。

```
int global = 100;

int& setGlobal()
{
    return global;    
}

// ... somewhere in main() ...

setGlobal() = 400; // OK
```

- 上記のsetValue（）とは異なり、ここではsetGlobalが参照を返すので機能する。
- 参照は既存のメモリ位置（グローバル変数）を指すものであるため、lvalueであるため、代入することができる。
  - 補足：　＆ はアドレス演算子ではなく、返されるものの型（参照）を定義する。
- 関数から左辺値を返す機能はかなりあいまいに見えるが、オーバーロードされた演算子を実装するなどの高度な機能を実装する場合に役立つ。 

#### Lvalue to rvalue conversion

- lvalueはrvalueに変換されることがある。
- たとえば、加算+演算子の場合、C ++仕様によれば、引数として2つのrvalueを取り、rvalueを返します。

```
int x = 1;
int y = 3;
int z = x + y;   // ok
```

- xとyはlvalueですが、加算演算子はrvalueが必要です。
- 実際には、xとyは、暗黙のlvalueからrvalueへの変換されている。
  - 他の多くの演算子（加算、減算、除算）がそのような変換を実行する。

#### Lvalue references

- 反対の変換であるrvalueをlvalueへの変換はできない。
- 理由：
  - 技術的にできないのではなく、プログラミング言語としてこの変換を禁止するように設計されていいる。

```
int y = 10;
int& yref = y;
yref++;        // y is now 11
```

- yrefを型int＆として宣言し、yrefはyへの参照を示している。これはlvalue referenceと呼ばれている。
- これにより、参照であるyrefを通じてyの値を変更できる。

- 参照は特定のメモリ位置、すなわちlvalueにある既存のオブジェクトを指している必要がある。
- 一方、以下のように、それを保持しているオブジェクト（y）なしで10を直接参照に割り当てる場合を想定する。

```
int& yref = 10;  // will it work? => NG
```

- 右側には、lvalueが示すどこかに格納する一時的なrvalueがある。
- 左側には、既存のオブジェクトを指す参照（lvalue）がある。
- しかし、10は数値定数であり、特定のメモリアドレスやrvalueがないため、この式は参照の精神と衝突します。

- これは、rvalueからlvalueへの禁じられた変換である。
- 参照されるためには、揮発性数値定数（rvalue）が左辺値になる必要があり、それが許されるならば、その参照を通して数値定数の値を変更することができる。
- しかし、これは無意味である。
- 最も重要なのは、数値がなくなった後の参照は何を指すかということである。

- 次のコードは、まったく同じ理由で失敗する。

```
void fnc(int& x)
{
}

int main()
{
    fnc(10);  // Nope!
    // This works instead:
    // int x = 10;
    // fnc(x);
}
```

- 引数として参照を受け取る関数に一時的なrvalue（10）を渡している。
- rvalueからlvalueへの変換は無効である。
- 回避策（コメントアウトされたコード）
  - 右辺値を格納する場所に一時変数を作成してから関数に渡すこと。
  - しかし、関数に数値を渡したいだけの場合は非常に不便。

#### Const lvalue reference to the rescue
- 定数左辺値参照の救済

- 最後の2つのコードでは、GCCで以下のエラーが発生する。

```
error: invalid initialization of non-const reference of type 'int&' from an rvalue of type 'int'
（エラー：型 'int'のrvalueから型 'int＆'の非const参照の初期化は無効）
```

- 参照がconst（定数）ではないことでGCCエラーになっている。
- 言語の仕様では、const lvalueをrvalueに束縛することが許されている。
- したがって、正しいコードは次のようになる。

```
const int& ref = 10;  // OK!
```

- そして、以下も同様にＯＫ

```
void fnc(const int& x)
{
}

int main()
{
    fnc(10);  // OK!
}
```

- 背後にある考え方は非常に簡単です。 
- リテラル定数10は揮発性であり、すぐに期限切れになるので、これを参照しても意味がない。
- 代わりに、参照自体を定数にして、それが指す値を変更できないようにしている。
- これでrvalueを修正するという問題は解決された。 
- 繰り返しになるが、これは技術的な制限ではなく、愚かなトラブルを回避するためにC++の人々が選択したもの。

- これは、前に抜粋したように、関数への定数参照によって値を受け取るという非常に一般的なC++イディオムを可能にします。

- 内部的には、コンパイラーは元のリテラル定数を格納する場所に隠し変数（左辺値）を作成し、その隠し変数をユーザーの参照にバインドする。
- これは基本的に、上の2つのスニペットで手動で行ったのと同じこと。例えば、

```
// the following...
const int& ref = 10;

// ... would translate to:
int __internal_unique_name = 10;
const int& ref = __internal_unique_name;
```

- これで、参照は実際に存在するもの（範囲外になるまで）を指し示し、それが指す値を変更することを除けば通常どおり使用できる。

```
const int& ref = 10;
std::cout << ref << "\n";   // OK!
std::cout << ++ref << "\n"; // error: increment of read-only reference ‘ref’
```

#### Conclusion

Understanding the meaning of lvalues and rvalues has given me the chance to figure out several of the C++'s inner workings. C++11 pushes the limits of rvalues even further, by introducing the concept of rvalue references and move semantics, where — surprise! — rvalues too are modifiable. I will restlessly dive into that minefield in one of my next articles.

- 左辺値と右辺値の意味を理解することで、C ++の内部動作のいくつかを理解することができた。
- C++11は、右辺値の参照と移動のセマンティクスの概念を導入することで、右辺値の限界をさらに広げている。（右辺値も変更可能）

#### Sources

- Thomas Becker's Homepage - C++ Rvalue References Explained
  - (http://thbecker.net/articles/rvalue_references/section_01.html)
- Eli Bendersky's website - Understanding lvalues and rvalues in C and C++
  - (http://eli.thegreenplace.net/2011/12/15/understanding-lvalues-and-rvalues-in-c-and-c)
- StackOverflow - Rvalue Reference is Treated as an Lvalue?
  - (http://stackoverflow.com/questions/28483250/rvalue-reference-is-treated-as-an-lvalue)
- StackOverflow - Const reference and lvalue
  - (http://stackoverflow.com/questions/22845167/const-reference-and-lvalue)
- CppReference.com - Reference declaration
  - (http://en.cppreference.com/w/cpp/language/reference)
