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

> Pretty straightforward. Vertical join the two lists to pair each element, then raze to make it one list

```
f:{raze x,'y}
```

* Create a function which take a single string argument that returns another function which appends that string as a suffix to its string argument

.e.g:

```
addLy: f"ly";
addLy "total" -> "totally"
addLy "actual" -> "actually"
```

> q projections make this pretty straightforward compared to some other languages. Return a projection of a function that appends one string to another

```
f:{{y,x}x}
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

> Sort the dictionary then isolate the second highest result by dropping the highest result. Use ? to reverse lookup the dictionary for the relevant key

```
f:{x?$[1=count x;first x;last -1_asc x]}
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

> So long as your friend's tile position is greater than yours and within 6 tiles, you have a chance of landing on their tile on your turn

```
f:{[x;y](x<y)&y<=x+6}
```

> Another solution is to calculate both you and your friends possible positions after rolling, then find any overlap - if your position is also less than theirs, you have a chance of getting the bonus on your turn

```
f:{[x;y](x<y)&0<count(inter). (x;y)+\:1+til 6}
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

> This is a fairly straightforward select, where you need to aggregate the average by client and in 1 hour buckets. the 'hh' component of the timestamp will give you the hour as an integer. Be sure to include the date condition first in the where clause to ensure it would work with a partitioned table - the question is also looking for the average since the date, i.e on that date and all dates up to the current date, in order to match the output
> This doesn't strictly need to be a functional select since there are no dynamic columns involved, but it doesn't hurt to get more practice creating them

```
tradingvolume: ("DPJS";enlist csv)0: `:/home/shared/liveCode/tradingvolume.csv;
avgvol:{[clients;date;tradingvolume]
  :?[tradingvolume;
     ((>=;`date;date);(in;`client;enlist clients));
     `client`hour!(`client;(xbar;1;`time.hh));
     enlist[`volume]!enlist(avg;`volume)];
};
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

> The calculation is much easier if you unpivot the table first.
> This is straightforward once you remember that keyed tables are just dictionaries, so you can use key and value
> to pull out the name and vote of each candidate from each row of the table, and make these the values in a new dictionary.
> Provided the keys are the same for each entry, this will collapse into another keyed table grouped by precinct
> which you can ungroup to denormalize.
> Then its a matter of summing the votes for each candidate and dividing by the total overall, and add an indicator
> for who got over 50%.
 
> For the runoff scenario, you'll need to rank the percentage of votes for each candidate - idesc will return the indices
> of each element in the list where it would be if put in descending order, then update for the rows where that value is 0 or 1,
> which can save you the overhead of actually sorting the table. Then only mark runoff if no value met the win condition
> You could also use rank or iasc function, which would put these values at the end of the list, but you would need to keep track of the number of values overall

```
election:{[x]
    total:exec sum votes from x:ungroup{`candidate`votes!(key x;value x)}each x;  / unpivot the table
    x:select pct:sum[votes]%total by candidate from x;                                           / mark the majority winner
    :update win:pct>0.5,runoff:(idesc[pct]<=1)&not[any pct>0.5]from x;            / if no majority, calculate two highest candidates for runoff  
  };
```

> In one line, if you hate semi-colons/being able to read your code:

```
election:{[x]update win:pct>0.5,runoff:(idesc[pct]<=1)&not[any pct>0.5]from select pct:sum[votes]%(exec sum votes from x)by candidate from x:ungroup{`candidate`votes!(key x;value x)}each x}
```

* Create a function to calculate the 'additive persistence' of a positive integer. Add together the digits of a number and count the digits of the result - the additive persistence is the number of times you would need to repeat this process on the result until it becomes a single digit number: https://mathworld.wolfram.com/AdditivePersistence.html

e.g.

```
f 1 -> 1
f 13 -> 1
f 1234 -> 2
f 199 -> 3
```

> This solution makes use of the 'while' overload of scan, since this will return the intermediate values of the sum of the digits until you reach a single digit result.
> Then we can just count the number of intermediates, minus the first result since it will always be the original input.
> Use |/or operator to return the larger result if subtracting 1 brings it below 1 to satisfy the cases where additive persistence is 1

```
f:{1|-1+count{sum 10 vs x}\[{1<count 10 vs x};x]}
f 1234 -> 2
f 1 -> 1
f 199 -> 3
```

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

> This solution transforms the input into a table which is easier to work with. Split each line into code/quantity, flip the dictionary and then extract the first letter to get the category, then aggregate the quantities.
> Map the categories passed to the function back to the table, fill with 0 if there were no books in that category, then join each code/total quantity pair in the table with a colon using 0:, and join again with a hyphen to make a single string

```
categoryReport:{[stocklist;categories]
   stocklist:select sum quantity by category from                   / aggregate
                   update category:first'[code],"J"$quantity from   / extract category and cast quantity to long
                   flip`code`quantity!flip" "vs'stocklist;          / split up the code and quantity, make into a dict then a table
  :" - "sv 1_":"0:0^([]category:categories)lj stocklist;            / map back to input category list,
}; 
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

> My approach here was to find the indices of the boundaries of each pair based on the fact that a letter preceded by a number indicates the start of the next pair.
> Use .Q.A to find the indices of the code component, and then deltas function to find the indices where the next code starts.
> We can then subtract the previous index from the next, appending the length of the input string to ensure we get the last code, in order to find the lengths of each code/quantity pair.
> Place 0 at the start of that first list, then vertical join with the lengths in order to build the indices of the start of each substring within the compressed string.
> We can then use these as the arguments for the 'slice' overload of sublist keyword: https://code.kx.com/q/ref/sublist/ to extract the pairs. Then cut each substring where the numerical part starts, so we join them back again with a space to remake the original sublist:

```
uncompress:{[compressed]
  c:c where 1<deltas c:where compressed in .Q.A;  / indices where each code/quantity pair ends - if difference larger than 1, previous character was a number
  c:(0,c),'prior[-]c,count compressed;            / subtract each index from the last to get length of each code/quantity pair, join with index bounds
  :" "sv'cut'[count each where each c in\:.Q.A]c:c sublist\:compressed; / use slice overload of sublist to extract each one, split up code and quantity, join with spaces
};
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

> Sanitize the input, then find the square root of the number of characters in the result.
> Round this number up to get n, the longest side of the square, then cut the sanitized string to make the
> square or rectangle with width n.
> Index into first character of each string to get the first 'word' of the secret message,
> and repeat for the other columns of the square.
> Strip out any extra whitespace, then join the resulting matrix with spaces
> to form the full secret message:

```
f:{[s]s:(n:ceiling sqrt count s)cut s:s where (s:upper s)in .Q.nA;" "sv trim s@\:/:til n}
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
> More info on this algorithm: https://en.wikipedia.org/wiki/Havel%E2%80%93Hakimi_algorithm

```
 deduce:{[l]n:first l:desc (l,:())except 0;if[not count l;:1b];N:count l:1_l;$[n>N;0b;.z.s l-raze[(n;N-n)#'1 0]]}
```
 
> if after removing all 0s from the list, we have nothing, we return true
> otherwise, we sort the list in descending order and take the first value as n
> we then drop this first value from the list, and count the remaining elements in the new list as N
> if n is bigger than N, the witnesses answers are inconsistent
> otherwise, we subtract 1 from the first n elements of l, and repeat the process with the result
> the aim is to reduce the result to the base cases of 1 1, 1 0 or 0 0 - in the case where there are two witnesses, the only two truthful scenarios are that both witnesses knew each other (1 1)or neither did (0 0)
> This is a bit trickier to implement cleanly with adverbs due to the number of conditions you need to check
