## 'Easy'

* Without using brackets (i.e. ( ) and [] ) create a simple table with two rows and two fields

* Create a function that takes an integer as an argument and returns back the nxn identity matrix: https://en.wikipedia.org/wiki/Identity_matrix

e.g.
```
f 3 ->

1 0 0
0 1 0
0 0 1
```

* Write 3 q statements showing examples of 3 different uses of ! operator

* Write a function that will take an integer as an argument, and returns a table with that number of columns

.e.g:

```
f 3 ->

col1 col2 col3
--------------
```

* Create a table, t, with 1 column name containing 50 random numbers between 0 and 50. Add a column which will return yes or no based on whether or not the number is greater than 25

* Give five different ways of getting the "o" from the string "hello"

* Create a function to calculate the factorial of a positive integer n, e.g

```
f 4 -> 24
```

* Create a function that takes a positive integer n that will output a pyramid with n rows like the following:

```
f 5 ->

1
2 2
3 3 3
4 4 4 4
5 5 5 5 5
```

* Write a function that takes a string as an argument, and returns true if the string is a palindrome, ignoring whitespace, capitilization and punctuation marks (and all other non-alphanumeric characters)

e.g

```
f "racecar" -> 1b

f "Madam, I'm Adam" -> 1b

f "i" -> 1b

f "101" -> 1b

f "" -> 0b

f "hello world" -> 0b
```

* Create a function that will return true if a positive integer is a Narcissistic/Armstrong number, where the sum of each digit raised to the power of the number of digits, is equal to itself: https://en.wikipedia.org/wiki/Narcissistic_number

e.g

```
f 371 -> 1b

f 1 -> 1b

f 1741725 -> 1b

f 22 -> 0b
```

## 'Intermediate'

* Create a function to print out the first n rows of Pascals triangle: https://en.wikipedia.org/wiki/Pascal%27s_triangle

e.g

```
f 5 ->

1
1 1
1 2 1
1 3 3 1
1 4 6 4 1
```

* Write a function to store all keyboard input in a table with the time it was entered and whether it was executed successfully.

* Write a function that takes an integer argument which outputs the first n terms of the Fibonacci sequence, starting with 0: https://en.wikipedia.org/wiki/Fibonacci_number

 e.g:

```
f 1 -> 0
f 2 -> 0 1
f 10 -> 0 1 1 2 3 5 8 13 21 34
```

* Write a function to asynchronously send a string argument as a message to all connected handles at once, e.g:

```
f "hello world"
```

Demonstrate by sending to at least 2 handles


* Load the questions/resources/intermediate/quotes.csv and questions/resources/intermediate/trades.csv files as tables quotes and trades into a q session. Join the prevailing quote to each trade by sym 

* Fizzbuzz - Create a function that takes a positive integer n as its only argument, and returns all numbers between 1 and n, but replaces any numbers that are a multiple of 3 with "fizz" and numbers that are a multiple of 5 with "buzz" - it should also replace any numbers that are multiples of both 3 and 5 with "fizzbuzz"

.e.g

```

f 15 ->

1
2
"fizz" (will also accept `fizz)
4
"buzz" (can also take `buzz)
"fizz"
7
8
"fizz"
"buzz"
11
"fizz"
13
14
"fizzbuzz" (will also accept `fizzbuzz or `fizz`buzz)

```

* Create a function that takes a dictionary as an argument, which can run an aggregate function on specified columns in a table for a specified list of syms, - allow the user to input ` to return values for all syms.

 e.g.:

