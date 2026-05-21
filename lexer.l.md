# Archivo lexer.l modificado

```c
%{
#include "parser.tab.h"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
%}

%%

"int"      { return INT; }
"if"       { return IF; }
"else"     { return ELSE; }
"while"    { return WHILE; }

"<="       { return LE; }
">="       { return GE; }
"=="       { return EQ; }
"!="       { return NE; }

"<"        { return LT; }
">"        { return GT; }

[0-9]+ {
    yylval.num = atoi(yytext);
    return NUM;
}

[a-zA-Z_][a-zA-Z0-9_]* {
    yylval.str = strdup(yytext);
    return ID;
}

"+" { return PLUS; }
"-" { return MINUS; }
"*" { return MULT; }
"/" { return DIV; }

"=" { return ASSIGN; }

";" { return SEMICOLON; }

"(" { return LPAREN; }
")" { return RPAREN; }

"{" { return LBRACE; }
"}" { return RBRACE; }

[ \t\r\n]+ ;

. {
    printf("Caracter invalido: %s\n", yytext);
}

%%

int yywrap() {
    return 1;
}


