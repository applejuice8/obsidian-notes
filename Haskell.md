![Haskell](https://cdn.svglogos.dev/logos/haskell-icon.svg)

---

# ghci
```bash
ghci  # Start ghci interactive program

:q  # :quit

:l  # :load  # Load source file to ghci
:l file.hs  # Load script to ghci
:reload  # Reload current script

:edit name  # Edit script name

:type expr  # Type of expression

:?  # Show all commands

:i  # :info  # See associativity, precedence
```

---

# Hello
```haskell
-- test.hs
sayHello :: String -> IO ()
sayHello x = putStrLn("Hello, " + x + "!")
```

```bash
ghci
ghci> :l test.hs
ghci> sayHello "Haskell"

Hello, Haskell!
```

---

# Define Functions
```haskell
-- In source file
triple x = x * 3
```

```bash
# In REPL
let triple x = x * 3
```

---

# Lazy Evaluation
```haskell
lazyAdd :: Int -> Int -> Int
lazyAdd x y = 1
```

```bash
ghci> lazyAdd 2 (3 + 4)
# Since always returns 1, Haskell doesn't compute 3 + 4
```

---

# Convert Infix, Prefix
```haskell
-- Prefix to infix
div 10 4
10 `div` 4

-- Infix to prefix
10 + 4
(+) 10 4
```

---

# Order of Declaration
```haskell
-- Order doesn't matter in source file
res = x + y
x = y + 5
y = 10
```

```bash
# Order matters in REPL
ghci> y = 10
ghci> x = y + 5
ghci> res = x + y
```

---

# $
- Like function application
- But right associative instead of left
```haskell
print $ sum $ map (+1) [1, 2, 3]
print (sum (map (+1) [1, 2, 3]))
```

---

# let, where
- Same result, 2 styles
```haskell
-- let
myFunc =
	let x = 1 + 1
		y = 2 + 2
	in x * y

-- where
myFunc = x * y
	where
		x = 1 + 1
		y = 2 + 2
```

---

# String / List Functions
## Print
```haskell
-- Print any value that is an instance of Show (With newline)
print :: Show a => a -> IO ()
print "Hello"

-- Same but only for string
putStrLn :: String -> IO ()
putStrLn "Hello"

-- Without newline
putStrLn :: String -> IO ()
putStr "Hello"  
```

## Concat (++)
```haskell
-- Concat1
(++) :: [a] -> [a] -> [a]
[1, 2] ++ [3, 4]  -- [1, 2, 3, 4]
"Hello " ++ "World"  -- "Hello World"

-- Concat2 (Same result for different format)
concat :: Foldable t => t [a] -> [a]
concat [[1, 2], [3, 4]]
concat ["Hello ", "World"]
```

## Cons Operator (:)
- Used to build list
- Reversed append()
- a : b (Append a to b)
```haskell
(:) :: a -> [a] -> [a]

1 : [2, 3, 4]  -- [1, 2, 3, 4]
'a' : "bc"  -- "abc"

-- List is syntactic sugar
[1, 2, 3]
1 : 2 : 3 : []
1 : (2 : (3 : []))  -- Right associative
```

## head, tail
```haskell
-- Return first item
head :: HasCallStack => [a] -> a
head [10, 20, 30]   -- 10
head "Haskell"   -- 'H'

-- Drop first item
tail :: HasCallStack => [a] -> [a]
tail [10, 20, 30]   -- [20, 30]
tail "Haskell"   -- "askell"
```

## take
```haskell
-- Return first few elements
take :: Int -> [a] -> [a]
take 3 [1, 2, 3, 4, 5]  -- [1, 2, 3]
take 0 [1, 2, 3]  -- []
take 10 [1, 2, 3]  -- [1, 2, 3]

take 4 "Haskell"           -- "Hask"
```

## drop
```haskell
-- Drop first few elements
drop :: Int -> [a] -> [a]
drop 3 [1, 2, 3, 4, 5]  -- [4, 5]
drop 0 [1, 2, 3]  -- [1, 2, 3]
drop 10 [1, 2, 3]  -- []

drop 4 "Haskell"           -- "ell"
```

## Indexing Operator (!!)
```haskell
(!!) :: [a] -> Int -> a
[10, 20, 30, 40] !! 0  -- 10
"Hello" !! 1  -- 'e'
[1,2,3] !! 5  -- Index out of bounds
```

---

# main, do
- Haskell only executes main, everything must be wrapped inside main
- `do` is special syntax allows sequencing actions
```haskell
-- Sequence actions
-- Do action, ignore result, continue next action
(>>) :: IO a -> IO b -> IO b

-- Bind operator
-- Do action, pass result into next action
(>>=) :: IO a -> (a -> IO b) -> IO b
```

```haskell
main :: IO ()
main = do
	putStr "Your name: "
	name <- getLine
	putStrLn ("Hello, " ++ name)

-- Underneath the hood
main :: IO ()
main =
	putStr "Your name: " >>
	getLine >>= \name
	-> putStrLn ("Hello, " ++ name)
```

## Can Separate
```haskell
askName :: IO String
askName = do
	putStr "Your name: "
	getLine

greet :: String -> IO ()
greet name = putStrLn ("Hello, " ++ name)

main :: IO ()
main = do
	name <- askName
	greet name
```

## 2 Types of Assignment
```haskell
x = 7  -- For most types

-- For IO types
main = do
	name <- getLine  -- Convert IO String to String
	putStrLn "Hello, " ++ name

main =
	getLine >>= \name ->  -- If not inside do (Must pass to func)
	putStrLn "Hello, " ++ name
```

---

# Boolean Data Type
```haskell
data Bool = False | True
-- Type constructor = Data constructor | Data constructor
```
