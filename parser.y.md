# Archivo parser.y modificado

```c
%{
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void yyerror(const char *s);
int yylex();

typedef struct Expr {
    char place[50];
} Expr;

char tac[1000][200];
int tacCount = 0;
int tempCount = 1;

char* newTemp() {

    char* temp = malloc(20);

    sprintf(temp, "t%d", tempCount++);

    return temp;
}

void emit(char *code) {

    strcpy(tac[tacCount], code);

    tacCount++;
}

Expr* newExpr(char *place) {

    Expr *e = malloc(sizeof(Expr));

    strcpy(e->place, place);

    return e;
}

void printTAC() {

    printf("\n========= TAC =========\n");

    for (int i = 0; i < tacCount; i++) {

        printf("%s\n", tac[i]);
    }
}
%}

%union {
    int num;
    char* str;
    Expr* expr;
}

%token INT IF ELSE WHILE

%token PLUS MINUS MULT DIV

%token ASSIGN

%token LT GT LE GE EQ NE

%token LPAREN RPAREN

%token LBRACE RBRACE

%token SEMICOLON

%token <num> NUM

%token <str> ID

%type <expr> expr condition

%left PLUS MINUS
%left MULT DIV

%%

program:
    statements
    {
        printTAC();
    }
;

statements:
      statements statement
    |
;

statement:
      declaration
    | assignment
    | if_stmt
    | while_stmt
;

declaration:
    INT ID SEMICOLON
;

assignment:
    ID ASSIGN expr SEMICOLON
    {
        char buffer[200];

        sprintf(buffer, "%s = %s", $1, $3->place);

        emit(buffer);
    }
;

if_stmt:
    IF LPAREN condition RPAREN
    LBRACE statements RBRACE
    else_part
;

else_part:
      ELSE LBRACE statements RBRACE
    |
;

while_stmt:
    WHILE LPAREN condition RPAREN
    LBRACE statements RBRACE
;

condition:

      expr LT expr
      {
          char *temp = newTemp();

          char buffer[200];

          sprintf(buffer, "%s = %s < %s",
                  temp,
                  $1->place,
                  $3->place);

          emit(buffer);

          $$ = newExpr(temp);
      }

    | expr GT expr
      {
          char *temp = newTemp();

          char buffer[200];

          sprintf(buffer, "%s = %s > %s",
                  temp,
                  $1->place,
                  $3->place);

          emit(buffer);

          $$ = newExpr(temp);
      }

    | expr LE expr
      {
          char *temp = newTemp();

          char buffer[200];

          sprintf(buffer, "%s = %s <= %s",
                  temp,
                  $1->place,
                  $3->place);

          emit(buffer);

          $$ = newExpr(temp);
      }

    | expr GE expr
      {
          char *temp = newTemp();

          char buffer[200];

          sprintf(buffer, "%s = %s >= %s",
                  temp,
                  $1->place,
                  $3->place);

          emit(buffer);

          $$ = newExpr(temp);
      }

    | expr EQ expr
      {
          char *temp = newTemp();

          char buffer[200];

          sprintf(buffer, "%s = %s == %s",
                  temp,
                  $1->place,
                  $3->place);

          emit(buffer);

          $$ = newExpr(temp);
      }

    | expr NE expr
      {
          char *temp = newTemp();

          char buffer[200];

          sprintf(buffer, "%s = %s != %s",
                  temp,
                  $1->place,
                  $3->place);

          emit(buffer);

          $$ = newExpr(temp);
      }
;

expr:

      expr PLUS expr
      {
          char *temp = newTemp();

          char buffer[200];

          sprintf(buffer, "%s = %s + %s",
                  temp,
                  $1->place,
                  $3->place);

          emit(buffer);

          $$ = newExpr(temp);
      }

    | expr MINUS expr
      {
          char *temp = newTemp();

          char buffer[200];

          sprintf(buffer, "%s = %s - %s",
                  temp,
                  $1->place,
                  $3->place);

          emit(buffer);

          $$ = newExpr(temp);
      }

    | expr MULT expr
      {
          char *temp = newTemp();

          char buffer[200];

          sprintf(buffer, "%s = %s * %s",
                  temp,
                  $1->place,
                  $3->place);

          emit(buffer);

          $$ = newExpr(temp);
      }

    | expr DIV expr
      {
          char *temp = newTemp();

          char buffer[200];

          sprintf(buffer, "%s = %s / %s",
                  temp,
                  $1->place,
                  $3->place);

          emit(buffer);

          $$ = newExpr(temp);
      }

    | NUM
      {
          char buffer[50];

          sprintf(buffer, "%d", $1);

          $$ = newExpr(buffer);
      }

    | ID
      {
          $$ = newExpr($1);
      }

    | LPAREN expr RPAREN
      {
          $$ = $2;
      }
;

%%

void yyerror(const char *s) {

    printf("Error sintactico: %s\n", s);
}

int main() {

    yyparse();

    return 0;
}
```

