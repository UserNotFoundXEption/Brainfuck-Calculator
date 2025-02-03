- ! - current pointer position before the operation.
- cx - cell x
- c*x - cell -x
- S, A - temporary cells
- X, Y - numbers of interest
- I - input cell



### Input of a number. One at a time. Takes 3 cell to the right.
```brainfuck
               !
 [-]>[-]+      0000  clear c1
                !
   [[-]        0100  clear c2
                !
   >[-],       S000  clear c3; INPUT
                 !
       [       S0I0
           +[        check for eof
               -----------     check for newline
                [            
                                                  !
                   >[-]++++++[<------>-]<--     S0X0   clear c4 and use it as a buffor for substraction of 38 from k3
                                                  !
                   <<[->>++++++++++<<]          S0Y0   multiply c1*10 and move it to c3
                                 !
                   >>[-<<+>>]    00S0      move c3 to c1; we've got a sum
                       !
             <+>]    S000  add 1 to c2 in order to exit in case of newline
           ]
       ]
   <] move to c2; go to INPUT
   <  end of input; move to c1
```
```brainfuck
[-]>[-]+[[-]>[-],[+[-----------[>[-]++++++[<------>-]<--<<[->>++++++++++<<]>>[-<<+>>]<+>]]]<]<
```

### Output of a number. The number is destroyed
```brainfuck
    start in c2 with X in c2: 00X00000000
<<[-]>[-]>>+  clear c0 and c1; move to c3
  [[-]<   clear c3 and move to X
        [->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<   substract 1 from X and 9 from c3; if X|10 go to END
           [->[-] substract the last time and move to c3
            >>+>+<<<] add to c5 and c6; move to c3
          ]]]]]]]]< END; move to X;
        ]  repeat untill X == 0; c3 is the last digit; c5 and c6 = X/10
    >>[>] you're in c4 = 0
    ++++++[-<++++++++>]>> add 48 to c3 and move to c5; c5 is the new X
  ] digits are in reversed order in cells c(3n)
<<< you end up in the last cell + 3; go back to the last digit
[.[-]<<<] output; clear cell; go to the next digit;
```
```brainfuck
<<[-]>[-]>>+[[-]<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->[-]>>+>+<<<]]]]]]]]]<]>>[>]++++++[-<++++++++>]>>]<<<[.[-]<<<]
```
With newline:
```brainfuck
<<[-]>[-]>>+[[-]<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->[-]>>+>+<<<]]]]]]]]]<]>>[>]++++++[-<++++++++>]>>]<<<[.[-]<<<]++++++++++.[-]
```

### Addittion. Requires 1 cell to the right
```brainfuck
XY0
[->>+<<] move X to c2
> move to Y
[->+<] move Y to c2
> move to c2
```
```brainfuck
[->>+<<]>[->+<]>
```

### Multiplication. Requires 2 cell to the right
```brainfuck
XY00; c2 temp; c3 result
[->  substract 1 from X and move to Y
copy Y to c2 and add it to c3
[->+>+<<] move Y to c2 and c3
> move to c2
[-<+>] move c2 to Y
next iteration of addition
<< move to X
] multiplication finished
>[-] clear Y
>>  move to c3
```
```brainfuck
[->[->+>+<<]>[-<+>]<<]>[-]>>
```

### substraction of X from Y
```brainfuck
XY
> move to Y
[-<->] substract 1 from both
< move to X
```
```brainfuck
>[-<->]<
```

### Division. Requires 3 cells to the left and 4 cell to the right
```brainfuck
XYS000
[
  >>>>+  add 1 to c4
  [      
    <<<
    [   you're in Y
       <-  substract from X
       >>>>-<<<< substract from c4; move to X
       [>>>>+>]  if X != 0 then add to c4 to continue
       you're in X if X == 0 and c4 == 0; Otherwise you're in c5
       <<<< You're either in c*3 or Y
       [<<<<<] If you're in Y then move to c*3
       >>>>>>>+      move to c3 which is a copy of Y
       <<-    you're in Y
    ]   you're in Y
    >>[<<+>>-] move c3 to Y
    <+>>    add 1 to Q and move to c4
  ] S has result but it can be 1 too big
]

<<<<  move to X
[ if X != 0 then divide 1 from S
  >
  [>>+<<<+>-]>>[<<+>>-] copy Y to X
   <-[<]  divide 1 from S; go full left to c*1 to leave the loop
]

>[>]< if the loop didn't happen then you're in X; go right; go full right; go left to the result
```
```brainfuck
[>>>>+[<<<[<->>>>-<<<<[>>>>+>]<<<<[<<<<<]>>>>>>>+<<-]>>[<<+>>-]<+>>]]<<<<[>[>>+<<<+>-]>>[<<+>>-]<-[<]]>[>]<
```

