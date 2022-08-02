# Seccion 8. Asignaciones y calculos





## Variables de sistema

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-28-10-37-30-image.png)



El siguiente programa busca en la tabla de clientes por el pais que se capture en la pantalla de parametros.

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

TABLES scustom.

PARAMETERS p_ctry LIKE scustom-country OBLIGATORY DEFAULT 'US'.

WRITE: / 'Fecha actual:',  sy-datum,
  / 'Usuario actual', sy-uzeit,
  / 'Cliente para el pais', p_ctry,
  /.

SELECT * FROM scustom
  WHERE country = p_ctry
  ORDER BY id.

  WRITE: / sy-dbcnt, scustom-id.

ENDSELECT.

WRITE: / sy-dbcnt, 'registro encontrados'.

IF sy-subrc <> 0.
  WRITE: / 'No existen clientes para el pais', p_ctry.
ENDIF.
```



Programa que dado un codigo de pais muestra la descripcion de ese pais.



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

TABLES t005t. " tabla del sistema donde se almacena descripciones de codigo de pais en multiples idiomas

PARAMETERS p_land LIKE t005t-land1 OBLIGATORY DEFAULT 'US'.

SELECT SINGLE * FROM t005t
  WHERE spras = sy-langu
  AND land1 = p_land.

IF sy-subrc EQ 0.
  WRITE: 'Description: ', t005t-landx.
ELSE.
  WRITE: 'No existe descripcion para ', p_land.
ENDIF.


```





## Sentencias de asignacion



Asigna una valor a una variable o campo de registro.



* **clear:** establece el valor en ceros, o en blanco (valores iniciales por defecto)



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.


TABLES scustom.
DATA: f1(2) TYPE c VALUE 'AB',
      f2 TYPE i VALUE 12345,
      f3 TYPE p VALUE 12345,
      f4 TYPE f VALUE '1E1',
      f5(3) TYPE n VALUE '789',
      f6 TYPE d VALUE '19980101',
      f7 TYPE t VALUE '120100',
      f8 TYPE x VALUE 'AA',
BEGIN OF s1,
  f1(3) TYPE c VALUE 'XYZ',
  f2 TYPE i VALUE 123456,
END OF s1.

scustom-id = 'XXX'.
scustom-country = 'VE'.

