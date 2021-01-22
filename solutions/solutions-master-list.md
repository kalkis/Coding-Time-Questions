## 'Easy'

* Without using brackets (i.e. ( ) and [] ) create a simple table with two rows and two fields
> ```
> q)2#enlist`x`y!1 2
> x y
> ---
> 1 2
> 1 2
> ```

* Create a function that takes an integer as an argument and returns back the nxn identity matrix: https://en.wikipedia.org/wiki/Identity_matrix

e.g.
```
f 3 ->

1 0 0
0 1 0
0 0 1
```

> Two ways:
>
> with booleans:
>
> ```
> q)f1:{{x=/:x}til x}
> q)f1 5
> 10000b
> 01000b
> 00100b
> 00010b
> 00001b
> 
> with integers, single function:
>
> q)f2:{(2#x)#1,x#0}
> q)f2 5
> 1 0 0 0 0
> 0 1 0 0 0
> 0 0 1 0 0
> 0 0 0 1 0
> 0 0 0 0 1

* Write 3 q statements showing examples of 3 different uses of ! operator

> Possible uses:
> ```
>  Making a dictionary: `a`b`c!1 2 3
>  internal functions: -11! (tickerplant logs) -9!/-8! (convert to/from bytes) -1! (standard output)
>  functional update/delete:
>  "update flag:1b from t where price>avg price" -> ![t;enlist(>;`price;(avg;`price));0b;enlist[`flag]!enlist 1b]
>  "delete flag from t" -> ![t;();0b;enlist`flag]
>  keying n columns of table/unkey table n!tab 0!tab
>  enumerate list: x:`a`b`c`d; `x!0 2 3 -> `x$`a`c`d
>  display and return: 0N!1+1 -> 2 2
> ```

* Write a function that will take an integer as an argument, and returns a table with that number of columns

.e.g:

```
f 3 ->

col1 col2 col3
--------------
```
> Something like:
> 
> ```
> f:{enlist(`$"col",/:string til x)!til x}
> f 1 ->
> col0
> ----
> 0
> 
> f 2 ->
> 
> col0 col1
> ---------
> 0    1
> ```
> or 
> ```
> f:{flip(`$"col",/:string til x)#()!()}
> ```
> for an empty table

* Create a table, t, with 1 column name containing 50 random numbers between 0 and 50. Add a column which will return yes or no based on whether or not the number is greater than 25

> Make use of vector conditional:
> ```
> t:([]nums:50?50)
> update flag:?[nums>25;`yes;`no]from t
> nums flag
> ---------
> 8    no
> 33   yes
> 2    no
> 24   no
> 48   yes
> 5    no
> 8    no
> 31   yes
> 18   no
> 47   yes
> 26   yes
> 40   yes
> 29   yes
> 45   yes
> 38   yes
> 41   yes
> 48   yes
> 29   yes
> 0    no
> 32   yes
> ..
> ```


* Give five different ways of getting the "o" from the string "hello"

> Lots of different ways:
> ```
>  -1#"hello" -> ,"o"
>  4_"hello" -> ,"o"
>  1#reverse "hello" -> ,"o"
>  -4_reverse "hello" -> ,"o"
>  "hello" 4 -> "o"
>  @["hello";4] -> "o"
>  "hello"@4 -> "o"
>  "hello"where"hello"="o" -> ,"o"
>  last hello -> ,"o"
>  first reverse hello -> ,"o"
> ```

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

> ```
> q)f:{{prior[+]x,0}\[x-1;1]}
> q)f 5
> 1
> 1 1
> 1 2 1
> 1 3 3 1
> 1 4 6 4 1
> ```
>
> https://code.kx.com/q/ref/prior/
>
> Kudos for whoever can format it nicely as a triangle

* Write a function to store all keyboard input in a table with the time it was entered and whether it was executed successfully.

> Use .z.pi:
>
> ```
> inputRecord:([]user:`;timestamp:`timestamp$();input:();success:())
> .z.pi:{success:@[value;x;{`$"'",x}];`inputRecord upsert (.z.u;.z.p;x;success);.Q.s value x}
> ```


* Write a function that takes an integer argument which outputs the first n terms of the Fibonacci sequence, starting with 0: https://en.wikipedia.org/wiki/Fibonacci_number

 e.g:

```
f 1 -> 0
f 2 -> 0 1
f 10 -> 0 1 1 2 3 5 8 13 21 34
``` 

> some variation on the below:
> ``` 
> f:{x#{x,sum -2#x}/[x;0 1]}
> ```
> In this case, since we start with two numbers the inner function will return a bit more than x terms, so use # to limit the output

* Write a function to asynchronously send a string argument as a message to all connected handles at once, e.g:

```
f "hello world"
```

Demonstrate by sending to at least 2 handles

>  2 examples:
> ```
>  f1:{[x] -25!(key .z.W;x)}
> 
>  f2:{[x]neg[key .z.W]x}
> ```
>  Make sure to set up .z.ps on the connected processes to display the message correctly, something like:
> ```
>  .z.ps:{show x}
> ```
> Bonus: Difference between -25! and using neg[handles]? how to make sure -25! is defined?

* Load the questions/resources/intermediate/quotes.csv and questions/resources/intermediate/trades.csv files as tables quotes and trades into a q session. Join the prevailing quote to each trade by sym 

> Classic use case for aj as-of join - see solutions/resources/intermediate/joined.csv for the expected output
> 
>  First parse the files:
>  ```
>  quotes:("PSSFFJJ";csv)0: `:quotes.csv
>  trades:("PSSFJ";csv)0: `:trades.csv
>  ```
>  Then run the join:
> ```
>  aj[`sym`time;trades;quotes]
> ```
> Bonus: how to optimize for much larger data sets?

## 'Hard'

* Without using lj or any other join explicitly (like uj, aj, ej, ij, rj, wj etc) write a function to join  two tables that gives similar functionality to lj.

> Key x on key columns of y, then take the rows from y only where the key exists in x. vertical join the two then fill null values with those from x and return unkeyed
> 
> ```
> q)f5:{x:cols[key y] xkey x;y:key[x]#y;0!x^x,'y}
> q)t1:([]k:1 2 3;v:`a`b`c)
> q)t2:([k:1 3]v:`a`d)
> q)f5[t1;t2]
> k v
> ---
> 1 a
> 2 b
> 3 d
> ```
> 

* Write a function that shows whether a symbol is defined and if it is a table, a dictionary, a function, or a variable

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

> Using error trap:
> ```
> f:{@[{[x]d:98 99 100 104h!("table";"dictionary";"function";"projection");$[""~d y:type value x;"other";d y]};x;"undefined"]}
> ```
> Could also check root namespace:
> ```
> f:{[x]d:98 99 100 104h!("table";"dictionary";"function";"projection");$[x in key`.;$[""~d y:type value x;"other";d y];"undefined"]}
> ```
> 
> To be more thorough, how would you traverse all namespaces with variable depths?
> 
> .e.g .namespace.depth1.depth2 & .blah.func?
> 
> Get brownie points with the interviewer for showing you have considered edge cases in your answer

* Write a function to append this in memory table to an on-disk date partitioned hdb /path/to/hdb

```
px:([]date:raze 3#/:2021.01.01+til 3;sym:`a`b`c`a`b`c`a`b`c;price:9?100f)
```

> Saving to disk:
> 
> Could save data for each date to partitions directly after enumerating:
> 
> ```
> f:{[t;d;p;x](` sv p,`$string d,t,`)set .Q.en[p]`date xasc?[x;enlist(=;`date;d);0b;()!()]}`
> 
> q)f[`px;;`:hdb;px]each asc distinct px`date
> `:hdb/2021.01.01/px//`:hdb/2021.01.02/px//`:hdb/2021.01.03/px//
> 
> [dcreighton@homer ~]$ q hdb
> KDB+ 4.0 2020.06.18 Copyright (C) 1993-2020 Kx Systems
> l64/ 24()core 128387MB dcreighton homer 127.0.1.1 EXPIRE 2021.06.30 AquaQ #59946
> 
> q)tables[]
> ,`px
> q)px
> date       sym price
> -----------------------
> 2021.01.01 a   39.27524
> 2021.01.01 b   51.70911
> 2021.01.01 c   51.59796
> 2021.01.02 a   40.66642
> 2021.01.02 b   17.80839
> 2021.01.02 c   30.17723
> 2021.01.03 a   78.5033
> 2021.01.03 b   53.47096
> ```

* Find the volume weighted average price (VWAP) for each sym in 5 minute buckets in the resources/intermediate/trades.csv table and the time-weighted average spread (TWAS)in 5 minutes buckets in the resources/intermediate/quotes.csv table. Which sym(s) have the highest VWAP and TWAS?

>   For VWAP:
>  ```
>   trades1:select vwap:size wavg price by sym, bucket:5 xbar time.minute from trades;
>  ```
>   For TWAS:
>  ```
>   quotes1:update spread:ask-bid from quotes;
>   quotes1:select twas:time wavg spread by sym, bucket:5 xbar time.minute from quotes;
>  ```
>   Then to get the highest values for VWAP and TWAS from each table
>  ```
>   select from trades1 where vwap=max vwap
>   sym  bucket| vwap
>   -----------| -----
>   GOOG 16:25 | 41.43
>   select from quotes1 where twas=max twas
>   sym  bucket| twas
>   -----------| ----------
>   MSFT 16:00 | 0.05333333
> ```