### Power. Requires 4 cells to the right
```brainfuck
XY0000; start in X; X base; Y exponent; c2 c3 and c4 temp for copying X; c5 result

copy X to c3
[->>>+>+<<<<]          move X to c3 and c4
>>>>[-<<<<+>>>>]       move c4 to X
<<<- substract 1 from Y so it doesn't exponentiate too much
[
  <
  [->>+>>+<<<<] move X to c2 and c4
  >>>>[-<<<<+>>>>] move c4 to X
  <<[->[->+>+<<]>[-<+>]<<]   multiply c2 and c3; move to c5
  >[-]  clear c3
  >>[-<<+>>] move c5 to c3
<<<<-]    move to Y and substract 1 from Y
>> you're in c3; move to c5
```
```brainfuck
[->>>+>+<<<<]>>>>[-<<<<+>>>>]<<<-[<[->>+>>+<<<<]>>>>[-<<<<+>>>>]<<[->[->+>+<<]>[-<+>]<<]>[-]>>[-<<+>>]<<<<-]>>
```

### Move X cells to the right. X is destroyed
```brainfuck
[
[->+<] move X to the right
>- move to X and divide 1 from X
] you've in a cell X cells to the right of original X
```
```brainfuck
[[->+<]>-]
```

### Find non-zero cell to the left. You have to start in 0
```brainfuck
You're moving digit 1 untill you find a non-zero cell and then clear the 1
+[<    add 1 to enter loop and go left
[>-]> if current cell == 0 then move to the 1; else clear the 1 and go right
[-<+>]< move the 1 left; if there's no "1" go right from the non zero cell and leave the loop
]< go to the non zero cell
```
```brainfuck
+[<[>-]>[-<+>]<]<
```

| symbol | ASCII | cell |
|---|---|---|
| * | 42 | 45 |
| + | 43 | 46 |
| - | 45 | 48 |
| / | 47 | 50 |
| ^ | 94 | 97 |

