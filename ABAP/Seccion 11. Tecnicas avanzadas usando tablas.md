# Seccion 11. Tecnicas avanzadas usando tablas

 

### Programa para obtener informacion de una tabla interna



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF it  OCCURS 3,
  f1 VALUE 'X',
END OF it,
n TYPE i.


IF it[] is INITIAL. " Valida si la tabla está en el estado inicial
  WRITE: / 'Esta vacio'.
ENDIF.

APPEND: it, it, it. " Usando los append it 3 veces

IF NOT  it[] IS INITIAL. " VAlida si la tabla NO esta en el estado inicial
  WRITE: / 'No es ta vacio'.
ENDIF.

WRITE: / 'Numero de filas desde: sy-tabix:', sy-tabix. " numero de filas totales

DESCRIBE TABLE it LINES n. " Contamos cuantas lineas tiene la tabla interna, asignando a la variable n

" para obtener el valor de las siguientes variables es necesario ejecutar antes la instruccion DESCRIBE
WRITE : / 'Numero de filas desde : sy-tfill: ', sy-tfill, " Numero de filas
  / 'Tamaño de las filas desde: sy-tleng:', sy-tleng, " Tamaño de una fila en bytes
  / 'occurs value desde: ', sy-toccu . " Valor actual de la clausula occurs"



```





## Copiando datos de una tabla interna a otra



Para duplicar de una estructura a otra que tienen una definicion parecida use lo siguiente. Pasa el contenido de it1 a it2

it2[] = it1[]



Cualquier contenido existente en it2 se sobreescribe.

Es la forma mas eficiente de copiar el contendid de una tabla a otra.





Para anexar filas al final de la tabla de destino utilice la instruccion **APPEND LINES**



APPEND LINES OF it1 [FROM nf] [TO nt] TO it2



it1, it2 son tablas internas 

nf, nt son variables numericos, literales o constantes.





Para insertar filas en un lugar que no sea el final en la tabla de destino utilice **INSERT LINES**.



INSERT LINES OF it1 [FROM nf] [TO nf] INTO it2 [INDEX nb]





```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF it1 OCCURS 10,
  f1,
END OF it1,
it2 LIKE it1 OCCURS 10 WITH HEADER LINE,
alpha(10) VALUE 'ABCDEFGHIJ',
linea TYPE i.


" Rellena la tabla 1 con 10 registros
DO 10 TIMES.
  linea = sy-index - 1.
  APPEND alpha+linea(1) TO it1.
ENDDO.

" copia de la tabla 1, desde la linea 2 hasta la linea 5, en la tabla 2.
APPEND LINES OF it1 FROM 2 TO 5 to it2.
LOOP AT it2.
  WRITE it2-f1.
ENDLOOP.

" copia de la tabla 1, desde la fila 8, en la fila 2 de la tabla 2.
INSERT LINES OF it1 FROM 8 INTO it2 INDEX 2.
SKIP.
LOOP AT it2.
  WRITE it2-f1.
ENDLOOP.

LOOP AT it2.
  IF it2-f1 >= 'E'.
    INSERT LINES OF it1 TO 1 INTO it2.
  ENDIF.
ENDLOOP.

SKIP.
LOOP AT it2.
  WRITE it2-f1.
ENDLOOP.

SKIP.
it2[] = it1[]. " copia el contenido de la tabla 1 en la tabla 2,  
LOOP AT it2.
  WRITE it2-f1.
ENDLOOP.


```



## Comparando el contenido de dos tablas internas.



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF it OCCURS 5,
  f1 LIKE sy-index,
END OF it.

DO 5 TIMES.
  it-f1 = sy-index. " inserta un registro con el indice de la iteracion
  APPEND it.
ENDDO.

" Crear un nuevo registr de it y lo inserta en la tabla en el indice 3, recorreiendo los elementos.
it-f1 = -99.
INSERT it INDEX 3.

LOOP AT it.
  WRITE / it-f1.
ENDLOOP.

LOOP AT it WHERE f1 >= 4.
  it-f1 = -88.
  INSERT it. " Como no se especifica en que indice se va a insertar el nuevo registro se inserta en sy-index (donde sea >=4) y se recorren los demas
ENDLOOP.

SKIP.
LOOP AT it.
  WRITE / it-f1.
ENDLOOP.


```





