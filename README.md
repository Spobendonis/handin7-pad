# handin7-pad

dotnet fsi -r FsLexYacc.Runtime.dll Absyn.fs CPar.fs CLex.fs Parse.fs Interp.fs ParseAndRun.fs Machine.fs Comp.fs ParseAndComp.fs

## Exercises

### 8.1
```c
EX3

24 19 1 5 25 15 1 13 0 1 1 0 0 12 15 -1 16 43 13 0 1 1 11 22 15 -1 13 0 1 1 13 0 1 1 11 0 1 1 12 15 -1 15 0 13 0 1 1 11 13 0 0 1 11 7 18 18 15 -1 21 0

===

0  LDARGS
1  CALL 1 5
4  STOP
    Label "L1"
5  INCSP 1
7  GETBP
8  CSTI 1
10 ADD
11 CSTI 0
13 STI
14 INCSP -1
16 GOTO 43
    Label "L2"
18 GETBP
19 CSTI 1
21 ADD
22 LDI
23 PRINTI
24 INCSP -1
26 GETBP
27 CSTI 1
29 ADD
30 GETBP
31 CSTI 1
33 ADD
34 LDI
35 CSTI 1
37 ADD
38 STI
39 INCSP -1
41 INCSP 0
    Label "L3"
43 GETBP
44 CSTI 1
46 ADD
47 LDI
48 GETBP
49 CSTI 0
51 ADD
52 LDI
53 LT
54 IFNZRO 18
56 INCSP -1
58 RET 0


EX5
24 19 1 5 25 15 1 13 0 1 1 13 0 0 1 11 12 15 -1 15 1 13 0 0 1 11 13 0 2 1 19 2 57 15 -1 13 0 2 1 11 22 15 -1 15 -1 13 0 1 1 11 22 15 -1 15 -1 21 0 13 0 1 1 11 13 0 0 1 11 13 0 0 1 11 3 12 15 -1 15 0 21 1

===

0  LDARGS
1  CALL 1 5
4  STOP
    Label "L1"
5  INCSP 1
7  GETBP
8  CSTI 1
10 ADD
11 GETBP
12 CSTI 0
14 ADD
15 LDI
16 STI
17 INCSP -1
19 INCSP 1
21 GETBP
22 CSTI 0
24 ADD
25 LDI
26 GETBP
27 CSTI 2
29 ADD
30 CALL 2 57
33 INCSP -1
35 GETBP
36 CSTI 2
38 ADD
39 LDI
40 PRINTI
41 INCSP -1
43 INCSP -1
45 GETBP
46 CSTI 1
48 ADD
49 LDI
50 PRINTI
51 INCSP -1
53 INCSP -1
55 RET 0
    Label "L2"
57 GETBP
58 CSTI 1
60 ADD
61 LDI
62 GETBP
63 CSTI 0
65 ADD
66 LDI
67 GETBP
68 CSTI 0
70 ADD
71 LDI
72 MUL
73 STI
74 INCSP -1
76 INCSP 0
78 RET 1
```