```
trades:([]time:12:00:00.000+1000*til 27;sym:27#3#`$'.Q.A;price:sum each(-1 0.5 -1.5 -0.5 2 -2 0 -0.25 1)cross 10 20 30;size:27#100 200);
quotes:update askPrice:bidPrice*1.2,askSize:bidSize from([]time:11:59:59.000+100*til 300;sym:300#3#`$'.Q.A;bidPrice:300#sum each(-1.5 0 -2 -1 1 -3.5 -1.25 -1.5 -2.25)cross 10 20 30;bidSize:300#100 200);

f `table`sym`columns`function!(`trades;`A`B;`price`size;avg) ->

sym| price    size
---| -----------------
A  | 9.805556 144.4444
B  | 19.80556 155.5556

f `table`sym`columns`function!(`quotes;`;`bidPrice;max) ->

sym| bidPrice
---| --------
A  | 11
B  | 21
C  | 31
```

* Create a function that returns a type map dictionary which maps each type character to the appropriate null value, and the uppercase type character to the appropriate type list

e.g 
```
map: f[];

map "i" -> 0Ni
map "I" -> `int$()

map "x" -> 0x00
map "X" -> `byte$()
```

The dictionary should have 36 entries.

* The business currently has a function which updates the sym in the following prices table to have the map in the map table using this:

```
map:([sym:`AAPL`GOOGL`MSFT`AAPL`GOOGL`MSFT];map:`AP1`GO1`MS1`AP2`GO2`MS2)
prices:([sym:`AAPL`GOOGL`MSFT]price:137.39 2053.63 242.01)
{[prices;map] delete map from update sym:map from prices lj map}[prices;map]

sym| price
---| -------
AP1| 137.39
GO1| 2053.63
MS1| 242.01

```

However, they want a function which returns a row for every map.
Create a function which updates the sym in the prices table to the map in the map table so that there is a row for each map so that the output is:
```
sym| price
---| -------
AP1| 137.39
AP2| 137.39
GO1| 2053.63
GO2| 2053.63
MS1| 242.01
MS2| 242.01
```

* Create a function that will take two tables, revenue and expenses, which will calculate the total commission for each salesperson in a beverage company.

Commission is 6.2% of profit per product, where profit equals revenue minus expenses, with no commission for any product to total less than 0. Round the total commission for each employee up to 2 decimal places. You do not have to display the second decimal if it is a 0, but kudos to anyone who can figure out how to do so.

e.g for the below:

```
revenue:flip`employee`product`revenue!(`Bob`Bob`Alice`Alice;`tea`coffee`tea`coffee;120 243 145 265);
expenses:flip`employee`product`expenses!(`Bob`Bob`Alice`Alice;`tea`coffee`tea`coffee;130 143 59 198);

f[revenue;expenses] ->

employee| commission
--------| ----------
Alice   | 9.49
Bob     | 6.2

```

Your input and expected output is the following:

```
revenue:flip`employee`product`revenue!(raze 4#/:`Dan`Will`Morris`James`Nick;raze flip 5#/:`tea`coffee`water`milk;190 325 682 829 140 19 14 140 1926 293 852 609 14 1491 56 120 143 162 659 87)

expenses:flip`employee`product`expenses!(raze 4#/:`Dan`Will`Morris`James`Nick;raze flip 5#/:`tea`coffee`water`milk;120 300 50 67 65 10 299 254 890 23 1290 89 54 802 12 129 430 235 145 76)

f[revenue;expenses] ->

employee| commission
--------| ----------
Dan     | 92.32
James   | 45.45
Morris  | 113.21
Nick    | 32.55
Will    | 5.21

```

## 'Hard'

* Without using lj or any other join explicitly (like uj, aj, ej, ij, rj, wj etc) write a function to join two tables that gives similar functionality to lj.


* Write a function that shows whether a symbol is defined and if it is a table, a dictionary, a function, projection or other variable

.e.g:

```
a:([]date:3#.z.d;sym:3?`3)
.namespace.blah:{x*y%100}[4]
b:("apples";1f;`oranges)

f `a -> "table"
f `b -> "other"
f `f -> "function"
f `.namespace.blah -> "projection"
f `c -> "undefined"
```

* Write a function to append this in memory table to an on-disk date partitioned hdb /path/to/hdb

```
px:([]date:raze 3#/:2021.01.01+til 3;sym:`a`b`c`a`b`c`a`b`c;price:9?100f)
```


* Find the volume weighted average price (VWAP) for each sym in 5 minute buckets in the resources/intermediate/trades.csv table and the time-weighted average spread (TWAS)in 5 minutes buckets in the resources/intermediate/quotes.csv table. Which sym(s) have the highest VWAP and TWAS?

* Given an integer list v that contains only unique elements, and another list w with the same elements as v but with one of them repositioned, create a function that will take the two lists as arguments, and return the index of the moved element, e.g.

```
v: 10 20 30 40 50 60 70
w: 10 20 70 30 40 50 60

f[v;w] -> 2 ( 70 moved to index 2)

Take care of ambiguous cases like the below, where the first element is the one that moved - in this case, assume that the number with the earliest index moved.

f[10 20 30;20 30 10] -> 2 (10 moved to index 2)
```
