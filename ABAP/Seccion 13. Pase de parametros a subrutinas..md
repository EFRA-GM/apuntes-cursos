# Seccion 13. Pase de parametros a subrutinas.

```abap
DATA: f1 VALUE 'A',
      f2 VALUE 'B',
      f3 VALUE 'C'.

PERFORM: s1 USING f1 f2 f3,
         s2 USING f1 f2 f3.

FORM s1 USING p1 p2 p3.
  WRITE: / 'FORM S1'.
  WRITE: / f1, f2, f3, "!-- Valores globales
         / p1, p2, p3. "!-- Parámetros
ENDFORM.

FORM s2 USING f1 f2 f3.
  SKIP.
  WRITE: / 'FORM S2'.
  WRITE: / f1, f2, f3. "!-- Valores globales
ENDFORM.
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


DATA: f1 VALUE 'A',   "!-- Char
      f2 TYPE i VALUE 4, "!-- Integer
      f3 LIKE sy-datum,  "!-- Date
      f4 LIKE sy-uzeit.  "!-- Hora

f3 = sy-datum.
f4 = sy-uzeit.

PERFORM s1 USING f1 f2 f3 f4.

FORM s1 USING p1 TYPE c
              p2 TYPE i
              p3 TYPE d
              p4 TYPE t.
  WRITE: / 'P1 (Char) = ', p1,
  / 'P2 (Integer) = ', p2,
  / 'P3 (Date) = ', p3,
  / 'P4 (Time) = ', p4.
ENDFORM.
```

### Valores y referencias.

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-30-23-32-50-image.png)

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.



DATA: f1 VALUE 'A',
      f2 VALUE 'B',
      f3 VALUE 'C',
      f4 VALUE 'D',
      f5 VALUE 'E',
      f6 VALUE 'F'.

PERFORM s1 USING f1 f2
           CHANGING f3 f4.

PERFORM s2 USING f1 f2 f3 f4
           CHANGING f5 f6.

PERFORM s3 USING f1 f2 f3.

FORM s1 USING p1 VALUE(p2)
        CHANGING p3 VALUE(p4).
  WRITE: / p1, p2, p3, p4.
ENDFORM.

FORM s2 USING p1 VALUE(p2) VALUE(p3) p4
        CHANGING VALUE(p5) p6.
  WRITE: / p1, p2, p3, p4, p5, p6.
ENDFORM.

FORM s3 USING VALUE(p1)
        CHANGING p2 VALUE(p3).
  WRITE: / p1, p2, p3.
ENDFORM.
```

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-30-23-40-42-image.png)







## Definir y llamar a subrutinas extrernas



Progrma 1 que llama a una subrutina externa

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
DATA f1(3) VALUE 'AAA' .

scustom-id = '1000'.
PERFORM s1(ZTX010208_2_EFRAIN).
WRITE: / 'f1 = ', f1,
    / 'id = ', scustom-id.




```



Programa 2, que tiene la subrutina que se llama desde el programa 1.

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_2_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_2_EFRAIN.

TABLES scustom.
DATA f1(3).

FORM s1.
    f1 = 'zzz'.
    scustom-id = '9999'
ENDFORM.





```
