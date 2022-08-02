# Seccion 10. Tablas internas

tabla temporal almacenada en la memoria RAM en el servidor de aplicaciones.

se crea y llena mediante un progama durante la ejecucion y se descarta cuando finaliza el programa

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF gt0 OCCURS 0,
  f1(1),
  f2(2),
  END OF gt0.

BREAK-POINT. " abre el debugger de SAP. que permite ver el valore de las variables.
```

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF gt1 OCCURS 10,
  f1,
  f2,
  f3,
  END OF gt1.

DATA gt2 LIKE scustom OCCURS 100.

DATA gt3 LIKE scustom OCCURS 100 WITH HEADER LINE. " Con liena de cabezeras

BREAK-POINT.
```

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

data: begin of gt1 occurs 10, "Tiene línea de cabecera
           f1,
           f2,
           f3,
       end of gt1.

 data gt2 like scustom occurs 100. "No tiene linea de cabecera

 data gt3 like scustom occurs 100 with header line.

 data gt4 like gt3 occurs 100. "No tiene linea de cabecera

 data begin of gt5 occurs 100. "Tiene linea de cabecera
      include structure scustom.
 data end of gt5.

 data begin of gt6 occurs 100.
      include structure scustom. "La sentencia include

 data: delflag,
       rowtotal,
 end of gt6.

 data: begin of gt7 occurs 100, "
         s like scustom, "component names will be
       end of gt7. "prefixed with gt7-s-

 data gt8 like sy-index occurs 10
          with header line.

 data: begin of gt9 occurs 10,
       f1 like sy-index,
       end of gt9.



BREAK-POINT.
```

## Agregar datos a una tabla interna

### APPEND

para agregar una sola fila a una tabla interna



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

report ztx010602_1.
data: begin of gt occurs 3,
  f1(1),
  f2(2),
end of it.
it-f1 = 'A'.
it-f2 = 'XX'.

append gt to gt. "incluir línea de encabezado GT a la tabla GT

write: / 'sy-tabix =', sy-tabix.

gt-f1 = 'B'.
gt-f2 = 'YY'.

append gt. "Igual que en la linea 8

write: / 'sy-tabix =', sy-tabix.

gt-f1 = 'C'.

append gt. "La table interna ahora contiene tres filas.

write: / 'sy-tabix =', sy-tabix.

 
```



## Leyendo datos desde una tabla interna



* **loop at**: multiples filas 



loop at it [into wa] [from m] [to n] [where exp]

endloop



it = nombre tabla interna

wa = nombre del area de trabajo

exp = expresion restrictiva para leer filas



Dentro de loop **sy-tabix** contiene el numero de fila relativo del registro actual



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF it OCCURS 3,
    f1(1),
    f2(2),
  END OF it.

BREAK-POINT.

it-f1 = 'A'.
it-f2 = 'XX'.
APPEND it TO it.

it-f1 = 'B'.
it-f2 = 'YY'.
APPEND it.

it-f1 = 'C'.
APPEND it.

sy-tabix = sy-subrc = 99.
LOOP AT it.
  WRITE: / sy-tabix, it-f1, it-f2.
ENDLOOP.
WRITE: / 'Fin. sy-tabix = ', sy-tabix,
  / ' sy-subrc', sy-subrc.
  
  
  
```



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF it OCCURS 3,
    f1(1),
    f2(2),
  END OF it.

BREAK-POINT.

it-f1 = 'A'.
it-f2 = 'XX'.
APPEND it TO it.

it-f1 = 'B'.
it-f2 = 'YY'.
APPEND it.

it-f1 = 'C'.
APPEND it.

WRITE / 'Inicia loop 1:' .
LOOP AT it WHERE f2 = 'YY'.
  WRITE: / sy-tabix, it-f1, it-f2.
ENDLOOP.
SKIP.

WRITE / 'Inicia loop 2:' .
LOOP AT it TO 2.
  WRITE: / sy-tabix, it-f1, it-f2.
ENDLOOP.
SKIP.

WRITE / 'Inicia loop 3:' .
LOOP AT it from 2.
  WRITE: / sy-tabix, it-f1, it-f2.
ENDLOOP.
SKIP.

WRITE / 'Inicia loop 4:' .
LOOP AT it from 2 WHERE f1 = 'C'.
  WRITE: / sy-tabix, it-f1, it-f2.
ENDLOOP.


```