```c
// micro-C example 3

void main(int n) { 
  int i; 
  i=0; 
  while (i < n) { 
    print i; 
    i=i+1;
  } 
}

===
//This is setup (load the args and start the main func) 4 = ret address, -999 = old bp
[ ]{0: LDARGS}
[ 4 ]{1: CALL 1 5}

// Here we create a new varaible on the stack with a value of i = 0
[ 4 -999 4 ]{5: INCSP 1}
[ 4 -999 4 0 ]{7: GETBP}
[ 4 -999 4 0 2 ]{8: CSTI 1}
[ 4 -999 4 0 2 1 ]{10: ADD}
[ 4 -999 4 0 3 ]{11: CSTI 0}
[ 4 -999 4 0 3 0 ]{13: STI}
[ 4 -999 4 0 0 ]{14: INCSP -1}

//This finds the value of the value of the variable i
[ 4 -999 4 0 ]{16: GOTO 43}
[ 4 -999 4 0 ]{43: GETBP}
[ 4 -999 4 0 2 ]{44: CSTI 1}
[ 4 -999 4 0 2 1 ]{46: ADD}
[ 4 -999 4 0 3 ]{47: LDI}

//This finds the value of the arg we gave (n)
[ 4 -999 4 0 0 ]{48: GETBP}
[ 4 -999 4 0 0 2 ]{49: CSTI 0}
[ 4 -999 4 0 0 2 0 ]{51: ADD}
[ 4 -999 4 0 0 2 ]{52: LDI}

//Checks if i < n
//i < n is true since i = 0, n = 4, thus we jump to 18
[ 4 -999 4 0 0 4 ]{53: LT}
[ 4 -999 4 0 1 ]{54: IFNZRO 18}

//Again, finds the value of i
[ 4 -999 4 0 ]{18: GETBP}
[ 4 -999 4 0 2 ]{19: CSTI 1}
[ 4 -999 4 0 2 1 ]{21: ADD}
[ 4 -999 4 0 3 ]{22: LDI}

//Prints the value of i
[ 4 -999 4 0 0 ]{23: PRINTI}
0 [ 4 -999 4 0 0 ]{24: INCSP -1}

//Increments i
[ 4 -999 4 0 ]{26: GETBP}
[ 4 -999 4 0 2 ]{27: CSTI 1}
[ 4 -999 4 0 2 1 ]{29: ADD}
[ 4 -999 4 0 3 ]{30: GETBP}
[ 4 -999 4 0 3 2 ]{31: CSTI 1}
[ 4 -999 4 0 3 2 1 ]{33: ADD}
[ 4 -999 4 0 3 3 ]{34: LDI}
[ 4 -999 4 0 3 0 ]{35: CSTI 1}
[ 4 -999 4 0 3 0 1 ]{37: ADD}
[ 4 -999 4 0 3 1 ]{38: STI}
[ 4 -999 4 1 1 ]{39: INCSP -1}

//Second iteration of while loop
[ 4 -999 4 1 ]{41: INCSP 0}
[ 4 -999 4 1 ]{43: GETBP}
[ 4 -999 4 1 2 ]{44: CSTI 1}
[ 4 -999 4 1 2 1 ]{46: ADD}
[ 4 -999 4 1 3 ]{47: LDI}
[ 4 -999 4 1 1 ]{48: GETBP}
[ 4 -999 4 1 1 2 ]{49: CSTI 0}
[ 4 -999 4 1 1 2 0 ]{51: ADD}
[ 4 -999 4 1 1 2 ]{52: LDI}
[ 4 -999 4 1 1 4 ]{53: LT}
[ 4 -999 4 1 1 ]{54: IFNZRO 18}
[ 4 -999 4 1 ]{18: GETBP}
[ 4 -999 4 1 2 ]{19: CSTI 1}
[ 4 -999 4 1 2 1 ]{21: ADD}
[ 4 -999 4 1 3 ]{22: LDI}
[ 4 -999 4 1 1 ]{23: PRINTI}
1 [ 4 -999 4 1 1 ]{24: INCSP -1}
[ 4 -999 4 1 ]{26: GETBP}
[ 4 -999 4 1 2 ]{27: CSTI 1}
[ 4 -999 4 1 2 1 ]{29: ADD}
[ 4 -999 4 1 3 ]{30: GETBP}
[ 4 -999 4 1 3 2 ]{31: CSTI 1}
[ 4 -999 4 1 3 2 1 ]{33: ADD}
[ 4 -999 4 1 3 3 ]{34: LDI}
[ 4 -999 4 1 3 1 ]{35: CSTI 1}
[ 4 -999 4 1 3 1 1 ]{37: ADD}
[ 4 -999 4 1 3 2 ]{38: STI}
[ 4 -999 4 2 2 ]{39: INCSP -1}
[ 4 -999 4 2 ]{41: INCSP 0}
[ 4 -999 4 2 ]{43: GETBP}
[ 4 -999 4 2 2 ]{44: CSTI 1}
[ 4 -999 4 2 2 1 ]{46: ADD}
[ 4 -999 4 2 3 ]{47: LDI}
[ 4 -999 4 2 2 ]{48: GETBP}
[ 4 -999 4 2 2 2 ]{49: CSTI 0}
[ 4 -999 4 2 2 2 0 ]{51: ADD}
[ 4 -999 4 2 2 2 ]{52: LDI}
[ 4 -999 4 2 2 4 ]{53: LT}
[ 4 -999 4 2 1 ]{54: IFNZRO 18}
[ 4 -999 4 2 ]{18: GETBP}
[ 4 -999 4 2 2 ]{19: CSTI 1}
[ 4 -999 4 2 2 1 ]{21: ADD}
[ 4 -999 4 2 3 ]{22: LDI}
[ 4 -999 4 2 2 ]{23: PRINTI}
2 [ 4 -999 4 2 2 ]{24: INCSP -1}
[ 4 -999 4 2 ]{26: GETBP}
[ 4 -999 4 2 2 ]{27: CSTI 1}
[ 4 -999 4 2 2 1 ]{29: ADD}
[ 4 -999 4 2 3 ]{30: GETBP}
[ 4 -999 4 2 3 2 ]{31: CSTI 1}
[ 4 -999 4 2 3 2 1 ]{33: ADD}
[ 4 -999 4 2 3 3 ]{34: LDI}
[ 4 -999 4 2 3 2 ]{35: CSTI 1}
[ 4 -999 4 2 3 2 1 ]{37: ADD}
[ 4 -999 4 2 3 3 ]{38: STI}
[ 4 -999 4 3 3 ]{39: INCSP -1}

//third iteration of the while loop
[ 4 -999 4 3 ]{41: INCSP 0}
[ 4 -999 4 3 ]{43: GETBP}
[ 4 -999 4 3 2 ]{44: CSTI 1}
[ 4 -999 4 3 2 1 ]{46: ADD}
[ 4 -999 4 3 3 ]{47: LDI}
[ 4 -999 4 3 3 ]{48: GETBP}
[ 4 -999 4 3 3 2 ]{49: CSTI 0}
[ 4 -999 4 3 3 2 0 ]{51: ADD}
[ 4 -999 4 3 3 2 ]{52: LDI}
[ 4 -999 4 3 3 4 ]{53: LT}
[ 4 -999 4 3 1 ]{54: IFNZRO 18}
[ 4 -999 4 3 ]{18: GETBP}
[ 4 -999 4 3 2 ]{19: CSTI 1}
[ 4 -999 4 3 2 1 ]{21: ADD}
[ 4 -999 4 3 3 ]{22: LDI}
[ 4 -999 4 3 3 ]{23: PRINTI}
3 [ 4 -999 4 3 3 ]{24: INCSP -1}
[ 4 -999 4 3 ]{26: GETBP}
[ 4 -999 4 3 2 ]{27: CSTI 1}
[ 4 -999 4 3 2 1 ]{29: ADD}
[ 4 -999 4 3 3 ]{30: GETBP}
[ 4 -999 4 3 3 2 ]{31: CSTI 1}
[ 4 -999 4 3 3 2 1 ]{33: ADD}
[ 4 -999 4 3 3 3 ]{34: LDI}
[ 4 -999 4 3 3 3 ]{35: CSTI 1}
[ 4 -999 4 3 3 3 1 ]{37: ADD}
[ 4 -999 4 3 3 4 ]{38: STI}
[ 4 -999 4 4 4 ]{39: INCSP -1}

//fourth iteration of the while loop
[ 4 -999 4 4 ]{41: INCSP 0}
[ 4 -999 4 4 ]{43: GETBP}
[ 4 -999 4 4 2 ]{44: CSTI 1}
[ 4 -999 4 4 2 1 ]{46: ADD}
[ 4 -999 4 4 3 ]{47: LDI}
[ 4 -999 4 4 4 ]{48: GETBP}
[ 4 -999 4 4 4 2 ]{49: CSTI 0}
[ 4 -999 4 4 4 2 0 ]{51: ADD}
[ 4 -999 4 4 4 2 ]{52: LDI}
[ 4 -999 4 4 4 4 ]{53: LT}

//this time, IFNZRO is now false, thus we dont jump to 18
//exit the loop
[ 4 -999 4 4 0 ]{54: IFNZRO 18}
[ 4 -999 4 4 ]{56: INCSP -1}

//return the inital funciton call, main
[ 4 -999 4 ]{58: RET 0}

//terminate the program
[ 4 ]{4: STOP}
```

