# 例外処理
## モダンな例外処理
```cpp
#include <iostream>
#include <stdexcept>
#include <vector>

// 例外を投げないことをnoexceptで明示
void processData(const std::vector<int>& vec) noexcept {
    if (vec.empty()) return; 
    // ...
}

// 例外を投げる可能性のある関数
double divide(double a, double b) {
    if (b == 0.0) {
        throw std::invalid_argument("Division by zero"); // stdexceptの利用
    }
    return a / b;
}

int main() {
    try {
        double result = divide(10.0, 0.0); //例外が発生する可能性あり
    } catch (const std::invalid_argument& e) {
        std::cerr << "Error: " << e.what() << std::endl; // 適切なエラー処理
    } catch (const std::exception& e) {
        std::cerr << "General error: " << e.what() << std::endl;
    }
    return 0;
}

```

## 例外の種類
```
std::exception
├── std::logic_error
│   ├── std::invalid_argument
│   ├── std::domain_error
│   ├── std::length_error
│   └── std::out_of_range
│
├── std::runtime_error
│   ├── std::overflow_error
│   ├── std::underflow_error
│   ├── std::range_error
│
├── std::bad_alloc
├── std::bad_cast
├── std::bad_typeid
├── std::bad_exception
```
<br>

## 標準例外の種類と意味（重要なもの）

C++の標準例外は、大きく以下のカテゴリに分かれます。

---

### 1. std::logic_error 系
**意味：プログラムの論理的ミスを表す例外**

主に「コードの使い方が間違っている」場合に使われます。

| 型 | 意味 | 使用例 |
|------|------|--------|
| std::invalid_argument | 不正な引数 | 関数に不正な値を渡した |
| std::domain_error | 定義域エラー | sqrt(-1) のような数学的に不正な値 |
| std::length_error | 長さ異常 | サイズが上限を超えた |
| std::out_of_range | 範囲外アクセス | vector.at(1000) など |

---

### 2. std::runtime_error 系
**意味：実行中に発生し得るエラー**

入力・状態・計算結果によって起こる可能性があります。

| 型 | 意味 | 使用例 |
|------|------|--------|
| std::overflow_error | オーバーフロー | 計算結果が上限を超えた |
| std::underflow_error | アンダーフロー | スタックが空なのにpop |
| std::range_error | 計算範囲エラー | 値が想定範囲外になった |

---

### 3. メモリ・型関連例外

| 型 | 意味 | 使用例 |
|------|------|--------|
| std::bad_alloc | メモリ確保失敗 | new に失敗 |
| std::bad_cast | dynamic_cast失敗 | 不正な型変換 |
| std::bad_typeid | typeid失敗 | 無効なオブジェクトに対するtypeid |
| std::bad_exception | 例外仕様違反 | 例外仕様に違反した場合 |

---

## 補足：共通の基底クラス

すべての標準例外は以下を継承しています。

```cpp
std::exception
```

そのため、まとめて捕まえることも可能です。

```cpp
catch (const std::exception& e) {
    std::cout << e.what() << std::endl;
}
```

---

## 使い分けの目安

- **logic_error** → プログラム側の設計・使用ミス
- **runtime_error** → 実行中に起こり得る問題
- **bad_alloc など** → システムレベルの異常

---

## 実務でよく使うもの

- std::invalid_argument
- std::out_of_range
- std::runtime_error
- std::bad_alloc
- std::exception（まとめて捕まえる場合）

---

例外は「エラーの意味を型で分類する仕組み」です。

