# Seccion 9. Sentencias de control.



## IF



#### Operadores logicos

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-28-13-46-00-image.png)



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*



REPORT ZTX010208_1_EFRAIN.


 data: begin of s1,
        x value 'X',
        y value 'Y',
        z value 'Z',
      end of s1,
      begin of s2,
        x value 'X',
        z value 'Z',
      end of s2.

 if s1-x = s2-x.
   write: / s1-x, '=', s2-x.
 else.
   write: / s1-x, '<>', s2-x.
 endif.

 if s1-x between s2-x and s2-z.
   write: / s1-X, 'esta entre', s2-x, ' y ', s2-z.
 else.
   write: / s1-X, 'no esta entre ', s2-x, ' y ', s2-z.
 endif.

 if s1 = s2. "comparación de campos byte a byte
   write: / 's1 = s2'.
 else.
   write: / 's1 <> s2'.
 endif.

 if 0 = ' '. "Cuidado con este !!
   write: / '0 = '' '''.
 else.
   write: / '0 <> '' '''.
 endif.
 
 
 
 

```

Anidando sentencias IF ELSE



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

" Ejemplo de IFs anidados

REPORT ZTX010208_1_EFRAIN.

 parameters: f1 default 'A',
  f2 default 'B',
  f3 default 'C'.

 if f1 = f2.
   write: / f1, '=', f2.
 elseif f1 = f3.
   write: / f1, '=', f3.
 elseif f2 = f3.
   write: / f2, '=', f3.
 else.
   write: / 'all fields are different'.
 endif.



 if f1 = f2.
   write: / f1, '=', f2.
 else.
   if f1 = f3.
     write: / f1, '=', f3.
   else.
     if f2 = f3.
       write: / f2, '=', f3.
     else.
       write: / 'Todos los campos son diferentes'.
     endif.
  endif.
endif.



```



### Comparando cadena de caracteres

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

* operator: co
 write / '''AABB'' co ''AB'''.
 if 'AABB' co 'AB'. write 'True'. else. write 'False'. endif.
 write / '''ABCD'' co ''ABC'''.
 if 'ABCD' co 'ABC'. write 'True'. else. write 'False'. endif.

* operator: cn
 write / '''AABB'' cn ''AB'''.
 if 'AABB' cn 'AB'. write 'True'. else. write 'False'. endif.
 write / '''ABCD'' cn ''ABC'''.
 if 'ABCD' cn 'ABC'. write 'True'. else. write 'False'. endif.

* operator: ca
 write / '''AXCZ'' ca ''AB'''.
 if 'AXCZ' ca 'AB'. write 'True'. else. write 'False'. endif.
 write / '''ABCD'' ca ''XYZ'''.
 if 'ABCD' ca 'XYZ'. write 'True'. else. write 'False'. endif.

* operator: na
 write / '''AXCZ'' na ''ABC'''.
 if 'AXCZ' na 'ABC'. write 'True'. else. write 'False'. endif.
 write / '''ABCD'' na ''XYZ'''.
 if 'ABCD' na 'XYZ'. write 'True'. else. write 'False'. endif.
 
 
```



### CASE



```abap

parameters f1 type i default 2.

 case f1.
   when 1. write / 'f1 = 1'.
   when 2. write / 'f1 = 2'.
   when 3. write / 'f1 = 3'.
   when others. write / 'f1 is not 1, 2, or 3'.
 endcase.

"!-- Estas sentencias IF son las equivalentes a la
"!-- anterior CASE

 if f1 = 1. write / 'f1 = 1'.
 elseif f1 = 2. write / 'f1 = 2'.
 elseif f1 = 3. write / 'f1 = 3'.
 else. write / 'f1 is not 1, 2, or 3'.
 endif.
 
 
 
```



### EXIT

Evita que se produsca un procesamiento posterior a este.  un uso comun es dentro de loops



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

WRITE: / 'Hola'.
EXIT.
WRITE: / 'Mundo'. " Esta linea ya no se ejecuta porque esta despues de EXIT


```



## DO

mecanismo basico de loop



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

" SY-INDEX contiene el numero de iteracion actual.

sy-index = 99.
write: / 'Antes del loop, sy-index =', sy-index, / ''.

do 5 times.
  write sy-index.
enddo.

write: / 'despues del loop, sy-index =', sy-index, / ''.

do 4 times.
  write: / 'outer loop, sy-index =', sy-index.
  do 3 times.
    write: / ' inner loop, sy-index =', sy-index.
  enddo.
enddo.

write: / ''. "new line

do 10 times.
  write sy-index.
  if sy-index = 3.
    exit.
  endif.
enddo.



```





## WHILE



similar a DO

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: a TYPE i.
a = 0.
WHILE a <> 8.
  WRITE: / 'Esta es la linea: ', a.
  a = a + 1.
ENDWHILE.



```



## CONTINUE



Se brinca a la siguiente iteracion del ciclo ignorando la parte de abajo del codigo.



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DO 5 TIMES.
  IF sy-index = 3.
    CONTINUE.
  ENDIF.
  WRITE / sy-index.
ENDDO.



```



## CHECK

Dentro de un bucle.

casi similar a CONTINUE, pasando el control de inmediato a la declaracion de terminacion de ciclo y evitando las declaraciones.



acepta una expresion logica. si la expresion es verdadera hace nada, de lo contrario salta al final del ciclo.



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA reminder TYPE i.

DO 20 TIMES.
  reminder = sy-index mod 2.
  CHECK reminder = 0. " Si el modulo es igual 0 entonces continua el flujo e imprime el write, de lo contrario se lo salta y no se mustra en pantalla
  WRITE: / sy-index.
ENDDO.


```