### 8.3

`Absyn.fs`
```fsharp
and expr =                     
  | PreInc of access                 // <NEW>
  | PreDec of access                 // <NEW> 
  ...
```

`CLex.fsl`
```
rule Token = parse
  ...
  | "++"            { PREINC }      // <NEW>
  | "--"            { PREDEC }      // <NEW>
  ...
```

`CPar.fsy`
```
%right ASSIGN             /* lowest precedence */
...
%left PREINC PREDEC       // <NEW>
...
%nonassoc LBRACK          /* highest precedence  */

...

ExprNotAccess:
  ...
  | PREINC Access                       { PreInc($2)          } // <NEW>
  | PREDEC Access                       { PreDec($2)          } // <NEW>
  ...
```


`Comp.fs`
```fsharp
and cExpr (e : expr) (varEnv : varEnv) (funEnv : funEnv) : instr list = 
    match e with
    ...
    | PreInc acc     -> cAccess acc varEnv funEnv @ [DUP; LDI; CSTI 1; ADD; STI] // <NEW>
    | PreDec acc     -> cAccess acc varEnv funEnv @ [DUP; LDI; CSTI 1; SUB; STI] // <NEW>
    ...
```

The following file is the c program we used to test our changes.

`decrincrexp.c`
```c
void main(int n) {
    print n;
    ++n;
    yes(++n);
    print n;
}

void yes(int n) {
    print n;
}
```

