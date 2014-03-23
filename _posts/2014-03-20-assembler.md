---
layout: post
title:  "Proyecto #3"
subtitle:  "Ensamblador de MIPS"
date:   2014-03-20 21:28:27
categories: projects
published: true
due_date: 2014-04-27
excerpt: Ensamblador de MIPS, escrito en MIPS.
tags: mips assembler
---

## Objetivos

Este proyecto esta diseñado para que el estudiante comprenda a detalle la función de un ensamblador, como es que este traduce código legible por humanos a lenguaje maquina. Con este proyecto nos acercamos un poco maás a los componentes lógicos con los cuales están construidas las PC's.

Este proyecto nos enseña los detalles de la codificación, y las ventajas que tiene tener una arquitectura estándar, sin muchos casos especiales, pocos formatos de instrucciones, utilización de cualquier registro para cualquier operación aritmético-lógica, etc.

--

## Procedimiento

Usted debe implementar una función en MIPS llamada `ensamblador` la cual reciba como primer argumento una **dirección de memoria**, en la cual se encuentra el código (en forma de cadena de caracteres) que se desea ensamblar, como segundo argumento la **dirección de memoria** donde se encuentra su área de `.text`, y como tercer argumento **una dirección** donde se encuentre su área de `.data`.

La función debe convertir las instrucciones de MIPS a lenguaje maquina, (únicamente las que se especifican) además de calcular los offsets de los branches y los jumps. Únicamente debe soportar las directivas `.text`, `.data` y `.asciiz`. Además todo el input numérico es en formato decimal, construcciones como ‘0xbaba’ son ilegales, pero no es necesario que indique el error.

La función debe devolver la dirección donde almacenó el código en lenguaje maquina, al regresar a la función que la mando a llamar se deberá ser capaz de saltar a la dirección devuelta por `ensamblador` y el simulador debe ser capaz de ejecutar el código.

No es necesario que soporten los comentarios, sin embargo su implementación es trivial y les evitaría tener que estarlos borrando.

La mejor forma de probar su proyecto es haciendo programas en MIPS, codificarlos utilizando **su** ensamblador, luego ejecutar el código que su ensamblador generó y verificar que el output fue el mismo. De cualquier manera adjunto en el zip va una prueba de algunas de las instrucciones.

Las mejores fuentes de información que tendrán acá será el `apéndice A` del CoD y el propio simulador. El `apéndice A` tiene todas las instrucciones que soporta MIPS y cómo es su codificación, y con el simulador podemos ver la codificación de cualquier instrucción que incluyamos en algún programa.

No es necesario que escriba un código muy robusto, los programas con los que los probaremos no contendrán errores de ningún tipo, es especialmente importante la forma de codificación, para estandarizar, no utilizaremos comas para separar registros, ni más de un espacio, los registros nunca estarán en forma numérica a excepción de $0, los labels siempre están en una línea por si mismos, en caso de duda pregúnten por medio de Piazza o en persona, o revise los ejemplos.

Este es un poyecto que conlleva mucho trabajo, por lo que les recomendamos trabajar en equipos de 2 personas. Y comiencen a trabajar con tiempo, no podrán terminar el proyecto si lo dejan para la última semana.


## Implementación

Sólo le pediremos que soporte un subset del set de instrucciones de MIPS, el cual se detalla a continuación:

### Comparación

#### TAL

1. slt
2. slti
3. sltu
4. sltiu
5. seq
6. sne

### Aritmético-lógicas

#### TAL

7. add
8. sub
9. addi
10. addu
11. subu
12. addiu
13. mult
14. multu
15. div
16. divu
17. mfhi
18. mflo
19. and
20. or
21. andi
22. sll
23. srl
24. srlv
25. sllv

#### MAL
26. move
27. neg
28. mul
29. div
30. abs
31. ror
32. rol

### Memoria

#### TAL

33. lw
34. sw
35. lui

#### MAL

36. li
37. la

### Branches

#### TAL

38. j
39. jr
40. jal
41. beq
42. bne
43. bgtz

#### MAL

44. bgt
45. blt

### Directivas

46. .text
47. .data
48. .asciiz

