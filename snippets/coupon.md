> coupon collector problem :)
```c++
// https://math.stackexchange.com/questions/28905/expected-time-to-roll-all-1-through-6-on-a-die
// https://en.wikipedia.org/wiki/Coupon_collector%27s_problem
#include <iostream>
#include <random>
#include <set>

using namespace std;

int main() {
  long double expected = 0.0L;
  const long long int MAXN = 1e5;
  const long long int howmany = 200;

  random_device rd;
  mt19937 gen(rd());
  uniform_int_distribution<> dis(1, howmany);

  for (int i = 0; i < MAXN; ++i) {
    long long trials = 0LL;
    set<int> numbers;
    while (numbers.size() < howmany) {
      numbers.insert(dis(gen));
      trials += 1LL;
    }
    expected += (long double) trials;
  }
  cout << (long double)(expected/MAXN) << endl;
  return 0;
}
```
