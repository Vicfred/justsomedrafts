```c++
  set<string> valid{"a", "b", "c", "?"};
  for(const auto& i : valid) {
    for(const auto& j : valid) {
      for(const auto& k : valid) {
        for(const auto& l : valid) {
          for(const auto& m : valid) {
            for(const auto& n : valid) {
              for(const auto& o : valid) {
                string str = i + j + k + l + m + n + o;
                good.insert(str);
              }
            }
          }
        }
      }
    }
  }
```
