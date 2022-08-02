# Seccion 14. Modulo de funciones

### INCLUDE

progrma en el que los contenidos estan diseñados para ser utilizados por otro progrma

Otros programas utilizan el codigo que contiene el programa de inclusion copiando las lineas de codigo en si mismo (en tiempo de ejecucion).

INCLUDE ipgm

El progrma debe ser de tipo i en los atributos del progrma.

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-16-44-39-image.png)

```abap
*&---------------------------------------------------------------------*
*& Include  ZTX_001_3
*&
*&---------------------------------------------------------------------*

SELECT * FROM sbook WHERE carrid = p_carrid.
WRITE: / sbook-carrid, sbook-customid, sbook-class
ENDSELECT.
```

```abap
*&---------------------------------------------------------------------*
*& Include  ZTX_001_2
*&
*&---------------------------------------------------------------------*
```

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_2_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*


REPORT ZTX010208_2_EFRAIN.

TABLES: scarr, sbook.

PARAMETERS p_carrid TYPE S_CARR_ID OBLIGATORY DEFAULT space.

INCLUDE: ZTX_001_2,
    ZTX_001_3

TOP-OF-PAGE.
    WRITE: / 'cod. linea: ', p_carrid.
```





### Grupo de funciones.



Trx **SE80**

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-17-45-09-image.png)



Para crear un nuevo grupo escribe el nombre que aun no existe, dar enter y dar clic  en Yes.

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-17-46-40-image.png)

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-17-47-43-image.png)

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-17-48-00-image.png)



Para activar

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-17-50-28-image.png)

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-17-51-21-image.png)





Para Crear  un modulo de funciones:

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-17-53-24-image.png)



Trx **SE37**

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-17-56-45-image.png)



![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-05-29-image.png)



![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-02-56-image.png)



![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-04-37-image.png)





## Definicion de la interfaz



### IMPORTING

variables que se pasan al modulo desde el programa que realiza la llamada. estos valores se originan fuera del modulo de funciones y se importan en el

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-08-30-image.png)





### EXPORTING

Son variables que contienen valores devueltos desde el modulo de funciones. Estos valores se originan dentro del modulo de funciones y se exportan fuera de el.



### CHANGING

Variables que se pasan al modulo de funciones, se modifican mediante el codigo dentro del modulo de funciones y luego se devuelven.



### TABLE

tablas internas que se pasan al modulo de funciones, se modifican dentro de el y se devuelven. 

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-12-19-image.png)



### EXCEPTION

nombre de un error que ocurre dentro de un modulo de funciones.

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-13-19-image.png)





### Practica



![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-18-31-image.png)



![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-19-29-image.png)



Source code

```abap
FUNCTION Z_MF_SUMA.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     REFERENCE(IV_VALOR1) TYPE  I DEFAULT 0
*"     REFERENCE(IV_VALOR2) TYPE  I DEFAULT 0
*"  EXPORTING
*"     REFERENCE(EV_SUMA) TYPE  I
*"----------------------------------------------------------------------

DATA: iv_resultado TYPE  i.
iv_resultado = iv_valor1 + iv_valor2.

ev_suma = iv_resultado.



ENDFUNCTION.
```

Activar.



Creando un programa que utilice el modulo de funciones anterior. (desde Trx **SE38**)

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-26-35-image.png)



![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-27-24-image.png)



codigo fuente

```abap
REPORT ZTX011002.

DATA: gv_resul TYPE i.

PARAMETERS: p_valor1 TYPE  i DEFAULT 0,
  p_valor2 TYPE i DEFAULT 0.

START-OF-SELECTION.
  " Llama a una funcion con parametros
  CALL FUNCTION 'Z_MF_SUMA'
   EXPORTING
     IV_VALOR1       = p_valor1
     IV_VALOR2       = p_valor2
   IMPORTING
     EV_SUMA         = gv_resul
            .



WRITE: / 'Valor 1 = ', p_valor1.
WRITE: / 'Valor 2 = ', p_valor2.

WRITE: / 'Resultado = ', gv_resul.
```





### Crear un matchcode



Trx SE11

crear un dominio nuevo

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-40-30-image.png)



![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-41-32-image.png)

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-43-15-image.png)

Guardar, objeto local

Activar

Copiar

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-45-00-image.png)

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-18-45-31-image.png)



Ahora creamos un tipo de dato



![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-19-01-10-image.png)



![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-19-01-33-image.png)

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-19-06-19-image.png)

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-19-07-09-image.png)



Guardar

Modificar el programa para que pida el tipo de operacion y permita seleccionar de la lista de operaciones en el tipo de dato.

```abap
REPORT ZTX011002.

DATA: gv_resul TYPE i.

PARAMETERS: p_oper TYPE Z_E_OPER OBLIGATORY VALUE CHECK,
  p_valor1 TYPE  i DEFAULT 0,
  p_valor2 TYPE i DEFAULT 0.

START-OF-SELECTION.
  " Llama a una funcion con parametros
  CALL FUNCTION 'Z_MF_SUMA'
   EXPORTING
     IV_VALOR1       = p_valor1
     IV_VALOR2       = p_valor2
   IMPORTING
     EV_SUMA         = gv_resul
            .



WRITE: / 'Valor 1 = ', p_valor1.
WRITE: / 'Valor 2 = ', p_valor2.