* **read table**: una sola fila

read table it [into wa]

[index i | with key  keyexp [binary search]] [comparing cmpexp]

[transporting textp]



it = nombre de la tabla interna.

wa = nombre del área de trabajo.

i = numero de fila relativo.



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF gt OCCURS 3, " reserva espacio para 3 filas
    f1(1) TYPE n,
    f2 TYPE i,
    f3(2) TYPE c,
    f4 TYPE p,
  END OF gt,
  gs LIKE gt.

" BREAK-POINT.

gt-f1 = '10'. gt-f3 = 'AA'. gt-f2 = gt-f4 = 1. APPEND gt.
gt-f1 = '20'. gt-f3 = 'BB'. gt-f2 = gt-f4 = 1. APPEND gt.
gt-f1 = '30'. gt-f3 = 'CC'. gt-f2 = gt-f4 = 1. APPEND gt.

READ TABLE gt INDEX 2.
WRITE: / 'sy-subrc = ', sy-subrc,
  / 'sy-tabix = ', sy-tabix,
  / gt-f1, gt-f2, gt-f3, gt-f4.


READ TABLE gt INTO gs INDEX 1.
WRITE: / 'sy-subrc = ', sy-subrc,
  / 'sy-tabix = ', sy-tabix,
  / gt-f1, gt-f2, gt-f3, gt-f4,
  / gs-f1, gs-f2, gs-f3, gS-f4.
  
  
READ TABLE gt INDEX 4.
WRITE: / 'sy-subrc = ', sy-subrc,
  / 'sy-tabix = ', sy-tabix,
  / gt-f1, gt-f2, gt-f3, gt-f4,
  / gs-f1, gs-f2, gs-f3, gS-f4.







```



#### Variables de ambiente en las sentencia READ TABLE



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF gt OCCURS 3, " reserva espacio para 3 filas
    f1(1) TYPE n,
    f2 TYPE i,
    f3(2) TYPE c,
    f4 TYPE p,
  END OF gt,
  BEGIN OF gs1,
    f1 LIKE gt-f1,
    f2 LIKE gt-f2,
  END OF gs1,
  gs2 LIKE gt,
  f(8).


" BREAK-POINT.

gt-f1 = '10'. gt-f3 = 'AA'. gt-f2 = gt-f4 = 1. APPEND gt.
gt-f1 = '20'. gt-f3 = 'BB'. gt-f2 = gt-f4 = 1. APPEND gt.
gt-f1 = '30'. gt-f3 = 'CC'. gt-f2 = gt-f4 = 1. APPEND gt.

READ TABLE gt WITH KEY f1 = '30' f2 = 3.
WRITE: / 'sy-subrc = ', sy-subrc,
  / 'sy-tabix = ', sy-tabix,
  / gt-f1, gt-f2, gt-f3, gt-f4.


f = 'F2'.
READ TABLE gt INTO gs2 WITH KEY (f) = 2.
WRITE: / 'sy-subrc = ', sy-subrc,
  / 'sy-tabix = ', sy-tabix,
  / gt-f1, gt-f2, gt-f3, gt-f4,
  / gs2-f1, gs2-f2, gs2-f3, gS2-f4.


CLEAR gs2.
gs2-f1 = '10'. gs2-f3 = 'AA'. gs2-f2 = gs2-f4 = 1.

READ TABLE gt WITH KEY = gs2.
WRITE: / 'sy-subrc = ', sy-subrc,
  / 'sy-tabix = ', sy-tabix,
  / gt-f1, gt-f2, gt-f3, gt-f4.

gs2-f1 = '20'. gs1-f2 = 2.
READ TABLE gt INTO gs2 WITH KEY gs1.
WRITE: / 'sy-subrc = ', sy-subrc,
  / 'sy-tabix = ', sy-tabix,
  / gt-f1, gt-f2, gt-f3, gt-f4,
  / gs2-f1, gs2-f2, gs2-f3, gs2-f4.


```

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF gt OCCURS 3, " reserva espacio para 3 filas
    f1(1) TYPE n,
    f2 TYPE i,
    f3(2) TYPE c,
    f4 TYPE p,
  END OF gt.
  


