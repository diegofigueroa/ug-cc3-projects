---
layout: post
title:  "Proyecto #4"
subtitle:  "Unidad Básica"
date:   2014-03-23 16:54:27
categories: projects
published: true
due_date: 2014-05-25
excerpt: Simulación de un procesador básico.
tags: processor
---

## Objetivos

Este proyecto esta diseñado para que usted viva la experiencia de diseñar su propio procesador, para que aprenda a diseñar su propio ISA, entender por qué los diferentes formatos de codificación de instrucciones, la importancia de un diseño sencillo entre otras cosas.  

Este proyecto cubre "casi" todo lo visto en clase, su complejidad es bastante alta sin embargo es extremadamente divertido ;-) y educativo.  

Para completarlo debe construir todas las piezas de las que se compone un procesador, basándose en el ISA que se define más adelante.

--

## Procedimiento

Usted debe implementar en Logisim un procesador de 16 bits el cual tenga las siguientes instrucciones (el número indica el `op-code` de la instrucción):

0. Add
1. Sub
2. And
3. Or
4. Xor
5. Sl
6. Slt
7. Cmp
8. Sb
9. Lb
10. Li
11. Lui
12. Jmp
13. Bra
14. Jr
15. SPC

**Todas** las instrucciones son de 16 bits y los primeros 4 bits siempre indican que instrucción se desea realizar, las primeras 8 instrucciones son aritmético-lógicas, las siguientes 4 son de transferencia de memoria y las ultimas 4 son branches.


## Implementación

Hay 2 formatos de instrucciones:

### Tipo “R”:

* 4 bits op-code (Código de la instrucción) bits 12-15
* 4 bits Rdest (Registro destino) bits 8-11
* 4 bits Rsrc1 (Registro que identifica al operador 1) bits 4-7
* 4 bits Rsrc2 (Registro que identifica al operador 2) bits 0-3


### Tipo “I”:

* 4 bits op-code (Código de la instrucción) bits 12-15
* 4 bits Rdest (Registro que interviene en la operación) bits 8-11
* 8 bits Imm (Para un valor inmediato) bits 0-7

Para información detallada de cada una de las instrucciones puede referirse a la [documentación](https://www.dropbox.com/s/hcdulxqa19jxvcl/isa.pdf):

### Acerca de los registros:

Utilizaremos 16 registros de 16 bits cada uno, todos los registros son de propósito general (se pueden leer y escribir en cualquier operación) a excepción del registro R0 el cual esta alambrado a tierra (siempre tiene el valor de cero y no se puede sobrescribir).

### El tipo de máquina:

Utilizaremos un 3 address machine, esto significa que las operaciones aritméticas tienen la siguiente forma: `A = B + C`

Visto en el código: `Add R3 R2 R1`

### Acerca del diseño:

Puede diseñar su procesador como crea más conveniente, con la restricción que debe utilizar el mismo ISA, esto con el afán que se puedan ejecutar todos los programas en todas las implementaciones, no importando cómo esta construido internamente el hardware.

--

## Evaluación

La evaluación será hecha de forma presencial, por lo que es muy importante que asista, si no lo hace, su nota automáticamente será de 0.

Este proyecto se trabajará de manera **individual**.

Este proyecto se calificara de la siguiente manera:

- 50% por tener las operaciones de ALU(20%) y los registros(20%) funcionando en conjunto con li y lui (10%)
- 60% por lo anterior + funcionamiento del Program Counter
- 80% por lo anterior y saltos (Jmp y Beq con jr y spc).
- 100% por lo anterior y accesos a memoria (Lb, Sb) tanto lectura como escritura.
- 120% por agregar pantalla y teclado ascii.

Puede utilizar los componentes del siguiente [paquete](https://www.dropbox.com/s/duze632lyh5miy5/c316.jar), el cual incluye un **teclado**, **pantalla**, joystick entre otras cosas, más información [aquí](https://www.dropbox.com/s/4v8ikp12qst3sv4/toys.pdf).

Adicionalmente, debe realizar un **video que muestre cómo funciona su proyecto**, enseñe varias ejecuciones de su procesador, explicando el programa cargado y cómo debe
ser su ejecución.

--

## Entrega

La entrega se realizará a través del `GES` antes de las **23:55:00** del **Domingo 25 de Mayo de 2014** debe enviar un archivo llamado `grupo.s` conteniendo todo su código de MIPS.

--