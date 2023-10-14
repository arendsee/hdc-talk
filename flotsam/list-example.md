

 [123,23,4]  ---> 23, i.e. find the second element in a list

``` python
x = [123,23,4]
x[1]
```

``` haskell
nth :: Int -> [a] -> Maybe a
nth i = case drop (i - 1) of
 [] -> Nothing
 (x:_) -> Just x
```

``` Idris
nth :: {i:Int | i >= 0, i < length xs} -> xs:[a] -> a
nth i xs = xs !! i
```

``` Bioinformatics
sed 's/\[[^,]*,\([^,]*\).*/\1/'

sed 's/^\[//' | sed 's/\]$//' | awk 'BEGIN{FS=","} {print $2}' | sed 's/^ *//' | sed 's/ *$//'
```
