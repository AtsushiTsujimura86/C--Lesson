
# 1. signed と unsigned を混在させない

## ■ 原則
**signed と unsigned を同じ式で混在させない。**

理由：
- 計算時に「符号なし型へ暗黙変換」されることがある
- 意図しない巨大値や無限ループの原因になる

---

## ■ unsigned のオーバーフロー例

```cpp
unsigned int u = 0;
u = u - 1;
std::cout << u << std::endl;
```

出力：

```
4294967295  // (32bit環境なら 2^32 - 1)
```

理由：
- unsigned は負の値を持てない
- 0 - 1 は内部的に最大値へ回る（mod 2^N）

---

## ■ 終わらない unsigned の for 文

```cpp
for (unsigned u = 10; u >= 0; --u) {
    std::cout << u << std::endl;
}
```

なぜ終わらない？

- u が 0 になる
- --u で最大値に回る
- 常に u >= 0 は true（unsigned は負にならない）

つまり条件が永遠に真。

---

## ■ 終わる unsigned の while 文

```cpp
unsigned u = 11;

while (u > 0) {
    u--;
    std::cout << u << std::endl;
}
```

これは終わる。

理由：
- 判定は u > 0
- 0 になった瞬間に false

---

## ■ 安全な書き方

カウントダウンするなら：

```cpp
for (int i = 10; i >= 0; --i)
```

もしくは：

```cpp
for (unsigned i = 10; i-- > 0; )
```

---

# 2. リテラルとは何か

## ■ 定義

**ソースコードに直接書かれている値**

例：

```cpp
10          // 整数リテラル
3.14        // 浮動小数点リテラル
'a'         // 文字リテラル
"Hello"     // 文字列リテラル
true        // 真偽値リテラル
```

---

## ■ サフィックス（suffix）

リテラルの型を明示できる。

```cpp
10u     // unsigned int
10L     // long
10LL    // long long
10.0f   // float
```

---

## ■ 進数表記

```cpp
010     // 8進数（=8）
0x10    // 16進数（=16）
0b1010  // 2進数（C++14以降）
```

---

# 3. 初期化は代入ではない

```cpp
int a = 10;   // 初期化
a = 20;       // 代入
```

違い：

- 初期化 → オブジェクト生成と同時
- 代入 → 既存オブジェクトへ値を書き換え

参照は「必ず初期化が必要」なのはこのため。

---

# 4. 宣言・定義・extern

## ■ 定義（definition）

実体を作ること。

```cpp
int globalValue = 10;
```

メモリが確保される。

---

## ■ 宣言（declaration）

「その変数はどこかにある」と知らせる。

```cpp
extern int globalValue;
```

メモリは確保されない。

---

## ■ ルール

- 定義は1回だけ
- 他ファイルでは extern 宣言のみ

---

## ■ 例

### main.cpp

```cpp
int globalValue = 10;  // 定義
```

### other.cpp

```cpp
extern int globalValue;  // 宣言
```

---

# 5. 参照とポインタ

---

# ■ 本質の違い

|            | 参照 | ポインタ |
|------------|------|----------|
| 正体 | 別名 | アドレスを持つオブジェクト |
| オブジェクトか | いいえ | はい |
| 再バインド | 不可 | 可能 |
| nullptr | 不可 | 可能 |
| 自身のアドレス | ない | ある |

---

# 6. 参照（Reference）

## ■ 定義

**バインド先オブジェクトの別名**

```cpp
int value = 0;
int &ref = value;
```

- ref は value の別名
- ref 自体はオブジェクトではない

---

## ■ 特徴

- 宣言時に必ず初期化
- 再バインド不可
- 参照の参照は存在しない（折りたたまれる）

---

## ■ 例

```cpp
ref = 10;
std::cout << value << std::endl;  // 10
std::cout << &ref << std::endl;   // value のアドレス
```

---

## ■ ポインタへの参照は可能

```cpp
int* p = &value;
int*& pref = p;   // ポインタの別名
```

---

# 7. ポインタ（Pointer）

## ■ 定義

**他オブジェクトのアドレスを持つオブジェクト**

```cpp
int value = 0;
int* p = &value;
```

---

## ■ 特徴

- オブジェクトである
- 自身のアドレスを持つ
- 再代入可能
- nullptr にできる
- ポインタのポインタ可能

---

## ■ 例

```cpp
*p = 10;
std::cout << *p << std::endl;   // 10
std::cout << p << std::endl;    // value のアドレス
std::cout << &p << std::endl;   // ポインタ自身のアドレス
```

---

# 8. 記号の意味整理

## ■ 宣言時

```cpp
int &ref = value;   // 参照
int *p = &value;    // ポインタ
int *&pref = p;     // ポインタへの参照
int **pp = &p;      // ポインタへのポインタ
```

---

## ■ 式の中

```cpp
&value   // オブジェクトのアドレス
&ref     // 参照先のアドレス
*p       // ポインタ先の中身
p        // ポインタ先のアドレス
&p       // ポインタ自身のアドレス
```