WRITE: / 'Resultado = ', gv_resul.
```



El resultado es el siguiente:

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-19-11-59-image.png)





Renombrar la funcion de suma

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-19-29-34-image.png)



![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-19-30-27-image.png)

agregar nuevo parametro

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-20-05-00-image.png)



Modificar el codigo fuente.

```abap
FUNCTION Z_MF_OPERACION .
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     REFERENCE(IV_OPER) TYPE  Z_E_OPER DEFAULT '+'
*"     REFERENCE(IV_VALOR1) TYPE  I DEFAULT 0
*"     REFERENCE(IV_VALOR2) TYPE  I DEFAULT 0
*"  EXPORTING
*"     REFERENCE(EV_ENTERO) TYPE  I
*"     REFERENCE(EV_DECIMAL) TYPE  DECIMAL21_7
*"  EXCEPTIONS
*"      EX_DIVISIO_POR_CERO
*"----------------------------------------------------------------------

DATA: iv_resultado TYPE  i,
      iv_decimals TYPE decfloat16.

CASE IV_OPER.
  WHEN '1'.
    ev_entero = iv_valor1 + iv_valor2.
  WHEN '2'.
    ev_entero = iv_valor1 - iv_valor2.
  WHEN '3'.
    ev_entero = iv_valor1 * iv_valor2.
  WHEN '4'.
    IF iv_valor2 eq 0.
      RAISE EX_DIVISIO_POR_CERO.
    ELSE .
      ev_decimal = iv_valor1 / iv_valor2.
    ENDIF.

  WHEN OTHERS.
ENDCASE.








ENDFUNCTION.
```



Crear Exception

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-19-40-47-image.png)



Modificar parametros de salida

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-20-05-14-image.png)



Modificar programa que llama

```abap
REPORT ZTX011002.

DATA: gv_resul TYPE i,
      gv_decimal TYPE DECFLOAT16.

PARAMETERS: p_oper TYPE Z_E_OPER OBLIGATORY VALUE CHECK,
  p_valor1 TYPE  i DEFAULT 0,
  p_valor2 TYPE i DEFAULT 0.

START-OF-SELECTION.
  " Llama a una funcion con parametros
  CALL FUNCTION 'Z_MF_OPERACION'
   EXPORTING
     IV_OPER                   = p_oper
     IV_VALOR1                 = p_valor1
     IV_VALOR2                 = p_valor2
   IMPORTING
     EV_ENTERO                 = gv_resul
     EV_DECIMAL                = gv_decimal
   EXCEPTIONS
     EX_DIVISIO_POR_CERO       = 1
     OTHERS                    = 2
            .
  IF SY-SUBRC EQ 1.
    MESSAGE 'Division por cero' TYPE 'E'.
  ELSE.
    WRITE: / 'Valor 1 = ', p_valor1.
    WRITE: / 'Valor 2 = ', p_valor2.

    IF p_oper EQ 4.
      WRITE: / 'Resultado =', gv_decimal.
    ELSE.
      WRITE: / 'Resultado =', gv_resul.
    ENDIF.
  ENDIF.

            .
```





## Pasando una tabla interna a traves de tables



Crear nuevo programa ZTX011004

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX011004
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*
REPORT ZTX011004.

INCLUDE ZTX011004_TOP.
INCLUDE ZTX011004_001.
```



```abap
*&---------------------------------------------------------------------*
*&  Include           ZTX011004_TOP
*&---------------------------------------------------------------------*

tables: SCARR, SBOOK.

data: GT_SBOOK type table of SCARR,
      GS_SCARR type SCARR.
```



```abap
*&---------------------------------------------------------------------*
*&  Include           ZTX011004_001
*&---------------------------------------------------------------------*

PARAMETERS: pcarrid TYPE S_CARR_ID.
SELECT-OPTIONS: sfldate FOR sbook-fldate.
```



Crear una estructura para que se pueda usar de parametro la tabla con rangos de fechas y activarlo

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-21-26-53-image.png)

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-21-27-13-image.png)

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-21-36-04-image.png)



Crear un tipo de datos

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-21-36-40-image.png)

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-21-36-58-image.png)

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-21-38-09-image.png)



Otra estructura para le export

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-21-47-36-image.png)

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-21-49-47-image.png)

 ![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-21-51-50-image.png)



Activar



Crear un modulo de funciones

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-21-16-46-image.png)



![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-21-20-58-image.png)



![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-21-39-42-image.png)



![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-31-21-43-08-image.png)



Source code

```abap
FUNCTION Z_MF_CONSULTA_BOOK.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     REFERENCE(IV_CARRID) TYPE  S_CARR_ID
*"     REFERENCE(IT_FLDATE) TYPE  ZTDATES
*"  EXPORTING
*"     REFERENCE(ET_SBOOK) TYPE  Z_T_SBOOK
*"----------------------------------------------------------------------

  SELECT * FROM sbook INTO CORRESPONDING FIELDS OF et_book
    WHERE CARRID = IV_CARRID
    AND FLDATE IN IT_FLDATE.



ENDFUNCTION.
```



    CONCLUSION: TODO REVUELTO. NO SE ENTIENDE EL ORDEN DE LOS PASOS EN EL VIDEO Y HACE CAMBIOS QUE NO QUEDARON GRABADOS.















## Encontrar un modulo de funcion



Este pdf tiene una lista de modulos de funciones incluidos en sap y los tiene agrupados por diferentes categorias.

Function Modules in ABAP® 

Tanmaya Gupta



para ver una de ellas ir a Trx **SE37** y buscar el modulo