### Calculator
```brainfuck
Put the first number in c0
[-]>[-]+[[-]>[-],[+[-----------[>[-]++++++[<------>-]<--<<[->>++++++++++<<]>>[-<<+>>]<+>]]]<]
You're in c1; put the symbol in c1; input includes newline so put it in c2
,>,
Put the second number in c2
[-]>[-]+[[-]>[-],[+[-----------[>[-]++++++[<------>-]<--<<[->>++++++++++<<]>>[-<<+>>]<+>]]]<]
You're in c3; vector looks like this: X*Y; move to c1 and move the symbol to c3
<<[->>+<<]
Move to symbol; use it to move to cell A plus 3; A means the symbol's representation in ASCII; add 1
>>[[->+<]>-]+
Move to Y (c2)
<+[<[>-]>[-<+>]<]<
Move Y to c1; stay in c2
[-<+>]

LOOK FOR OPERATION
Add 6 to c2 and c3
++++++>++++++<
Multiply them to get 36 in c4; move to c4
[->[->+>+<<]>[-<+>]<<]>[-]>>
move 36 cell to the right
[[->+<]>-]
You're in c41

MULTIPLICATION
Move to c45 to see if we're multiplying
>>>>[
If so then move to Y (First non zero cell to the left)
<+[<[>-]>[-<+>]<]<
< move to X
Multiplication
[->[->+>+<<]>[-<+>]<<]>[-]>>
Output
<<[-]>[-]>>+[[-]<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->[-]>>+>+<<<]]]]]]]]]<]>>[>]++++++[-<++++++++>]>>]<<<[.[-]<<<]++++++++++.[-]
]

ADDITION
Move to c46 to see if we're adding
>[
If so then move to Y (First non zero cell to the left)
<+[<[>-]>[-<+>]<]<
< move to X
Addition
[->>+<<]>[->+<]>
Output
<<[-]>[-]>>+[[-]<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->[-]>>+>+<<<]]]]]]]]]<]>>[>]++++++[-<++++++++>]>>]<<<[.[-]<<<]++++++++++.[-]
]

SUBSTRACTION
Move to c48 to see if we're substracting
>>[
If so then move to Y (First non zero cell to the left)
<+[<[>-]>[-<+>]<]<
Substraction; don't move to X because we already end up there
[-<->]<
Output
<<[-]>[-]>>+[[-]<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->[-]>>+>+<<<]]]]]]]]]<]>>[>]++++++[-<++++++++>]>>]<<<[.[-]<<<]++++++++++.[-]
]

DIVISION
Move to c50 to see if we're dividing
>>[
If so then move to Y (First non zero cell to the left)
<+[<[>-]>[-<+>]<]<
< move to X
Division
[>>>>+[<<<[<->>>>-<<<<[>>>>+>]<<<<[<<<<<]>>>>>>>+<<-]>>[<<+>>-]<+>>]]<<<<[>[>>+<<<+>-]>>[<<+>>-]<-[<]]>[>]<
Output
<<[-]>[-]>>+[[-]<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->[-]>>+>+<<<]]]]]]]]]<]>>[>]++++++[-<++++++++>]>>]<<<[.[-]<<<]++++++++++.[-]
]

POWER
Add 6 to c50 and 7 to c51
++++++>+++++++<
Multiply them to receive 42 in c53; move to c53
[->[->+>+<<]>[-<+>]<<]>[-]>>
Move 42 cells to the right; end up in c95
[[->+<]>-]
Move to c97 to see if we're exponentiating
>>[
If so then move to Y (First non zero cell to the left)
<+[<[>-]>[-<+>]<]<
< Move to X
Power
[->>>+>+<<<<]>>>>[-<<<<+>>>>]<<<-[<[->>+>>+<<<<]>>>>[-<<<<+>>>>]<<[->[->+>+<<]>[-<+>]<<]>[-]>>[-<<+>>]<<<<-]>>
Output
<<[-]>[-]>>+[[-]<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->[-]>>+>+<<<]]]]]]]]]<]>>[>]++++++[-<++++++++>]>>]<<<[.[-]<<<]++++++++++.[-]
]
```
```brainfuck
[-]>[-]+[[-]>[-],[+[-----------[>[-]++++++[<------>-]<--<<[->>++++++++++<<]>>[-<<+>>]<+>]]]<],>,[-]>[-]+[[-]>[-],[+[-----------[>[-]++++++[<------>-]<--<<[->>++++++++++<<]>>[-<<+>>]<+>]]]<]<<[->>+<<]>>[[->+<]>-]+<+[<[>-]>[-<+>]<]<[-<+>]++++++>++++++<[->[->+>+<<]>[-<+>]<<]>[-]>>[[->+<]>-]>>>>[<+[<[>-]>[-<+>]<]<<[->[->+>+<<]>[-<+>]<<]>[-]>><<[-]>[-]>>+[[-]<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->[-]>>+>+<<<]]]]]]]]]<]>>[>]++++++[-<++++++++>]>>]<<<[.[-]<<<]++++++++++.[-]]>[<+[<[>-]>[-<+>]<]<<[->>+<<]>[->+<]><<[-]>[-]>>+[[-]<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->[-]>>+>+<<<]]]]]]]]]<]>>[>]++++++[-<++++++++>]>>]<<<[.[-]<<<]++++++++++.[-]]>>[<+[<[>-]>[-<+>]<]<[-<->]<<<[-]>[-]>>+[[-]<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->[-]>>+>+<<<]]]]]]]]]<]>>[>]++++++[-<++++++++>]>>]<<<[.[-]<<<]++++++++++.[-]]>>[<+[<[>-]>[-<+>]<]<<[>>>>+[<<<[<->>>>-<<<<[>>>>+>]<<<<[<<<<<]>>>>>>>+<<-]>>[<<+>>-]<+>>]]<<<<[>[>>+<<<+>-]>>[<<+>>-]<-[<]]>[>]<<<[-]>[-]>>+[[-]<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->[-]>>+>+<<<]]]]]]]]]<]>>[>]++++++[-<++++++++>]>>]<<<[.[-]<<<]++++++++++.[-]]++++++>+++++++<[->[->+>+<<]>[-<+>]<<]>[-]>>[[->+<]>-]>>[<+[<[>-]>[-<+>]<]<<[->>>+>+<<<<]>>>>[-<<<<+>>>>]<<<-[<[->>+>>+<<<<]>>>>[-<<<<+>>>>]<<[->[->+>+<<]>[-<+>]<<]>[-]>>[-<<+>>]<<<<-]>><<[-]>[-]>>+[[-]<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->+<[->[-]>>+>+<<<]]]]]]]]]<]>>[>]++++++[-<++++++++>]>>]<<<[.[-]<<<]++++++++++.[-]]
```