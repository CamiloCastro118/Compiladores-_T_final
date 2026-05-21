# Proyecto Final - Compiladores

Proyecto final del curso de Compiladores: Implementación de un lexer/analizador léxico.

## Archivos

- **entrada.txt**: Archivo de entrada de prueba
- **lexer.l**
- **parser.y**
  
## Requisitos

- Flex (Fast Lexical Analyzer Generator)
- GCC (compilador C)

## Compilación

```bash
flex lexer.l
gcc lex.yy.c -o lexer
```

## Uso

```bash
./lexer < entrada.txt
```

## Descripción

El lexer reconoce:
- Palabras clave: `int`, `if`, `else`, `while`
- Operadores: `+`, `-`, `*`, `/`, `=`
- Operadores de comparación: `<`, `>`, `<=`, `>=`, `==`, `!=`
- Identificadores y números
- Símbolos: `(`, `)`, `{`, `}`, `;`
