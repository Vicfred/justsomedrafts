read an integer and pass it to a function
```haskell
-- https://codeforces.com/problemset/problem/4/A

f :: Int -> String
f x
    | x == 2 = "NO"
    | even x = "YES"
    | otherwise = "NO"

main :: IO ()
main = interact $ f . read
```

read a list of integers

```haskell
-- https://codeforces.com/problemset/problem/1/A
main :: IO()
main = do
    [n, m, a] <- fmap (map read . words) getLine
    putStrLn $ show $ ceiling (n / a) * ceiling (m / a)
```

```haskell
main :: IO ()
main = getLine >>= print . product . map read . words
```

```haskell
main :: IO ()
main = product <$> map read <$> words <$> getLine
```

read n lines of strings and process them
```haskell
-- https://codeforces.com/problemset/problem/71/A
transform :: String -> String
transform s
    | (length s <= 10) = s
    | otherwise = ([head s] ++ show (length s - 2) ++ [last s])

main :: IO ()
main =  do
    n <- getLine
    words <- getContents
    putStrLn . unlines . map transform . lines $ words
```

ignoring the first n

```haskell
-- https://codeforces.com/problemset/problem/71/A
transform :: String -> String
transform s
    | (length s <= 10) = s
    | otherwise = ([head s] ++ show (length s - 2) ++ [last s])

main :: IO ()
main =  interact $ unlines . (map transform) . tail . words
```

using replicateM
```haskell
-- https://codeforces.com/contest/71/problem/A

import Control.Monad

solve :: String -> String
solve s =
    let len = length s
    in if len <= 10
        then s
        else ([head s] ++ show (len - 2) ++ [last s])

main :: IO ()
main = do
    n <- getLine
    inputs <- replicateM (read n) getLine
    let answers = map solve inputs
    sequence_ (map putStrLn answers)
```