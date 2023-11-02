# handin7-pad

dotnet fsi -r FsLexYacc.Runtime.dll Absyn.fs CPar.fs CLex.fs Parse.fs Interp.fs ParseAndRun.fs Machine.fs Comp.fs ParseAndComp.fs

## Exercises

### 8.1
```c
24 19 1 5 25 15 1 13 0 1 1 0 0 12 15 -1 16 43 13 0 1 1 11 22 15 -1 13 0 1 1 13 0 1 1 11 0 1 1 12 15 -1 15 0 13 0 1 1 11 13 0 0 1 11 7 18 18 15 -1 21 0

===

0 LDARGS
1 CALL 1 5
4 STOP
    Label "L1"
5 INCSP 1
7 GETBP
8 CSTI 1
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