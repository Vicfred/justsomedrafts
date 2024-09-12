Template code to use a different algorithm if the struct has it
```cpp
#include <iostream>

using std::cin;
using std::cout;
using std::endl;

template <typename T> struct fast {
  static const bool value = false;
};

template <bool b> struct AlgorithmSelector {
  template <typename T> static void implementation(T &object) {
    object = object;
    cout << "~.~" << endl;
  }
};

template <> struct AlgorithmSelector<true> {
  template <typename T> static void implementation(T &object) {
    object.OptimisedImplementation();
  }
};

template <typename T> void algorithm(T &object) {
  AlgorithmSelector<fast<T>::value>::implementation(object);
}

class ObjectA {};

class ObjectB {
public:
  void OptimisedImplementation() { cout << "wuw" << endl; }
};

template <> struct fast<ObjectB> {
  static const bool value = true;
};

int main() {
  ObjectA a;
  algorithm(a);
  ObjectB b;
  algorithm(b);
  return 0;
}
```