Output from f-sharp interactive.
```fsharp
> open ParseAndComp;;  
> compile "decrincrexp";;
val it: Machine.instr list =
  [LDARGS; CALL (1, "L1"); STOP; Label "L1"; GETBP; CSTI 0; ADD; LDI; PRINTI;
   INCSP -1; GETBP; CSTI 0; ADD; DUP; LDI; CSTI 1; ADD; STI; INCSP -1; GETBP;
   CSTI 0; ADD; DUP; LDI; CSTI 1; ADD; STI; CALL (1, "L2"); INCSP -1; GETBP;
   CSTI 0; ADD; LDI; PRINTI; INCSP -1; INCSP 0; RET 0; Label "L2"; GETBP;
   CSTI 0; ADD; LDI; PRINTI; INCSP -1; INCSP 0; RET 0]
```
It outputs the byte-code to the following file.

`decrincrexp.out`
```
24 19 1 5 25 13 0 0 1 11 22 15 -1 13 0 0 1 9 11 0 1 1 12 15 -1 13 0 0 1 9 11 0 1 1 12 19 1 52 15 -1 13 0 0 1 11 22 15 -1 15 0 21 0 13 0 0 1 11 22 15 -1 15 0 21 0
```

Output from console:
```
$ javac Machine.java
$ java Machine decrincrexp.out 3
3 5 5
Ran 0.014 seconds
```
It gives the expected output.

### 8.4

(i)

Output from fsharp interactive:
```fsharp
> compile "ex8";;        
val it: Machine.instr list =
  [LDARGS; CALL (0, "L1"); STOP; Label "L1"; INCSP 1; GETBP; CSTI 0; ADD;
   CSTI 20000000; STI; INCSP -1; GOTO "L3"; Label "L2"; GETBP; CSTI 0; ADD;
   GETBP; CSTI 0; ADD; LDI; CSTI 1; SUB; STI; INCSP -1; INCSP 0; Label "L3";
   GETBP; CSTI 0; ADD; LDI; IFNZRO "L2"; INCSP -1; RET -1]
```

The output file:
`ex8.out`
```
24 19 0 5 25 15 1 13 0 0 1 0 20000000 12 15 -1 16 35 13 0 0 1 13 0 0 1 11 0 1 2 12 15 -1 15 0 13 0 0 1 11 18 18 15 -1 21 -1
```

