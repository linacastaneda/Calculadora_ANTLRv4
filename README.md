# Implementación de Gramáticas con ANTLR v4

## 1. Descripción

Este repositorio contiene la implementación de dos gramáticas desarrolladas con ANTLR v4:

- **Hello.g4**: Gramática básica para comprender el funcionamiento del análisis léxico y sintáctico.
- **LabeledExpr.g4**: Implementación de una calculadora aritmética con evaluación semántica utilizando el patrón Visitor.

El proyecto demuestra el flujo completo de construcción de un lenguaje formal:

```
Archivo → Lexer → TokenStream → Parser → ParseTree → Visitor → Resultado
```

---

## 2. Hello.g4

### 2.1 Descripción

Gramática mínima que reconoce expresiones de la forma:

```
hello Nombre
```

### 2.2 Gramática

```antlr
grammar Hello;

r  : 'hello' ID ;
ID : [a-zA-Z]+ ;
WS : [ \t\r\n]+ -> skip ;
```

### 2.3 Compilación y prueba

**Generación del parser:**

```bash
antlr4 Hello.g4
```

**Compilación:**

```bash
javac Hello*.java
```

**Ejecución con árbol sintáctico:**

```bash
grun Hello r -tree
```

---

## 3. Calculadora con ANTLR v4

### 3.1 Características

- Operaciones aritméticas (`+`, `-`, `*`, `/`)
- Variables
- Paréntesis
- Manejo de precedencia
- Implementación semántica con Visitor

### 3.2 Gramática principal

```antlr
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
```

### 3.3 Generación del parser

```bash
antlr4 -no-listener -visitor LabeledExpr.g4
```

### 3.4 Compilación

```bash
javac Calc.java EvalVisitor.java LabeledExpr*.java
```

### 3.5 Ejecución

```bash
java Calc t.expr
```

### 3.6 Ejemplo de entrada (`t.expr`)

```
193
a = 5
b = 6
a+b*2
(1+2)*3
```

### 3.7 Salida

```
193
17
9
```
