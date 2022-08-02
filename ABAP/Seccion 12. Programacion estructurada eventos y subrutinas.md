# Seccion 12. Programacion estructurada: eventos y subrutinas.



Abap ofrece tres tpos de unidades de modularizacion

* eventos

* Subrutinas

* Modulo de funciones



### Modularizacion

elimina codigo redundante dentro de su programa y hacer que su programa sea mas facil de leer.    





### Eventos

Etiqueta que identifica una seccion de codigo.

* initalization

* start-of-selection

* end-of-selection



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.


INITIALIZATION.
  WRITE '1 INITIALIZATION.'.


START-OF-SELECTION.
  WRITE '2 START-OF-SELECTION'.
  
 END-OF-SELECTION.
  WRITE '3 END-OF-SELECTION'.
  
  

```



### Programa controlador.

  

Programa que controla a otro programa

Cuando inicia su programa, un programa controlador siempre se inicia primero y luego  llama a los eventos de su programa.



### Evento activo.

El codigo asociado con un evento es activado por una declaracion en un programa controlador.

los eventos son activados por el progrma del controlador en una secuencia predefinida y predecible.



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA f1 TYPE i VALUE 1.


INITIALIZATION.
  WRITE: / '1 INITIALIZATION f1 = ', f1 .
  ADD 1 TO f1.


START-OF-SELECTION.
  WRITE: / '2 START-OF-SELECTION f1 = ', f1 .
  f1 = 99.

 END-OF-SELECTION.
  WRITE: / '3 END-OF-SELECTION f1 = ', f1 .
  
  
  
```





![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-30-22-12-04-image.png)





### Categoria de los eventos.



* **drive:** Activados por el programa controlador.\
  
  * initialization
  
  * at selection-screen
  
  * start-of-selection
  
  * get
  
  * end-of-selection

* **user**: activados por el usuaurio a traves de la interfaz de usuario.
  
  * at line-selection
  
  * at pfn
  
  * at user-command

* **program**: se activan desde tu progrma
  
  * top-of-page
  
  * end-of-page





```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

PARAMETERS p1(8)  .

WRITE: / 'p1 = ', p1.

INITIALIZATION.
  p1 = 'Init'.
  
END-OF-SELECTION.
  WRITE: /(14) sy-uline,
    / 'Final del programa'.

TOP-OF-PAGE.
  WRITE: / 'Este es mi titulo.'.
  SKIP.
  
  
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


"Ejecute uno:
PARAMETERS: exit_sos RADIOBUTTON GROUP g1, "salida en: start-of-selection
            exit_eos RADIOBUTTON GROUP g1, "salida en: end-of-selection
            chck_sos RADIOBUTTON GROUP g1, "check en:  start-of-selection
            chck_eos RADIOBUTTON GROUP g1, "check en:  end of selection
            stop_sos RADIOBUTTON GROUP g1, "stop  en:  start-of-selection
            stop_eos RADIOBUTTON GROUP g1, "stop  en:  end-of-selection
            none     RADIOBUTTON GROUP g1. "No se detiene en stop, exit o check

INITIALIZATION.
*    exit.                          "event
*   check 1 = 2.                   "event
*   stop.                          "No
   chck_sos = 'X'.

AT SELECTION-SCREEN OUTPUT.
*   exit.                          "exits event
*   check 1 = 2.                   "exits event
*   stop.                          "don't do this
  MESSAGE s789(zk) WITH 'at selection-screen output'.

AT SELECTION-SCREEN ON RADIOBUTTON GROUP g1.
*   exit.                          "exits event
*   check 1 = 2.                   "exits event
*   stop.                          "va hasta end-of-selection
  MESSAGE i789(zk) WITH 'at selection-screen on rbg'.

AT SELECTION-SCREEN.
*   exit.                          "exits event
*   check 1 = 2.                   "exits event
*   stop.                          "va hasta end-of-selection
  MESSAGE i789(zk) WITH 'at selection-screen'.

START-OF-SELECTION.
  WRITE: / 'Inicio de Start-of-Selection'.
  IF      exit_sos = 'X'.
    EXIT.                      "exits report
  ELSEIF  chck_sos = 'X'.
    CHECK 1 = 2.               "exits event
  ELSEIF  stop_sos = 'X'.
    STOP.                      "va hasta end-of-selection
  ENDIF.
  WRITE: / 'Final de Start-of-Selection'.

END-OF-SELECTION.
  WRITE: / 'Inicio End-of-Selection'.
  IF      exit_eos = 'X'.
    EXIT.                      "exits report
  ELSEIF  chck_eos = 'X'.
    CHECK 1 = 2.               "exits report
  ELSEIF  stop_eos = 'X'.
    STOP.                      "exits report
  ENDIF.
  WRITE: / 'Fínal del End-of-Selection'.
  WRITE: / '1',
         / '2',
         / '3'.

TOP-OF-PAGE.
  WRITE: / 'Titulo'.
*   exit.                  "exits event and returns to write statement
*   check 'X' = 'Y'.       "exits event and returns to write statement
*   stop.                  "va hacia end-of-selection - no se hace "write"
  ULINE.

END-OF-PAGE.
  ULINE.
*   exit.                  "exits report
*   check 'X' = 'Y'.       "exits event and returns to write statement
*   stop.                  "va hacia el "end-of-selection" - no se hace "write"
  WRITE: / 'Pie de página'.
  
  
  
  
```





### Llamando subrutinas internas



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DO 3 TIMES.
  PERFORM sy-index OF s1 s2 s3.
ENDDO.

FORM s1.
  WRITE: / 'Hola desde la rutina S1'  .
ENDFORM.


FORM s2.
  WRITE: / 'Hola desde la rutina S2'  .
ENDFORM.


FORM s3.
  WRITE: / 'Hola desde la rutina S3'  .
ENDFORM.
```



### Saliendo de una subrutina.



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.


DATA f1 VALUE 'X'.

CLEAR sy-subrc.
PERFORM s1. WRITE: / 'sy-subrc =', sy-subrc.
PERFORM s2. WRITE: / 'sy-subrc =', sy-subrc.
PERFORM s3. WRITE: / 'sy-subrc =', sy-subrc.
PERFORM s4. WRITE: / 'sy-subrc =', sy-subrc.

END-OF-SELECTION.
  WRITE:  'STOP, sy-subrc =', sy-subrc.
  IF sy-subrc = 7.
    STOP.
  ENDIF.
  WRITE: / 'Luego del STOP'.

FORM s1.
  DO 4 TIMES.
    EXIT.
  ENDDO.
  WRITE / 'En la rutina S1'.
  EXIT.
  WRITE / 'Luego del EXIT'.
ENDFORM.

FORM s2.
  DO 4 TIMES.
    CHECK f1 = 'S'.
    WRITE / sy-index.
  ENDDO.
  WRITE / 'En la rutina S2'.
  CHECK f1 = 'S'.
  WRITE / 'Luego del CHECK'.
ENDFORM.

FORM s3.
  DO 4 TIMES.
    sy-subrc = 7.
    STOP.
    WRITE / sy-index.
  ENDDO.
ENDFORM.

FORM s4.
  WRITE: / 'En la rutina S4'.
ENDFORM.



```










