# 例外処理
## モダンな例外処理
```cpp
#include <iostream>
#include <stdexcept>
#include <vector>

// 例外を投げないことを明示
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
        double result = divide(10.0, 0.0);
    } catch (const std::invalid_argument& e) {
        std::cerr << "Error: " << e.what() << std::endl; // 適切なエラー処理
    } catch (const std::exception& e) {
        std::cerr << "General error: " << e.what() << std::endl;
    }
    return 0;
}

```