### Modificacion de filas en una tabla interna



Para modificar el contenido de una o varias filas en una tabla interna, use la sentencia modify.



MODIFY it [FROM wa] [INDEX n] [TRANSPORTING c1 c2 ... [WHERE exp]]



```abap

*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF it OCCURS 5,
  f1 LIKE sy-index,
  f2,
END OF it,
alpha(10) VALUE 'ABCDEFGHIJ',
linea TYPE i.


" Rellena la tabla con indice y la letra en el indice
DO 5 TIMES.
  it-f1 = sy-index.
  linea = sy-index - 1.
  it-f2 = alpha+linea(1).
ENDDO.

" Modifica el registros de la fila 4 con la letra z
it-f2 = 'z'.
MODIFY it INDEX 4.

" Mustra la tabla modificada
LOOP AT it.
  WRITE: / it-f1, it-f2.
ENDLOOP.

" Vuelve a modificar la primer columna de la tabla, los multiplica por 2
LOOP AT it.
  it-f1 = it-f1 * 2.
  MODIFY it.
ENDLOOP.
  
" Mustra la tabla modificada.
SKIP.
LOOP AT it.
  WRITE: / it-f1, it-f2.
ENDLOOP.

" De los registros donde f1 <> 10 se modifica solo el componente f2, con la letra x
it-f2 = 'x'.
MODIFY it TRANSPORTING f2 WHERE f1 <> 10.

" Se muetra la tabla modificada.
SKIP.
LOOP AT it.
  WRITE: / it-f1, it-f2.
ENDLOOP.



```



## Borrar el contenido de una tabla



FREE

    FREE it



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
  f1 LIKE sy-index,
END OF it.


" Rellena la tabla
DO 3 TIMES.
  it-f1 = sy-index.  
  APPEND it.
ENDDO.

LOOP AT it.
  WRITE it-f1.
ENDLOOP. 

FREE it.
IF it[] IS INITIAL'
  WRITE: / 'No existen registros luego de la sentencia free'.
ENDIF.



```



REFRESH

refresh elimina las filas pero deja la memoria asignada. Se utiliza cuando se desea elminar las filas de una tabla pero se planea volver a llenar la tabla interna.



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
  f1 LIKE sy-index,
END OF it,
i LIKE sy-index.



" Rellena la tabla
DO 3 TIMES.
  it-f1 = sy-index.

  " Inserta 3 registros a la tabla
  DO 3 TIMES.
    it-f1 = i * sy-index.
    APPEND it.
  ENDDO.
  " Mustra el contenido de la tabla
  WRITE: / ''.
  LOOP AT it.
    WRITE it-f1.
  ENDLOOP.

  " limpia las tres filas que se agregaron, pero se sigue conservando la memoria, porque se piensa volver a llenar nuevamente.
  REFRESH it.
ENDDO.


" Se limpian los registros de la tabla pero en esta ocacion se libera la memoria.
FREE it.

IF it[] IS INITIAL.
  SKIP.
  WRITE 'La tabla it esta vacia'.
ENDIF.



```





### CLEAR



CLEAR it | CLEAR it[]



CLEAR it[] borra todas las filas

CLEAR it borra la linea de cabecera.

ambos dejan la memoria asignada.



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
        f1,
      END OF it.

it-f1 = 'X'.
APPEND: it, it.

CLEAR it. " borra las lineas de cabecera. 
WRITE: 'f1=', it-f1.

WRITE: / ''.
LOOP AT it.
  WRITE it-f1.
ENDLOOP.

CLEAR it[]. "!-- mismo efecto que usar REFRESH.
LOOP AT it.
  WRITE it-f1.
