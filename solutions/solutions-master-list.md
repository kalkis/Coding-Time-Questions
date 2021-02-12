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

* Create a function to calculate the factorial of a positive integer n, e.g

```
f 4 -> 24
```

> ```
> f:{prd 1+til x}
> ```
> Bonus: create a solution using explicit recursion with .z.s

* Create a function that takes a positive integer n that will output a pyramid with n rows like the following:

```
f 5 ->

1
2 2
3 3 3
4 4 4 4
5 5 5 5 5
```

> Multiple ways:
>
> ```
> f1:{{x#x}'[1+til x]}
>
> f2:{.'[#;2#/:1+til x}
> ```

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

> Sanitize the string and strip out any characters that aren't in .Q.a and .Q.n. (or use .Q.an for both)
> 
> If the number of characters after sanitizing is 0, return false. Otherwise, check if the string is the same as its reverse
> 
> ```
> f:{[x]$[not count x:x where (x:lower(),x)in .Q.a,.Q.n;0b;x~reverse x]}
> ```

* Create a function that will return true if a positive integer is a Narcissistic/Armstrong number, where the sum of each digit raised to the power of the number of digits, is equal to itself: https://en.wikipedia.org/wiki/Narcissistic_number

e.g

```
f 371 -> 1b

f 1 -> 1b

f 1741725 -> 1b

f 22 -> 0b
```

> Using numbers as the arguments for vs can split a number into the digits in the base of the left argument. Raise each to the power of the number of digits and add together
> 
> ```
> f:{x=sum xexp\:[n;count n:10 vs x]}
> ```
> Another method is to cast the number to a string, then cast each character back to an integer and then do the same
> ```
> f:{x=sum xexp\:[n;count n:"J"$/:string x]}
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

* Fizzbuzz - Create a function that takes a positive integer n as its only argument, and returns all numbers between 1 and n, but replaces any numbers that are a multiple of 3 with "fizz" and numbers that are a multiple of 5 with "buzz" - it should also replace any numbers that are multiples of both 3 and 5 with "fizzbuzz".

.e.g

```
f 15 ->

1
2
"fizz"
4
"buzz"
"fizz"
7
8
"fizz"
"buzz"
11
"fizz"
13
14
"fizzbuzz"
```

> add both fizz and buzz to the string to skip checking x mod 15; if its not a multiple of either number, the string will be empty
> ```
> f:{[x]{r:"";r,:$[0=x mod 3;"fizz";""];r,:$[0=x mod 5;"buzz";""];:$[not count r;x;r]}each 1+ til x}
> ```


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
> Make sure you handle the cases for 1 column, all syms etc correctly
> Can use (func;)notation to make sure you create a list for each column
> ```
> f:{[x]
>  s:$[`~x`sym;exec asc distinct sym from x`table;x`sym];
>   :?[x`table;enlist(in;`sym;enlist s);{x!x}(),`sym;((),x`columns)!(x`function;)@/:(),x`columns];
> };
> ```

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

> Generate nulls for the types you can't make by casting with the characters first, then use .Q.t for the rest:
> 
> ```
> f:{
>   map:"bBcCgGsSxX"!(0b;"B"$();" ";"";0Ng;"G"$();`;`$();"x"$0N;"X"$());
>   :map,raze{(x;upper x)!(x$0N;upper[x]$())}each .Q.t except" bcgsx";
>  };
>  
> ```

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

> This is essentially making the inverse of the map table and then updating the sym column. A few approaches:
> 
> ```
> f:{[map;prices]`sym xasc 1!(enlist[`map]!enlist`sym)xcol delete sym from map lj prices}
> ```
> or using exec
> 
> ```
> f:{[map;prices]`sym xasc exec([sym:map]price)from map lj prices}
> ```
> 
> or to avoid re-sorting the price table, group the map table by sym and then update the sym in the price table
> ```
> f:{[map;prices]sym2map:(exec distinct sym from map)!(exec map from map)value group exec sym from map;1!ungroup update sym:sym2map[sym]from prices}
> ```

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

>Join the expenses for each product to the revenue by employee/product columns, then calculate the profit. Caluclate 6.2% of the profit to get commission for each product, or set to 0 where profit was negative. Get the total commission by summing by employee, then use .Q.f to round it to 2 decimal places 
>
> ```
> f:{[revenue;expenses]
>  commission:revenue lj`employee`product xkey expenses;
>  commission:update profit:revenue-expenses from commission;
>  commission:update commission:(6.2*profit%100)|0 from commission;
>  :select "F"$.Q.f[2;sum commission] by employee from commission;
>  }
>
> ```
>
> Result for the question input:
> 
> ```
> employee| commission
> --------| ----------
> Dan     | 92.32
> James   | 45.45
> Morris  | 113.21
> Nick    | 32.55
> Will    | 5.21
> ```

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

* Given an integer list v that contains only unique elements, and another list w with the same elements as v but with one of them repositioned, create a function that will take the two lists as arguments, and return the index of the moved element, e.g.

```
v: 10 20 30 40 50 60 70
w: 10 20 70 30 40 50 60

f[v;w] -> 2
( 70 moved to index 2)
```

Take care of ambiguous cases like the below, where the first element is the one that moved - in this case, assume that the number with the earliest index moved.

```
f[10 20 30;20 30 10] -> 2
(10 moved to index 2)
```

> you could do:
> ```
> f:{first where not x=y}
> ```
> but this would not handle the second ambiguous case
> ```
> f:{[x;y]mvd:where x<>y;mv1:first mvd;mv2:last mvd;$[00b~x[mv1]~/:@[y;(mv1;mv2)];mv1;mv2]}
> ```
> this gets indices for all elements that differ between the two lists/"moved" (mvd), then we get the first and last of the indices and get the corresponding values in y . we then compare the number at the first "moved" index in the original list x. if it is different from both these numbers, we can say that the first "moved" number is the one that changed - otherwise, it's the one at the end of the "moved" list, i.e this is the second case