`prog1`
```
0 20000000 16 7 0 1 2 9 18 4 25
```
`

```
    LDARGS
    CALL (0, "L1")
    STOP
L1:
    INCSP 1
    GETBP
    CSTI 0
    ADD
    CSTI 20000000
    STI
    INCSP -1
    GOTO "L3"
L2:
    GETBP
    CSTI 0
    ADD
    GETBP
    CSTI 0
    ADD
    LDI
    CSTI 1
    SUB
    STI
    INCSP -1
    INCSP 0
L3:
    GETBP
    CSTI 0
    ADD
    LDI
    IFNZRO "L2"
    INCSP -1
    RET -1
```

```
    CSTI 20000000
    GOTO "L2"
L1:
    CSTI 1
    SUB
L2:
    DUP
    IFNZRO "L1"
    STOP
```

`ex8.c` is a lot slower because it has tons of overhead in comparison. It takes a total of 17 instructions just to decrement by 1.

(ii)

Bytecode from compiling `ex13.c`, written out in a more readable form
```
    LDARGS
    CALL 1 "L1"
    STOP
L1: 
    INCSP 1
    GETBP
    CSTI 1
    ADD
    CSTI 1889
    STI
    INCSP -1
    GOTO "L3"
L2:
    GETBP
    CSTI 1
    ADD
    GETBP
    CSTI 1
    ADD
    LDI
    CSTI 1
    ADD
    STI
    INCSP -1
    GETBP
    CSTI 1
    ADD
    LDI
    CSTI 4
    MOD
    CSTI 0
    EQ
    IFZERO "L7"
    GETBP
    CSTI 1
    ADD
    LDI
    CSTI 100
    MOD
    CSTI 0
    EQ
    NOT
    IFNZRO "L9"
    GETBP
    CSTI 1
    ADD
    LDI
    CSTI 400
    MOD
    CSTI 0
    EQ
    GOTO "L8"
L9: 
    CSTI 1
L8: 
    GOTO "L6"
L7:
    CSTI 0
L6:
    IFZERO "L4"
    GETBP
    CSTI 1
    ADD
    LDI
    PRINTI
    INCSP -1
    GOTO "L5"
L4:
    INCSP 0
L5:
    INCSP 0
L3:
    GETBP
    CSTI 1
    ADD
    LDI
    GETBP
    CSTI 0
    ADD
    LDI
    LT
    IFNZRO "L2"
    INCSP -1
    RET 0
```

### 8.5

`Àbsyn.fs`
```fsharp
and expr =                     
  ...
  | Cond of expr * expr * expr       // <NEW>
  ...
```

`CLex.fsl`
```
rule Token = parse
  ...
  | '?'             { QUESTION }    // <NEW>
  | ':'             { COLON }       // <NEW>
  ...
```

`CPar.fsy`
```
...
%token QUESTION COLON                                         // <NEW> 
...
ExprNotAccess:
  ...
  | LPAR Expr QUESTION Expr COLON Expr RPAR { Cond($2, $4, $6) } // <NEW>
  ...
```

`Comp.fs`
```fsharp
and cExpr (e : expr) (varEnv : varEnv) (funEnv : funEnv) : instr list = 
    match e with
    ...
    | Cond(eCond, e1, e2) ->                          // <NEW>
      let labelse = newLabel()                        // <NEW>
      let labend  = newLabel()                        // <NEW>
      cExpr eCond varEnv funEnv @ [IFZERO labelse]    // <NEW>
      @ cExpr e1 varEnv funEnv @ [GOTO labend]        // <NEW>
      @ [Label labelse] @ cExpr e2 varEnv funEnv      // <NEW>
      @ [Label labend]                                // <NEW>
    ...