ENDLOOP.
WRITE: / 'sy-subrc=', sy-subrc.
```





### DELETE



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF it OCCURS 12,
        f1,
      END OF it,
      alpha(12) VALUE 'ABCDEFGHIJKL',
      car type i.

DO 12 TIMES.
  car = sy-index - 1.
  APPEND alpha+car(1) to it.
ENDDO.

LOOP AT it.
  WRITE: / sy-tabix, it-f1.
ENDLOOP.

DELETE it INDEX 5.
SKIP.
LOOP AT it.
  WRITE: / sy-tabix, it-f1.
ENDLOOP.

DELETE it FROM 6 TO 8.
SKIP.
LOOP AT it.
  WRITE: / sy-tabix, it-f1.
ENDLOOP.

DELETE it WHERE f1 BETWEEN 'B' AND 'D'.
SKIP.
LOOP AT it.
  WRITE: / sy-tabix, it-f1.
ENDLOOP.

LOOP AT it WHERE f1 BETWEEN 'E' AND 'J'.
  DELETE it.
ENDLOOP.

SKIP.
LOOP AT it.
  WRITE: / sy-tabix, it-f1.
ENDLOOP.

READ TABLE it WITH KEY f1 = 'K' BINARY SEARCH.
WRITE: /, / 'sy-subrc=', sy-subrc, 'sy-tabix=', sy-tabix, / ''.
IF sy-subrc = 0.
  DELETE it INDEX sy-tabix.
ENDIF.

SKIP.
LOOP AT it.
  WRITE: / sy-tabix, it-f1.
ENDLOOP.

FREE it.




```





### Sumarizacion (COLLECT)



Sirve para ir sumando cantidades en reportes.



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF it OCCURS 10,
        date      TYPE d,               "    parte de la clave
        tot_sales TYPE p DECIMALS 2,    "no es parte de la clave
        name(10),                       "    parte de la clave
        num_sales TYPE i VALUE 1,       "no es parte de la clave
      END OF it.

it-date      = '19980101'.
it-tot_sales  = 100.
it-name       = 'Jack'.
COLLECT it.

it-date       = '19980101'.
it-tot_sales  = 200.
it-name       = 'Jim'.
COLLECT it.

it-date       = '19980101'.
it-tot_sales  = 300.
it-name       = 'Jack'.
COLLECT it.

it-date       = '19980101'.
it-tot_sales  = 400.
it-name       = 'Jack'.
COLLECT it.

it-date       = '19980101'.
it-tot_sales  = 500.
it-name       = 'Jim'.
COLLECT it.

it-date       = '19980101'.
it-tot_sales  = 600.
it-name       = 'Jane'.
COLLECT it.

it-date       = '19980102'.
it-tot_sales  = 700.
it-name       = 'Jack'.
COLLECT it.

LOOP AT it.
  WRITE: / it-date, it-tot_sales, it-name, it-num_sales.
ENDLOOP.







```



## Llenar una tabla interna con una tabla de base de datos.



Existen dos formas principales:

* seleccionando multiple filas directamente en el cuerpo de una tabla interna.

* seleccionando filas individuales en un area de trabajo y luego agregando.



### Seleccionando multiples filas en tabla interna.



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

DATA it LIKE scustom OCCURS 0 WITH HEADER LINE.

SELECT * FROM scustom INTO TABLE it.
LOOP AT it.
  WRITE: / it-id, it-name.
ENDLOOP.

SELECT * FROM scustom INTO TABLE it
WHERE id BETWEEN '9' AND '30'.
  
SKIP.
LOOP AT it.
  WRITE: / it-id, it-name.
ENDLOOP.

FREE it.


```



Asignar informacion en tabla interna con diferente una columna extra.

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

DATA BEGIN OF it OCCURS 2.
INCLUDE STRUCTURE scustom.
DATA: invoice_amt TYPE p,
      END OF it.

SELECT * FROM scustom INTO TABLE it WHERE country = 'NL '.
LOOP AT it.
  WRITE: / it-id, it-country, it-region, it-invoice_amt.
ENDLOOP.

SKIP.   
" ASigna solo los campos que le correspoden a los seleccionados en el select.
SELECT id country region FROM scustom INTO CORRESPONDING FIELDS OF TABLE it WHERE country = 'DE '.
LOOP AT it.
  WRITE: / it-mandt, it-id, it-country, it-region.
ENDLOOP.
FREE it.





```



# 