WRITE: / 'f1=''' NO-GAP, f1 NO-GAP, '''',
/ 'f2=''' NO-GAP, f2 NO-GAP, '''',
/ 'f3=''' NO-GAP, f3 NO-GAP, '''',
/ 'f4=''' NO-GAP, f4 NO-GAP, '''',
/ 'f5=''' NO-GAP, f5 NO-GAP, '''',
/ 'f6=''' NO-GAP, f6 NO-GAP, '''',
/ 'f7=''' NO-GAP, f7 NO-GAP, '''',
/ 'f8=''' NO-GAP, f8 NO-GAP, '''',
/ 's1-f1=''' NO-GAP, s1-f1 NO-GAP, '''',
/ 's1-f2=''' NO-GAP, s1-f2 NO-GAP, '''',
/ 'scustom-id=''' NO-GAP, scustom-id NO-GAP, '''',
/ 'scustom-country=''' NO-GAP, scustom-country NO-GAP, ''''.

CLEAR: f1, f2, f3, f4, f5, f6, f7, f8, s1, scustom.  " limpiar los valores que se asignaron previamente.

WRITE: / 'f1=''' NO-GAP, f1 NO-GAP, '''',
/ 'f2=''' NO-GAP, f2 NO-GAP, '''',
/ 'f3=''' NO-GAP, f3 NO-GAP, '''',
/ 'f4=''' NO-GAP, f4 NO-GAP, '''',
/ 'f5=''' NO-GAP, f5 NO-GAP, '''',
/ 'f6=''' NO-GAP, f6 NO-GAP, '''',
/ 'f7=''' NO-GAP, f7 NO-GAP, '''',
/ 'f8=''' NO-GAP, f8 NO-GAP, '''',
/ 's1-f1=''' NO-GAP, s1-f1 NO-GAP, '''',
/ 's1-f2=''' NO-GAP, s1-f2 NO-GAP, '''',
/ 'scustom-id=''' NO-GAP, scustom-id NO-GAP, '''',
/ 'scustom-country=''' NO-GAP, scustom-country NO-GAP, ''''.



```



* **move**: copia un valor de un campo a otro, tambien se puede utilizar el operado =.

* **move-corresponding**.



### Conversiones de datos



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

CONSTANTS >(3) VALUE '==>'. "Definción de una constante llamda '>'
DATA: fc(10) TYPE c VALUE '-A1B2C3.4',
      fn(10) TYPE n,
      fp TYPE p,
      fd TYPE d,
      ft TYPE t,
      fx(4) TYPE x,
      fc1(5) TYPE c VALUE '-1234',
      fc2(5) TYPE c VALUE '1234-',
      fp1 TYPE p VALUE 123456789,
      fp2 TYPE p VALUE '123456789-',
      fp3 TYPE p VALUE 1234567899,
      fp4 TYPE p VALUE 12345678901,
      fp5 TYPE p VALUE 12345,
      fp6 TYPE p VALUE 0.

fn = fc. WRITE: / fc, >, fn, 'Caracteres no-numericos son ignorados'.
fd = 'ABCDE'. WRITE: / fd, 'Campos de fecha y tiempo son invalidos'.
ft = 'ABCDE'. WRITE: / ft, ' cuando se cargan con basura'.
fp = sy-datum. WRITE: / sy-datum, >, fp, 'd->p: número de días desde 0001/01/01'.
fp = sy-uzeit. WRITE: / sy-uzeit, >, fp, 'd->t: número de segundos desde la media noche '.
fx = 'A4 B4'. WRITE: / 'A4 B4', >, fx, 'se ingnoran todos los caracteres luego del caracter invalido'.
fp = fc1. WRITE: / fc1, >, fp, 'Permite signo'.
fp = fc2. WRITE: / fc2, >, fp, 'tambien permite el signo final'.
fc = fp1. WRITE: / fp1, >, fc, 'el byte más a la derecha reservado para el signo'.
fc = fp2. WRITE: / fp2, >, fc, 'solo los números negativos lo usan, pero'.
fc = fp3. WRITE: / fp3, >, fc, '+ve algunos lo necesitan'.
fc = fp4. WRITE: / fp4, >, fc, 'La sobre carga es indicada por *'.
fc = fp5. WRITE: / fp5, >, fc, 'Los ceros a la izquierda están suprimidos'.
fc = fp6. WRITE: / fp6, >, fc, 'Entrada con cero = Salida con cero'.
fp = ' '. WRITE: / ' ', >, fp, 'Entrada en blanco = Salida en cero'.
```





## Variables y subcampos



Codigo de ejemplo

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*


REPORT ZTX010208_1_EFRAIN.

data: f1(7),
      f2(7).
      f1 = 'BOY'. 
      f2 = 'BIG'. 
      f1+0(1) = 'T'. 
	  
 write / f1.
 f1(1) = 'J'. "same as: f1+0(1) = 'J'.
 write / f1. "f1 now contains 'JOY '.
 f1(1) = f2. "same as: f1(1) = f2(1).
 write / f1. "f1 now contains 'BOY '.
 f1+4 = f1. "same as: f1+4(3) = f1(3).
 write / f1. "f1 now contains 'BOY BOY'.
 f1(3) = f2(3). "same as: f1+0(3) = f2+0(3).
 write / f1. "f1 now contains 'BIG BOY'.




```





### Move



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*


REPORT ZTX010208_1_EFRAIN.

" la instruccion move trata un campo tipo registro, sin un nombre de compoente como una variable de tipo char

DATA: f1(4) VALUE 'ABCD',
      BEGIN OF s1,
        c1(1),
        c2(2),
        c3(1),
      END OF s1.



s1 = f1. " s1 es tratada como una variable de tipo char 4

WRITE: / s1, " Escribe ABCD
  / s1-c1, s1-c2, s1-c3. " Escribe A BC D



```



## Realizando calculos matematicos

Los operadores y operandos deben estar separados por espacio.

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-28-12-54-30-image.png)





Calculo de fechas. Ejemplo:



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

" Calculos de fechas usando variables de fechas dentro de calculos.

REPORT ZTX010208_1_EFRAIN.

DATA: d1 LIKE sy-datum,
      d2 LIKE d1,
      num_days TYPE p.

d1 = d2 = sy-datum.

d1 = sy-datum - 1.
WRITE: / 'Ayer fue: ', d1. " Ayer
d2+6 = '01'. " toma los primeros seis caracteres (aniomes)y al dia le asigna 01 | yyyymmdd
WRITE: / 'Primer dia del mes actual: ', d2. " Primer dia del mes actual.
d2 = d2 - 1.
WRITE: / 'Ultimo dia del mes anterior: ', d2. " Ultimo dia del mes anterior.


```



## Asignacion dinamica (field-symbol)



Puntero que puede asignar dinamicamente a un campo

debe terminar y comenzar con <>



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

" FIELD-SYMBOL es una referencia a otro campo

REPORT ZTX010208_1_EFRAIN.

DATA f1(3) VALUE 'ABC'.
FIELD-SYMBOLS <f>.
ASSIGN f1 TO <f>. " <f> puede ser usado en lugar de f1
WRITE <f>. " escribe el contenido de f1
<f> = 'XYZ'.
WRITE / f1.



```



sa
