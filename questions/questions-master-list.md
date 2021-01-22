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

* Write a function that takes an integer argument which outputs the first n terms of the fibonacci sequence, starting with 0 e.g:

```
f 1 -> 0
f 2 -> 0 1
f 10 -> 0 1 1 2 3 5 8 13 21 34
```

* Write a function to asynchronously send a string argument as a message to all connected handles at once, e.g:

`f "hello world"`

Demonstrate by sending to at least 2 handles


* Load the questions/resources/intermediate/quotes.csv and questions/resources/intermediate/trades.csv files as tables quotes and trades into a q session. Join the prevailing quote to each trade by sym 

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

`px:([]date:raze 3#/:2021.01.01+til 3;sym:`a`b`c`a`b`c`a`b`c;price:9?100f)`


* Find the volume weighted average price (VWAP) for each sym in 5 minute buckets in the resources/intermediate/trades.csv table and the time-weighted average spread (TWAS)in 5 minutes buckets in the resources/intermediate/quotes.csv table. Which sym(s) have the highest VWAP and TWAS?
