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

* Create a function f that merges two lists by alternating the items from each, e.g: 

```
x:`a`b`c
y:1 2 3

0N!f[x;y] ->
(`a;1;`b;2;`c;3)
`a
1
`b
2
`c
3
```

* Create a function which take a single string argument that returns another function which appends that string as a suffix to its string argument

.e.g:

```
addLy: f"ly";
addLy "total" -> "totally"
addLy "actual" -> "actually"
```

* Create a function that will return the key of the input dictionary which maps to the second highest value. Assume that the values for each key are always unique.

e.g:

```
d1:`a`b`c`d!(10.50 5.2 9.3 6.7)
d2:enlist[`a]!enlist 5.0
d3:()!()

f d1 -> `c
f d2 -> `a
f d3 -> ()
```

* You and a friend are playing a board game where you take turns rolling a 6 sided die and advance your piece forward the number of tiles you rolled on the die. If your piece lands on the same tile as another player's piece, you both earn a bonus.

Create a function that when given both you and your friend's tile number, returns if its possible for you to earn a bonus when YOU roll the die.

Note - you cannot move backwards

e.g.

```
f[3;7] -> 1b
f[1;9] -> 0b
f[5;3] -> 0b
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

* Create  an 'avgvol' function that takes 3 arguments, a client or list of clients, a date, and a table with a schema like the following:

```
tradingvolume:([]date:`date$();time:`timestamp$();volume:`long$();client:`$()) 
```

which will return the average volume by hour for each client since the given date

Use the file `/home/shared/liveCode/tradingvolume.csv`

e.g

```
avgvol[`A;2021.03.03;tradingvolume] ->

client hour| volume
-----------| --------
A      9   | 254.7092
A      10  | 238.9934
A      11  | 267.6839
A      12  | 255.0345
A      13  | 222.7162
A      14  | 271.4789
A      15  | 236.8994
A      16  | 246.6434
A      17  | 254

avgvol[`A`B;2021.03.02;tradingvolume] -> 

client hour| volume
-----------| --------
A      9   | 253.1391
A      10  | 242.241
A      11  | 267.7588
A      12  | 252.0645
A      13  | 225.9024
A      14  | 269.5455
A      15  | 231.6599
A      16  | 247.7872
A      17  | 254
B      9   | 256.9385
B      10  | 278.6087
B      11  | 252.0059
B      12  | 251.92
B      13  | 250.3061
B      14  | 245.6882
B      15  | 244.1
B      16  | 252.9318
B      17  | 206.5

```

* Examine the table below showing votes in a mayoral race reported by different precincts:

```
t1:([precinct:1 2 3 4 5]Quimby:192 147 186 114 267;Dewey:28 90 12 21 13;Tortimer:206 312 121 408 382;AdamWest:37 21 38 39 29)
```

Create an 'election' function that returns the percentage of the total vote received by each candidate, and a column that indicates who won the election
 - the winner is whoever took >50% of the total vote.
 Also add a 'runoff' column which will indicate the two candidates with the highest and second highest share of the vote in the event where there is
 no candidate with overall votes above 50%
 
e.g

```
election t1 ->
 
candidate| pct        win runoff
---------| ---------------------
AdamWest | 0.06158468 0   0
Dewey    | 0.06158468 0   0
Quimby   | 0.3402178  0   0
Tortimer | 0.5366128  1   0
```

To test the runoff scenario, use this table:

```
t2:([precinct:1 2 3 4 5]Quimby:192 147 186 114 267;Dewey:28 90 12 21 13;Tortimer:206 312 121 108 382;AdamWest:37 21 38 39 29)

election t2 ->
 
candidate| pct       win runoff
---------| --------------------
AdamWest | 0.0694033 0   0
Dewey    | 0.0694033 0   0
Quimby   | 0.3834109 0   1
Tortimer | 0.4777825 0   1
```

* Create a function to calculate the 'additive persistence' of a positive integer. Add together the digits of a number and count the digits of the result - the additive persistence is the number of times you would need to repeat this process on the result until it becomes a single digit number: https://mathworld.wolfram.com/AdditivePersistence.html

e.g.

```
f 1 -> 1
f 13 -> 1
f 1234 -> 2
f 199 -> 3
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

* A bookshop's computer system stores its inventory in a particular format. 
Each book is given a unique code of 3 - 5 characters, followed by a space and then the quantity of that book inside the store
The first character of the code represents what category that book belongs to, e.g "A", "B", "G" etc

```
stocklist:("ABART 20";"CDEF 50";"BKWRK 25";"BTS 89";"DRTYM 60")
```

Write a 'categoryReport' function that takes two arguments, the stocklist and a list of catgeory characters, and returns a string of the total number of books
in the stocklist for each of those categories in this format : "<category0>:<quantity0> - <category1>:<quantity1> - ..."

```
categories:"ABCW"

categoryReport[stocklist;categories] ->

"A:20 - B:114 - C:50 - W:0"
```

* In the same bookshop from the question above, it is discovered that the stocklist has been saved in the wrong format, and is now compressed into a single string:

```
compressed:"ABART20CDEF50BKWRK25BTS89DRTYM60"
```

write an 'uncompress' function that takes this compressed string and outputs the original stocklist

```
uncompress compressed ->

("ABART 20";"CDEF 50";"BKWRK 25";"BTS 89";"DRTYM 60")
```

* Create a function 'squarecode' that creates a secret message from its input using the Square Code method. 
The function takes a string vector and strips out the spaces and special characters, then formats the
result into a square. The secret message is then made from the characters in the columns of the square, read from left to right.,
and then joined together with spaces between each 'word'.
If one side of the square would be longer than the either(i.e a rectangle), use the longer side as the width.
The result is not case sensitive

e.g

"have a nice day" ->
"haveaniceday" ->
"have"
"anic"
"eday" ->
"hae and via ecy"

```
s1:"have a nice day"
s2:"The quick brown fox jumped over the lazy dog"
s3:"Harder, better, faster, stronger"

squarecode s1 -> "hae and via ecy"
squarecode s2 -> "TCNMEA HKFPRZ EBOETY QRXDHD UOJOEO IWUVLG"
squarecode s3 -> "HBFSE AEATR RTSR DTTO EEEN RRRG"
```

* A detective arriving at the scene of a crime identifies 10 people who were present. One by one he asks them that out of the 10 people there, how many of them had they met before tonight? Their answers are represented by the list l below:

```
l: 5 3 0 2 6 2 0 7 2 5
```

These numbers seem suspicious however. Create a function 'deduce' that will return false if it is possible that someone is lying, or true otherwise. Assume that no one has given a wrong answer, and that if a person knows one person, that person will also know at least one person. Hint: the Havel-Hakimi algorithm will help you here: https://www.geogebra.org/m/ekajwspy 

```
deduce l -> 0b
```
Other test cases:

```
l2:3 1 2 3 1 0
l3:1 1
l4:6 0 10 10 10 5 8 3 0 14 16 2 13 1 2 13 6 15 5 1
l5:1 0
l6:0 0

deduce l2 -> 1b
deduce l3 -> 1b
deduce l4 -> 0b
deduce 15 -> 0b
deduce l6 -> 1b
```
