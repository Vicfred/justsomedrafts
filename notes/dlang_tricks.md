Formatted read/write is the same as C but using `readf`, `readln` and `writefln`

```d
import std.stdio;

void main() {
    int x;
    readf("%d", &x);
    if(x > 2 && x%2 == 0)
        writefln("YES");
    else
        writefln("NO");
}
```
Can also be used as macros. You can strip the string using `strip`

```d
readf!"%d"(n);
readln;
while(n--) {
    string s = readln.strip;
```

Use `chomp` to remove delimiters and cast with `to!int` along with `auto`

```d
auto n = readln.chomp.to!int;
```

Parse a list of space separated integers to a dynamic array.

```d
double[] words = readline.split.map!(x => x.to!double).array;
```

initialize NxM matrix
```d
int[][] a = new int[][](n,m);
```

Iterate the standard input line by line and optionally dropping the first line

```d
foreach(word; stdin.byLine.dropOne)
```

Example of a `filter` over an array `a`

```d
a.filter!(x => (x >= a[k-1]) && (x > 0)).count.writeln;
```

The `count` function is very helpful
```d
a.count!(x => (x >= a[k-1]) && (x > 0)).writeln;
```

A filter without a lambda, iterating over a filter
```d
bool is_vowel(dchar c) {
    return c=='a'||c=='o'||c=='y'||c=='e'||c=='u'||c=='i';
}

void main() {
    auto word = readln.chomp.toLower;

    auto filtered = word.filter!(x => !is_vowel(x));

    foreach(x; filtered)
        write(".", x);
    writeln;
}
```