# Ex.No:3
   RECOGNITION-OF-A-VALID-ARITHMETIC-EXPRESSION-THAT-USES-OPERATOR-AND-USING-YACC
## Register Number: 212224040023
## Date: 22-09-2025
## AIM
To write a yacc program to recognize a valid arithmetic expression that uses operator +,- ,* and /.
## ALGORITHM
1.	Start the program.
2.	Write a program in the vi editor and save it with .l extension.
3.	In the lex program, write the translation rules for the operators =,+,-,*,/ and for the identifier.
4.	Write a program in the vi editor and save it with .y extension.
5.	Compile the lex program with lex compiler to produce output file as lex.yy.c. eg $ lex filename.l
6.	Compile the yacc program with yacc compiler to produce output file as y.tab.c. eg $ yacc –d arith_id.y
7.	Compile these with the C compiler as gcc lex.yy.c y.tab.c
8.	Enter an arithmetic expression as input and the tokens are identified as output.
## PROGRAM
exp3.l
```
%{
#include "y.tab.h"
%}

%%

"="                  { printf("\nOperator is EQUAL"); return '='; }
"+"                  { printf("\nOperator is PLUS"); return PLUS; }
"-"                  { printf("\nOperator is MINUS"); return MINUS; }
"/"                  { printf("\nOperator is DIVISION"); return DIVISION; }
"*"                  { printf("\nOperator is MULTIPLICATION"); return MULTIPLICATION; }
[a-zA-Z][a-zA-Z0-9]* { printf("\nIdentifier is %s", yytext); return ID; }
[ \t]+               ;    /* Ignore whitespace */
.                    { return yytext[0]; }
\n                   { return 0; }

%%

int yywrap() {
    return 1;
}
```

exp3.yacc
```
%{
#include <stdio.h>

/* 
   FIX: Declare yyerror before Bison uses it
   This tells the compiler what yyerror looks like.
*/
void yyerror(const char *s);
%}

%token ID PLUS MINUS MULTIPLICATION DIVISION

/* Define operator precedence */
%left PLUS MINUS
%left MULTIPLICATION DIVISION

%%

statement:
    ID '=' E {
        printf("\nValid arithmetic expression\n");
        $$ = $3;  /* Final result (optional) */
    }
;

E:
    E PLUS ID            { $$ = $1 + $3; }
  | E MINUS ID           { $$ = $1 - $3; }
  | E MULTIPLICATION ID  { $$ = $1 * $3; }
  | E DIVISION ID        { $$ = $1 / $3; }
  | ID                   { $$ = 1; } /* Dummy value for identifier */
;

%%

extern FILE *yyin;

int main() {
    yyparse();
    return 0;
}

/* FIX: Define yyerror AFTER main */
void yyerror(const char *s) {
    fprintf(stderr, "Error: %s\n", s);
}
```

imput.txt
```
x = a + b * c
```

## OUTPUT
<img width="897" height="390" alt="image" src="https://github.com/user-attachments/assets/31f67e81-df8e-4c87-b53c-79d57a0b0eea" />

## RESULT
A YACC program to recognize a valid arithmetic expression that uses operator +,-,* and / is executed successfully and the output is verified.