```

An example program to test the conditional expression:
`tern.c`
```c
void main(int n) {
    print (n > 4 ? n : 0);
}
```

Output from fsharp interactive when compiling `tern.c`
```fsharp
> open ParseAndComp;;
> compile "tern";;
val it: Machine.instr list =
  [LDARGS; CALL (1, "L1"); STOP; Label "L1"; GETBP; CSTI 0; ADD; LDI; CSTI 4;
   SWAP; LT; IFZERO "L2"; GETBP; CSTI 0; ADD; LDI; GOTO "L3"; Label "L2";
   CSTI 0; Label "L3"; PRINTI; INCSP -1; INCSP 0; RET 0]
```

`tern.out`
```
24 19 1 5 25 13 0 0 1 11 0 4 10 7 17 23 13 0 0 1 11 16 25 0 0 22 15 -1 15 0 21 0
```

Running the program on different inputs:
```
$ java Machine tern.out 4
0 
Ran 0.014 seconds
$ java Machine tern.out 3
0 
Ran 0.014 seconds
$ java Machine tern.out 6
6 
Ran 0.013 seconds
$ java Machine tern.out 5
5 
Ran 0.014 seconds
```

They all give the expected result.

### 8.6

`CLex.fsl`
```
let keyword s =
    match s with
    ...
    | "switch"  -> SWITCH   //<NEW>
    | "case"    -> CASE     //<NEW>
    ...
```

`CPar.fsy`
```
StmtM:  /* No unbalanced if-else */
  ...
  | SWITCH LPAR Expr RPAR LBRACE Cases RBRACE {Switch($3, $6)}        //<NEW>
  | Block                               { $1                   }
  ...

...
Cases:                                                                //<NEW>
  CASE CSTINT COLON Block              { [($2, $4)] }                 //<NEW>
  | CASE CSTINT COLON Block Cases      { ($2, $4) :: $5 }             //<NEW>
...
```

`Absyn.fs`
```fsharp
and stmt =                                                         
  ...
  | Switch of expr * ((int * stmt) list)     // <NEW>
```

`Comp.fs`
```fsharp
let rec cStmt stmt (varEnv : varEnv) (funEnv : funEnv) : instr list = 
    match stmt with
    ...
    | Switch (e, cases) ->                                             // <NEW>
        let labend  = newLabel()                                       // <NEW>
        cExpr e varEnv funEnv @ List.concat (List.map (fun c ->        // <NEW>
          let label = newLabel()                                       // <NEW>
          [DUP; CSTI (fst c); EQ; IFZERO label; INCSP -1] @            // <NEW>
          (cStmt(snd c) varEnv funEnv) @                               // <NEW>
          [GOTO labend; Label label]) cases) @                         // <NEW>
          [INCSP -1; Label labend]                                     // <NEW>
    ...
```

This is an example program to test the switch statement:
`switch.c`
```c
void main(int n) {
    switch (n) {
        case 1:
            { 
                int i;
                i = 16;
                print 30;
                print 200; 
            }
        case 2:
            { print n; }
    }
    print 4;
}
```

Compiling the switch program in fsharp interactive:
```fsharp
> open ParseAndComp;;
> compile "switch";;
val it: Machine.instr list =
  [LDARGS; CALL (1, "L1"); STOP; Label "L1"; GETBP; CSTI 0; ADD; LDI; DUP;
   CSTI 1; EQ; IFZERO "L3"; INCSP -1; INCSP 1; GETBP; CSTI 1; ADD; CSTI 16;
   STI; INCSP -1; CSTI 30; PRINTI; INCSP -1; CSTI 200; PRINTI; INCSP -1;
   INCSP -1; GOTO "L2"; Label "L3"; DUP; CSTI 2; EQ; IFZERO "L4"; INCSP -1;
   GETBP; CSTI 0; ADD; LDI; PRINTI; INCSP -1; INCSP 0; GOTO "L2"; Label "L4";
   INCSP -1; Label "L2"; CSTI 4; PRINTI; INCSP -1; INCSP 0; RET 0]

```

From running the compiled switch program:
```
$ java Machine switch.out 3
4 
Ran 0.013 seconds
$ java Machine switch.out 2
2 4 
Ran 0.013 seconds
$ java Machine switch.out 1
30 200 4 
Ran 0.013 seconds
```
They all return the expected result.