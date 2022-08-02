# Seccion 15. Modulos de funciones ALV

### Realizando un reporte ALV.

* Crear un reporte nuevo ABAP

* Se incluye TYPE POOL SLIS

* Definicion de las estructuras

* Conociendo los datos que se desa mostrar

* Se copia el modulo de funciones REUSE_ALV_LIST_DISPLAY

### Parametros requeridos

* I_CALLBACK_PROGRAM

* IT_FIELDCAT

* T_OUTTAB

### Practica

ZTX01101_1 desde Trx **SE38**

```abap
REPORT ZTX011101_1.

TYPE-POOLS slis.

DATA: BEGIN OF gt_scustom OCCURS 0,
        id        TYPE s_customer,
        name      TYPE s_custname,
        form      TYPE s_form,
        street    TYPE s_street,
        postbox      TYPE s_postbox,
        postcode  TYPE postcode,
        city      TYPE city,
        country      TYPE s_country,
        region    TYPE s_region,
        telephone TYPE s_phoneno,
        custtype  TYPE s_custtype,
        discount  TYPE s_discount,
        langu        TYPE spras,
        email        TYPE s_email,
        webuser      TYPE s_webname,
      END OF gt_scustom.

DATA: gt_fieldcat TYPE slis_t_fieldcat_alv.

START-OF-SELECTION.

  PERFORM data_load.

  PERFORM fieldcat_get.

  PERFORM show_alv.


FORM data_load.

  SELECT * INTO CORRESPONDING FIELDS OF TABLE gt_scustom
    FROM scustom.

ENDFORM.

FORM fieldcat_get.

  " esta rutina explora la tabla interna
  CALL FUNCTION 'REUSE_ALV_FIELDCATALOG_MERGE'
    EXPORTING
      i_program_name         = sy-repid
      i_internal_tabname     = 'GT_SCUSTOM'
      i_structure_name       = 'SCUSTOM'
*     I_CLIENT_NEVER_DISPLAY = 'X'
      i_inclname             = sy-repid
*     I_BYPASSING_BUFFER     =
*     I_BUFFER_ACTIVE        =
    CHANGING
      ct_fieldcat            = gt_fieldcat
    EXCEPTIONS
      inconsistent_interface = 1
      program_error          = 2
      OTHERS                 = 3.
  IF sy-subrc <> 0.
* Implement suitable error handling here
  ENDIF.


ENDFORM.

FORM show_alv.

  CALL FUNCTION 'REUSE_ALV_LIST_DISPLAY'
    EXPORTING
*     I_INTERFACE_CHECK  = ' '
*     I_BYPASSING_BUFFER =
*     I_BUFFER_ACTIVE    = ' '
      i_callback_program = sy-repid
*     I_CALLBACK_PF_STATUS_SET       = ' '
*     I_CALLBACK_USER_COMMAND        = ' '
*     I_STRUCTURE_NAME   =
*     IS_LAYOUT          =
      it_fieldcat        = gt_fieldcat " nombres de los atributos de cada uno de las columnas del reporte
*     IT_EXCLUDING       =
*     IT_SPECIAL_GROUPS  =
*     IT_SORT            =
*     IT_FILTER          =
*     IS_SEL_HIDE        =
*     I_DEFAULT          = 'X'
*     I_SAVE             = ' '
*     IS_VARIANT         =
*     IT_EVENTS          =
*     IT_EVENT_EXIT      =
*     IS_PRINT           =
*     IS_REPREP_ID       =
*     I_SCREEN_START_COLUMN          = 0
*     I_SCREEN_START_LINE            = 0
*     I_SCREEN_END_COLUMN            = 0
*     I_SCREEN_END_LINE  = 0
*     IR_SALV_LIST_ADAPTER           =
*     IT_EXCEPT_QINFO    =
*     I_SUPPRESS_EMPTY_DATA          = ABAP_FALSE
* IMPORTING
*     E_EXIT_CAUSED_BY_CALLER        =
*     ES_EXIT_CAUSED_BY_USER         =
    TABLES
      t_outtab           = gt_scustom
* EXCEPTIONS
*     PROGRAM_ERROR      = 1
*     OTHERS             = 2
    .
  IF sy-subrc <> 0.
* Implement suitable error handling here
  ENDIF.


ENDFORM.
```

### Modificando atributos de un reporte

* titulos

* Columnas

* textos de pie de pagina

* Transferir las modificaciones al modulo de funciones para mostrar el reporte ALV.
  
  * REUSE_ALV_LIST_DISPLAY
  
  * REUSE_ALV_GRID_DISPLAY
  
  * REUSE_ALV_HIERSEQ_LIST_DISPLAY

```abap

```
