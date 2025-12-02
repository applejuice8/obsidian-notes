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
- prepend
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

# Data Types
## Boolean
- Type constructor = Data constructor | Data constructor
```haskell
data Bool = False | True
-- Type constructor (Datatype Typename)
```

## Numeric Types
- Every Int, Integer is an Integral
- Every Integral, Fractional is a Num
- Every Float, Double is a Fractional
![Numeric Types](https://ksvi.mff.cuni.cz/~dingle/2022-3/npp/haskell_numeric.svg)

```haskell
-- Integral Numbers
x :: Int  -- 64 bit
x = 5

y :: Integer  -- Arbitrary large
y = 123456789012345678901234567890



-- Fractional Numbers
piF :: Float  -- 32 bit
piF = 3.14

piD :: Double  -- 64 bit
piD = 3.141592653589793

import Data.Ratio
half :: Ratio Int
half = 3 % 4  -- 3/4 exactly, no rounding

import Data.Scientific
x :: Scientific
x = 12345e-3  -- 12.345 as Scientific



-- Complex Numbers
import Data.Complex
z :: Complex Double  -- Complex number with real, imaginary parts
z = 2 :+ 3  -- 2 + 3i
```

## Tuples
```haskell
showPerson :: (String, Int) -> String
showPerson (name, age) = name ++ " is " ++ age ++ " years old."

divide :: Int -> Int -> (Int, Int)
divide x y = (x `div` y, x `mod` y)
```

## Lists
- All elements must have same type
```haskell
nums :: [Int]
nums = [1, 2, 3]
nums = 1 : 2 : 3 : []  -- Same with cons operator

-- Ranges
[1..5]  -- [1,2,3,4,5]
[2,4..10]  -- [2,4,6,8,10]
[10,9..4]  -- [10,9,8,7,6,5,4]

-- Infinite list
[1..]  -- [1,2,3, ...]
take 5 [1..]   -- [1,2,3,4,5]
```

---

# Currying
- Every function in Haskell takes in exactly 1 argument, returns 1 result
- A multi-argument function is a chain of single-argument functions
- Allow us to partially apply a function
```haskell
add :: Int -> Int -> Int  -- Same as (Int -> (Int -> Int))
add x y = x + y

-- Partially application
add5 = add 5
add5 :: Int -> Int
add5 y = 5 + y

add5 10  -- =15
```

## Built-in Curry, Uncurry
```haskell
curry :: ((a, b) -> c) -> a -> b -> c
curry f x y = f (x, y)

uncurry :: (a -> b -> c) -> (a, b) -> c
uncurry g (x,y) = g x y
```

## Manual Curry, Uncurry
```haskell
-- Manual curry
f :: (a, b) -> c

f' :: a -> b -> c
f' x y = f (x, y)

-- Manual uncurry
g :: a -> b -> c

g' :: (a, b) -> c
g' (x, y) = g x y
```

---


# Type Signature
```haskell
functionName :: inputType1 -> inputType2 -> ... -> outputType

-- Function takes in 2 numeric types, returns 1 numeric type
add :: Num a => a -> a -> a
```

---

# Polymorphism
- Lowercase is polymorphic, is a type variable (a, b, c)
- Uppercase is concrete type (`Int`, `Bool`, `[Char]`)

## 3 Types
- Concrete
- Parametric polymorphism (Fully polymorphic)
- Constrained polymorphism (Ad-hoc polymorphic)

### Parametric Polymorphism
- Function doesn't care about the type
- Function works exactly the same for every type
- `a` is a type variable (It can be `Int`, `Bool`, `[Char]`, ...)
```haskell
pair :: a -> b -> (a, b)
pair x y = (x, y)
```

### Constrained Polymorphism
- `a` is still a type variable
- But it must belong to `Num` typeclass
- This restricts the possible types (Cannot be `Bool`, `[Char]`)
```haskell
add :: a -> a -> a
add x y = x + y  -- Error, cannot +

-- Constraint to Numeric
add :: Num a => a -> a -> a
add x y = x + y

-- Multiple constraints
add :: (Eq a, Num a) => a -> a -> a
```

---

# Typeclass
- Any data type that wants to be part of this `Shape` typeclass must provide implementations for these functions
```haskell
type Shape :: * -> Constraint
class Shape a where
	area :: a -> Double
	perimeter :: a -> Double
```

## Typeclass Instance
- If the data type `Circle` wants to be part of the `Shape` typeclass, must support the 2 functions
```haskell
data Circle = Circle Double deriving Show

instance Shape Circle where
	area (Circle r) = pi * r * r
	perimeter (Circle r) = 2 * pi * r
```

---

# Deriving
- Compiler automatically generate instance implementations for some standard typeclasses
```haskell
data Color = Red | Green | Blue deriving (Show, Eq, Ord)

-- Can use
show Red
Red == Green

-- Order by defined order
Red < Green < Blue
```

## With Deriving vs Without
```haskell
-- With deriving
data MyBook = Book1 | Book2 deriving Eq

-- Without deriving
data MyBook = Book1 | Book2

instance Eq MyBook where
	Book1 == Book1 = True
	Book2 == Book2 = True
	_ == _ = False
```

---

# Type Constraint
```haskell
-- Doesn't have any utility, raises error
add :: a -> a -> a
add x y = x + y

-- Constraint to Num type class, allow arithmetic operations
add :: Num a => a -> a -> a
add x y = x + y

-- Constraint to Ord type class, allow comparisons
addSpecial :: (Num a, Ord a) => a -> a -> a
addSpecial x y = 
	if x > 1
	then x + y
	else x
```

---

# Functions
```haskell
triple :: Integer -> Integer
triple x = x * 3

-- Anonymous function
(\x -> x * 3) :: Integer -> Integer
```

## Function Composition
```haskell
-- Without function composition
process xs = reverse (sort (filter even xs))

-- With function composition
process = reverse . sort . filter even
```

---

# Pattern Matching
```haskell
isItTwo :: Integer -> Bool
isItTwo 2 = True
isItTwo _ = False

-- Tuple
fst3 :: (a, b, c) -> a
fst3 :: (x, _, _) = x
```

## Case Expression
```haskell
area shape =
    case shape of
        Circle r -> pi * r * r
        Rect w h -> w * h
        _ -> 0
```

---

# Guards
```haskell
-- With guards
absolute :: Int -> Int
absolute x
    | x < 0     = -x
    | otherwise = x

-- Inline if
absolute :: Int -> Int
absolute x = if x <0 then -x else x
```

## Long Example
```haskell
bmiTell :: Double -> Double -> String
bmiTell weight height
    | bmi <= 18.5 = "underweight"
    | bmi <= 25.0 = "normal"
    | bmi <= 30.0 = "overweight"
    | otherwise   = "obese"
  where
    bmi = weight / height^2
```

---

# Recursion
```haskell
fac :: Int -> Int
fac 0 = 1
fac n = n * fac(n - 1)

-- Recursion on list
product :: Num a => [a] -> a
product[] = 1
product(x:xs) = x * product xs
```

## Mutual Recursion
```haskell
even :: Int -> Bool
even 0 = True
even n = odd (n - 1)

odd :: Int -> Bool
odd 0 = False
odd n = even (n - 1)
```

---

# Lists
```haskell
[1..5]  -- [1, 2, 3, 4, 5]
['t'..'z']  -- "tuvwxyz"
```

## Functions on List
```haskell
-- Return first few
take :: Int -> [a] -> [a]
take 3 [1..7]  -- [1, 2, 3]

-- Drop first few, Return rest
drop :: Int -> [a] -> [a]
drop 3 [1..7]  -- [4, 5, 6, 7]

-- Split list
splitAt :: Int -> [a] -> ([a], [a])
splitAt 2 [1..7]  -- ([1, 2], [3, 4, 5, 6, 7])
splitAt 0 [1, 2, 3]  -- ([], [1, 2, 3])
```

## takeWhile, dropWhile
```haskell
takeWhile :: (a -> Bool) -> [a] -> [a]
takeWhile (<3) [1..10]  -- take 2 [1..10]

dropWhile :: (a -> Bool) -> [a] -> [a]
dropWhile (<3) [1..10]  -- drop 2 [1..10]
```

---

# List Comprehensions
- `x <- [1..9]` is called generator
```haskell
-- In maths {x^2 ∣ x ∈ {1, .., 9}}
[x^2 | x <- [1..9]]  -- [1, 4, 9, 16, 25, ...]
map (\x -> x^2) [1..9]  -- Syntactic sugar for map function

-- With filters
[x | x <- [0..19], x `mod` 2 == 0]
```

## Nested Loops
```haskell
-- Example (Pythagorean triples)
[(a,b,c) |
    a <- [1..20],
    b <- [a..20],
    c <- [b..20],
    a*a + b*b == c*c
]
```

## List Functions
```haskell
-- Flatten a list of lists
concat :: [[a]] -> [a]
concat xss :: [x | xs <- xss, x <- xs]

-- Maps 2 lists to a list of pairs
zip :: [a] -> [b] -> [(a, b)]
zip ['a', 'b'] [1, 2, 3]  -- [('a', 1), ('b', 2)]
```

## Prime Numbers
```haskell
factors :: Int -> [Int]
factors n = [x | x <- [1..n], n `mod` x == 0]

prime :: Int -> Bool
prime n = factors n == [1, n]

primes :: Int -> [Int]
primes n = [x | x <- [2..n], prime x]
```






















## OR
```haskell
data Direction = North | South | East | West

go :: Direction -> String
go North = "Up"
go South = "Down"
go East = "Right"
go West = "Left"
```

## AND
```haskell
Person :: String -> Int -> Person
data Person = Person String Int

alice = Person "Alice" 25

-- Deconstruct
getName :: Person -> String
getName (Person name age) = name

getName (Person "Eve" 22)  -- Eve
```

## AND with OR
```haskell
data Shape
	= Circle Float  -- Radius
	| Rectangle Float Float  -- Width, height
	| Triangle Float Float Float  -- 3 sides

-- Deconstruct
area :: Shape -> Float
area (Circle r) = pi * r * r
area (Rectangle w h) = w * h
area (Triangle a b c) =
  let s = (a + b + c) / 2
  in sqrt (s * (s - a) * (s - b) * (s - c))
```

## Can be General
```haskell
data Pair a b = Pair a b
Pair 1 "hello"  -- Pair Int String
Pair True False  -- Pair Bool Bool
```

---

# Monads (Maybe, Either)
## Maybe
```haskell
data Maybe a = Nothing | Just a

safeDiv :: Int -> Int -> Maybe Int
safeDiv _ 0 = Nothing  -- Division by zero won't error
safeDiv x y = Just (x `div` y)
```

## Either
```haskell
data Either a b = Left a | Right b

safeDiv :: Int -> Int -> Either String Int
safeDiv _ 0 = Left "Cannot divide by zero"  -- Left for String
safeDiv x y = Right (x `div` y)  -- Right for Int
```

---