" BREAK-POINT.

gt-f1 = '10'. gt-f3 = 'AA'. gt-f2 = gt-f4 = 1. APPEND gt.
gt-f1 = '20'. gt-f3 = 'BB'. gt-f2 = gt-f4 = 2. APPEND gt.
gt-f1 = '30'. gt-f3 = 'CC'. gt-f2 = gt-f4 = 3. APPEND gt.

SORT gt BY f1 f3. " ordenar el contendido de la tabla gt por los campos f1 y f3
CLEAR gt.
gt(2) = ' '. gt-f3 = 'BB'.

READ TABLE gt BINARY SEARCH.
WRITE: / 'sy-subrc = ', sy-subrc,
  / 'sy-tabix = ', sy-tabix,
  / gt-f1, gt-f2, gt-f3, gt-f4.

CLEAR gt.
gt-f1 = '30'. gt-f3 = 'AA'.
READ TABLE gt BINARY SEARCH.
WRITE: / 'sy-subrc = ', sy-subrc,
  / 'sy-tabix = ', sy-tabix,
  / gt-f1, gt-f2, gt-f3, gt-f4,
  / gs2-f1, gs2-f2, gs2-f3, gS2-f4.


```



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF gt OCCURS 3, " reserva espacio para 3 filas
    f1(1) TYPE n,
    f2 TYPE i,
    f3(2) TYPE c,
    f4 TYPE p,
  END OF gt.
  


" BREAK-POINT.
gt-f1 = '40'. gt-f3 = 'DD'. gt-f2 = gt-f4 = 2. APPEND gt.
gt-f1 = '20'. gt-f3 = 'BB'. gt-f2 = gt-f4 = 2. APPEND gt.


SORT gt BY f1. " ordenar el contendido de la tabla gt por los campos f1
DO 5 TIMES.
  gt-f1 = sy-index * 10.
  gt-f3 = 'XX'.
  gt-f2 = gt-f4 = sy-index.
  READ TABLE  gt WITH KEY f1 = gt-f1
  BINARY SEARCH
  TRANSPORTING NO FIELDS.
  IF sy-subrc <> 0.
    INSERT gt INDEX sy-tabix.
  ENDIF.
ENDDO.

LOOP AT gt.
  WRITE: / gt-f1, gt-f2, gt-f3, gt-f4.
ENDLOOP.


```



## Ordenando el contendio de una tabla (SORT).



**sort**

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF gt OCCURS 5,
        i LIKE sy-index,
        t,
      END OF gt,
      alpha(5) VALUE 'CBABB'.

DO 5 TIMES.
  gt-i = sy-index.
  gt-t = alpha.
  APPEND gt.
  SHIFT alpha.
ENDDO.

LOOP AT gt.
  WRITE: / gt-i, gt-t.
ENDLOOP.


"!-- Clase 1
SKIP.
SORT gt BY t.
LOOP AT gt.
  WRITE: / gt-i, gt-t.
ENDLOOP.

"!-- Clase 2
SKIP.
SORT gt BY t i.
LOOP AT gt.
  WRITE: / gt-i, gt-t.
ENDLOOP.

"!-- Clase 3
SKIP.
SORT gt BY t DESCENDING i.
LOOP AT gt.
  WRITE: / gt-i, gt-t.
ENDLOOP.

"!-- Clase 4
SKIP.
SORT gt.
LOOP AT gt.
  WRITE: / gt-t.
ENDLOOP.


```