--

### Algo de ayuda

A continuación encontrará algo de código de MIPS, el cual está diseñado como referencia, incluye un main funcional (pueden modificarlo para hacer sus pruebas), algunas funciones (string_compare, ascii_to_integer, etc.) y un pequeño trozo del programa que ustedes deben implementar (únicamente reconoce ori y syscall).

Les recomendamos que lo **estudien**, **pueden modificarlo a su discreción** y utilizar/basar sus funciones en las que allí aparecen. Nótese que este es solamente **un ejemplo**, no es necesario que lo implemente de esta manera.  

	.data
	      msgBCodif: .asciiz "Codificando el siguinete programa:\n\n"
	      errMsg:    .asciiz "Error!!!\n"
	      msgFCodif: .asciiz "\n\nFinaliza la codificacion...\n\nEjecutando el programa codificado...\n"
	      
	      ########## Aca va el programa que voy a codificar!!! ##############
	      programa:	.asciiz "   .text
	      main:
		ori $a0 $0 10
		ori $v0 $0 1
		syscall
		ori $v0 $0 10
		syscall"
	      ########## Aca Termina el Programa ############
	      
	.align 2
	      data:	.space 200
	      text:	.space 400	# Reservo espacio para guardar programa compilado (100 instrucciones)
	.text

	      main:
		la $a0 msgBCodif	# imprimo el programa que voy a ensamblar
		jal print_str
		la $a0 programa
		jal print_str
		la $a0 msgFCodif
		jal print_str

		la $a0 programa		# llamo a la funcion con sus respectivos argumentos
		la $a1 text
		la $a2 data
		jal ensamblador

		jr $v0			# una vez compilado lo ejecuto
		j exit			# solo por si acaso ;)


	      print_str:
		li $v0 4
		syscall
		jr $ra

	      exit:
		li $v0 10
		syscall

	      ###################################################
	      ########### FIN DEL MAIN ##########################
	      ###################################################
	      
	      
	      ###################################################
	      ######### Comienza la Funcion Ensamblador #########
	      ###################################################

	  .data		# palabras reservadas (instrucciones)
	      str_data:		.asciiz ".data"
	      str_text:		.asciiz ".text"
	      str_ori:		.asciiz "ori"
	      str_syscall:	.asciiz "syscall"

	  .text
	      ###################################################
	      ############ funcion ensamblador ##################
	      ###################################################
	      
	      ensamblador:
	      # Entrada, almaceno variables a utilizar
		addi $sp $sp -40
		sw $a1 36($sp)
		sw $ra 32($sp)
		sw $s7 28($sp)
		sw $s6 24($sp)
		sw $s5 20($sp)
		sw $s4 16($sp)
		sw $s3 12($sp)
		sw $s2  8($sp)
		sw $s1  4($sp)
		sw $s0  0($sp)
		
	      # Transfiero los argumentos a un lugar seguro
		move $s0 $a0	# s0 -> codigo fuente
		move $s1 $a1	# s1 -> area de .text
		move $s2 $a2	# s2 -> area de .data
		move $s3 $a3	# s3 -> area de stack
		li   $s4 ' '	# s4 -> caracter espacio
		li   $s5 10     # s5 -> caracter "\n" -> EOL
		li   $s6 9      # s6 -> tabulador

	      # Cuerpo
	      # asumo que empieza en modo .text
	      asm_text_loop:
		lb $t0 0($s0)
		addi $s0 $s0 1
		beq $t0 $0 asm_exit		# si ya llegue al fin de archivo me salgo
		beq $t0 $s4 asm_text_loop	# ignoro espacios en blanco
		beq $t0 $s5 asm_text_loop	# lineas en blanco
		beq $t0 $s6 asm_text_loop	# y tabulador
		j asm_get_instruction

	      asm_data_loop:			# nada aun
		j asm_exit

	      asm_error:			# manejo generico de errores
		la $a0 errMsg			# en caso de cualquier problema, imprimo error
		jal print_str
		j asm_exit

	      # Salida
	      asm_exit:
		lw $a1 36($sp)
		lw $ra 32($sp)
		lw $s7 28($sp)
		lw $s6 24($sp)
		lw $s5 20($sp)
		lw $s4 16($sp)
		lw $s3 12($sp)
		lw $s2  8($sp)
		lw $s1  4($sp)
		lw $s0  0($sp)

		move $v0 $a1			# hago trampa para devolver el buffer ;)
		addi $sp $sp 40
		jr $ra

	      ###########################################
	      ###### Funciones ##########################
	      ###########################################

	      asm_get_instruction:		# este es un gran switch, me indica que instruccion es
		addi $s0 $s0 -1			# chapuz pq ya le habia sumado 1

		move $a0 $s0			# verifico si es la directiva .data
		la $a1 str_data
		jal strcmp
		bne $v0 $0 asm_data_loop
		
		move $a0 $s0			# verifico si es la directiva .text
		la $a1 str_text
		jal strcmp
		bne $v0 $0 asm_text_loop

		move $a0 $s0			# verifico si es la instruccion ori
		la $a1 str_ori
		jal strcmp
		bne $v0 $0 asm_ori

		move $a0 $s0			# verifico si es la instruccion syscall
		la $a1 str_syscall
		jal strcmp
		bne $v0 $0 asm_syscall

		move $a0 $s0			# verifico si es un label
		jal asm_label_check
		bne $v0 $0 asm_label

		j asm_error			# si no es ninguna me voy a error

	      ###########################################
	      ######### asm_label #######################
	      ###########################################
	      asm_label:			# codigo a ejecutarse en caso que sea un label
		lb $t0 0($s0)			# se utiliza para llenar la tabla de simbolos
		addi $s0 $s0 1
		bne $t0 $s5 asm_label
		j asm_text_loop

	      ###########################################
	      ######### asm_syscall #####################
	      ###########################################
	      asm_syscall:			# esta instruccion es muy facil de codificar ;)
		la $t0 0x0000000c
		sw $t0 0($s1)
		addi $s1 $s1 4
		j asm_text_loop

	      ###########################################
	      ######### asm_ori #########################
	      ###########################################
	      asm_ori:
		li $s7 0x0d		# codigo de ori 0x0d

		sll $s7 $s7 10		# shift porque son 5b de rt + 5b de rs
		addi $s0 $s0 1		# elimino el espacio
		jal asm_regs            # me devuelve el numero del registro
		add $s7 $s7 $v0		# almaceno el numero del registro en rs

		addi $s0 $s0 1		# elimino el espacio
		jal asm_regs         	# me devuelve el numero del registro
		sll $v0 $v0 5		# pongo rs en la posicion que debe ir (van cruzados)
		or  $s7 $s7 $v0		# almaceno el numero del registro en rt
	      
		sll $s7 $s7 16		# le hago shift de 16 para hacer espacio al imm
		addi $s0 $s0 1		# elimino el espacio
		jal ascii_to_int	# hago la conversion de ascii a int
		addu $s7 $s7 $v0 	# concateno el imm con el resto que ya tenia
		sw $s7 0($s1)		# almaceno la instruccion codificada
		addi $s1 $s1 4
		j asm_text_loop

	      ###########################################
	      ############# asm_regs ####################
	      ###########################################
	      asm_regs:			# pasa de $AN -> N ej. $s0 -> 16
		addi $sp $sp -4		# por supuesto que esta incompleto...
		sw $ra 0($sp)
		li $t7 '$'		# voy a utilizarlo para verificar que viene un registro
		li $t6 'a'		# aX -> argumentos
		li $t5 'v'		# vX -> valores de retorno
		li $t4 '0'		# cero
		
		lb $t0 0($s0)
		addi $s0 $s0 1
		bne $t0 $t7 asm_regs_error	# si no empieza con $ no es valido
		lb $t0 0($s0)
		addi $s0 $s0 1
		beq $t0 $t6 asm_regs_ax		# verifico a que grupo pertence para sumarle un offset
		beq $t0 $t5 asm_regs_vx
		beq $t0 $t4 asm_regs_zero
		j asm_regs_error		# no es ninguno... error

	      asm_regs_zero:		# caso trivial 
		li $v0 0
		j asm_regs_exit

	      asm_regs_ax:		# le pongo de base $a0 y sumo su offset
		jal ascii_to_int	# ($a0 = 4) => ($a3 - $a0) + 4 = 7
		addi $v0 $v0 4
		j asm_regs_exit

	      asm_regs_vx:
		jal ascii_to_int
		addi $v0 $v0 2
		j asm_regs_exit

	      asm_regs_exit:
		lw $ra 0($sp)
		addi $sp $sp 4
		jr $ra

	      asm_regs_error:
		addi $sp $sp 4
		j asm_error
	      
		
	      ###########################################
	      ######### ascii_to_int ####################
	      ###########################################
	      ascii_to_int:     # the infamous atoi, with no validations!
	      li $t1 0		# init with zero
	      li $t2 '0'	
	      li $t3 '9'	
	      li $t4 10
	      li $v0 0

	      ascii_to_int_loop:
		lb $t0 0($s0)
		beq $t0 $0  ascii_to_int_exit
		blt $t0 $t2 ascii_to_int_exit	# only accept numbers
		bgt $t0 $t3 ascii_to_int_exit	# only accept numbers
		addi $s0 $s0 1			# advance the char pointer
		addi $t0 $t0 -48		# get real number (without the '0' offset)
		mul  $v0 $v0 $t4		# multiply by 10
		add  $v0 $v0 $t0		# add real number
		j ascii_to_int_loop
		
	      ascii_to_int_exit:
		jr $ra

	      ###########################################
	      ######### strcmp ##########################
	      ###########################################

	      strcmp:				# compara 2 cadenas de caracteres
		li $t2 ' '	
		li $t3 10

	      strcmp_loop:
		lb $t0 0($a0)
		lb $t1 0($a1)
		beq $t0 $t2 strcmp_true		# no es una letra
		beq $t0 $t3 strcmp_true		# no es una letra (acepto algunas que no son letras)
		beq $t0 $0  strcmp_true		# fin de cadena
		bne $t0 $t1 strcmp_false	# es diferente
		addi $a0 $a0 1
		addi $a1 $a1 1
		j strcmp_loop
		
	      strcmp_true:
		move $s0 $a0			# chapuz FEO que uso para que siga por donde me quede
		li $v0 1			# el chapuz anterior mata la portabilidad de la funcion
		jr $ra

	      strcmp_false:
		li $v0 0
		jr $ra

	      ###########################################
	      ######### asm_label_check #################
	      ###########################################
	      asm_label_check:			# verifica si la cadena es un label
		li $t1 ':'
		li $t2 ' '
		li $t3 10

	      asm_label_loop:
		lb $t0 0($a0)
		addi $a0 $a0 1
		beq $t0 $t1 asm_label_true
		beq $t0 $t2 asm_label_false
		beq $t0 $t3 asm_label_false
		j asm_label_loop

	      asm_label_true:
		move $s0 $a0
		li $v0 1
		jr $ra

	      asm_label_false:
		li $v0 0
		jr $ra


## Evaluación

La evaluación será hecha de forma presencial, por lo que es muy importante que asista, si no lo hace, su nota automáticamente será de 0.

Puede descargar la siguiente [hoja de cálculo](https://www.dropbox.com/s/trd1m22buqodv6o/score.xls), en la cual encontrará la ponderación del proyecto, puede evaluarse a usted mismo según vaya progresando en su proyecto.

También hemos preparado una serie de [pruebas](https://www.dropbox.com/s/e92qr0cc0pmfeim/tests.zip) que puede utilizar para probar el funcionamiento de su proyecto, estas pruebas no garantizan que saque 100 en su proyecto, 
sin embargo puede utilizarlas como referencia.

Adicionalmente, debe realizar un **video que muestre cómo funciona su poyecto**, enseñe varias llamadas al mismo utilizando diferentes valores de entrada, explicando
cuál es el valor de retorno esperado.

--

## Entrega

La entrega se realizará a través del `GES` antes de las **23:55:00** del **Domingo 27 de abril de 2014** debe enviar un archivo llamado `grupo.s` conteniendo todo su código de MIPS.

--