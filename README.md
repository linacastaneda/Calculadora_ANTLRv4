# Implementación de Gramáticas con ANTLR v4

## Descripción

Este repositorio contiene la implementación de dos gramáticas desarrolladas con ANTLR v4:

- **Hello.g4**: Gramática básica para comprender el funcionamiento del análisis léxico y sintáctico.
- **LabeledExpr.g4**: Implementación de una calculadora aritmética con evaluación semántica utilizando el patrón Visitor.

El proyecto demuestra el flujo completo de construcción de un lenguaje formal:

Archivo → Lexer → TokenStream → Parser → ParseTree → Visitor → Resultado

---

## 1. Hello.g4

### Descripción

Gramática mínima que reconoce expresiones de la forma:


hello Nombre


### Gramática

```antlr
grammar Hello;

r  : 'hello' ID ;

ID : [a-zA-Z]+ ;

WS : [ \t\r\n]+ -> skip ;
Compilación y prueba
antlr4 Hello.g4
javac Hello*.java
grun Hello r -tree
2. Calculadora con ANTLR v4
Características

Operaciones aritméticas (+, -, *, /)

Variables

Paréntesis

Manejo de precedencia

Implementación semántica con Visitor

Gramática principal
prog: stat+ ;

stat:
     expr NEWLINE        # printExpr
   | ID '=' expr NEWLINE # assign
   | NEWLINE             # blank
;

expr:
     expr op=('*'|'/') expr # MulDiv
   | expr op=('+'|'-') expr # AddSub
   | INT                    # int
   | ID                     # id
   | '(' expr ')'           # parens
;
Generación del parser
antlr4 -no-listener -visitor LabeledExpr.g4
Compilación
javac Calc.java EvalVisitor.java LabeledExpr*.java
Ejecución
java Calc t.expr
Ejemplo de entrada (t.expr)
193
a = 5
b = 6
a+b*2
(1+2)*3
Salida
193
17
9
Conceptos aplicados

Diseño de gramáticas formales

Separación entre sintaxis y semántica

Patrón Visitor

Manejo automático de precedencia en ANTLR v4

Estructura del proyecto
Archivos de gramática

Hello.g4

LabeledExpr.g4

Archivos generados automáticamente por ANTLR

HelloLexer.java

HelloParser.java

Hello.tokens

Hello.interp

LabeledExprLexer.java

LabeledExprParser.java

LabeledExprVisitor.java

LabeledExprBaseVisitor.java

LabeledExpr.tokens

LabeledExpr.interp

Archivos escritos manualmente

Calc.java

EvalVisitor.java

t.expr

Archivos compilados

Archivos .class
