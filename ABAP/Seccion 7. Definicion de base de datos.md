# Definicion de base de datos

CAda programa compuesto por uno o mas sentencias

la primer sentencia es la palabra clave

Una sentencia puede incluir una o mas adicionales, siempre terminan con un punto

## Definicion de objetos

ubicaciones de memoria que se usa para almacenar datos mientras el programa se está ejectando

* Modificables

* No modificables

### niveles de visibilidad

* **localmente visibles**: desde el interior de la subrutina

* **globalmente visibles**: desde cualquier lugar dentro del programa

* **visibles externamente:** desde fuera del programa

### Definicion de literales

son cadenas de caracteres que no son modificables, se encierran entre comillas simples ''

'Hola mundo' 

### Literales numericos

no es neceasaria las comillas a menos que sea decimal

ejemplos: 

5 

'24.5'

### tipo de dato caracter

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-27-21-53-33-image.png)

### Tipo de dato numerico

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-27-21-56-43-image.png)

## Definicion de variables

DATA

DATA v1 [(I)] [type t ] [decimals d] [value 'xxx']

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA f1(2) type c. " variable de tipo caadena con logitud 2
DATA f2 LIKE f1. " variable que se crea a partir de otra existente
DATA max_value TYPE i VALUE 100. ". entero con vaor por defecto 100
DATA cur_date TYPE d VALUE '20220727'. " variable tipo date con valor por defecto
```

los nombres de las variables debe contener al menos un caracter alfabetico

SAP recomienden que siempre comiencen con un caracter y que no tengan un guin 

USING o CHANGING no causan alerta, pero pueden causar conflicto en la ejecucion

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA f1(2) type c. " variable de tipo caadena con logitud 2
DATA f2 LIKE f1. " variable que se crea a partir de otra existente
DATA max_value TYPE i VALUE 100. ". entero con vaor por defecto 100
DATA cur_date TYPE d VALUE '20220727'. " variable tipo date con valor por defecto


DATA f3(4) VALUE 'Hola'.  " definicion de variable sin especificar el tipo, por defecto es de tipo cadena"
"WRITE f3, f4. " La variable f2 no está definida aun, dará un error en tiempo de ejecucion
DATA f4(5) VALUE 'there'.  " definicion de variable sin especificar el tipo, por defecto es de tipo cadena"
WRITE: f3,f4.
```

### Parametros

Muy parecido a DATA, pero SAP pide estos datos en una pantalla antes de iniciar con la ejecucion del programa

longitud maxima de 8 caracteres en los nombres

las variables pueden utilizara para metros como valores por iniciales

PARAMETERS p1 [(I)] [type t] [decimals d]

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

PARAMETERS p1(2) TYPE c.
PARAMETERS p2 LIKE p1.
PARAMETERS mx_v TYPE i DEFAULT 100.
PARAMETERS cur_date TYPE d DEFAULT '20220727' OBLIGATORY. " Es obligatorio
PARAMETERS cur_dt LIKE sy-datum DEFAULT sy-datum OBLIGATORY.


PARAMETERS: p3(15) TYPE c,
  p4 LIKE p1 OBLIGATORY LOWER CASE,
  p5 LIKE sy-datum DEFAULT sy-datum,
  cb1 AS CHECKBOX,
  cb2 AS CHECKBOX,
  rb1 RADIOBUTTON GROUP g1 DEFAULT 'X',
  rb2 RADIOBUTTON GROUP g1,
  rb3 RADIOBUTTON GROUP g1.

WRITE: / 'Salida del programa',
  /' p3 = ', p3,
  /' p4 = ', p4,
  /' cb1 = ', cb1,
  /' cb2 = ', cb2,
  /' rb1 = ', rb1,
  /' rb2 = ', rb2,
  /' rb3 = ', rb3.
```

### Etiquetas en los parametros de entrada.

Para asignar un texto custom a los parametros de entrada en la pantalla

**pasar a** --> **Elementos de text**--> **Textos de selección**

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-27-23-09-56-image.png)

Despues de este cambio s debe volver a activar el programa

### Constantes

su valor no se puede cambiar, siempre permanece con el mismo valor.

constants c1 [(I)] [type t] [decimals d] value 'xxx'

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

CONSTANTS c1(2) TYPE c VALUE 'AA'.
CONSTANTS c2 LIKE c1 VALUE 'BB'.
CONSTANTS error_de_constante TYPE i VALUE 5.
CONSTANTS nuevamente_error LIKE sy-datum VALUE '20180419'.
```

## Definicion de campo tipo registro

es un tipo de varibale y es el equivalente de una estructura en el DDIC, pero definido dentro de un programa.

Es una serie de campos agrupados bajo un nombre comun.

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.


DATA: BEGIN OF names,
  name1 LIKE scustom-name,
  name2 LIKE scustom-name,
  END OF names.
DATA: BEGIN OF cust_info,
  number(10) TYPE n,
  nm LIKE names,
  END OF cust_info.

" asignacion de variables
cust_info-number = 15.
cust_info-nm-name1 = 'Juan'.
cust_info-nm-name2 = 'Eduardo'.

WRITE: / cust_info-number,
  cust_info-nm-name1,
  cust_info-nm-name2.
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


DATA: my_tcus LIKE scustom, " referencia a la tabla estandar
      my_scus LIKE zcusto. " Estructura en el DDIC

" Asignacion de valores

my_tcus-name = 'Jose Soto'.
my_tcus-telephone = '362-243-2746'.
my_tcus-region = 'ZU'.

WRITE: / my_tcus-name,
  my_tcus-telephone,
  my_tcus-region.


 
```





### Agregar campos extras a estructuras existentes dentro de un programa



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.


DATA BEGIN OF fs1.
  INCLUDE STRUCTURE scustom. " scustom es una estructura existente
  DATA: extra_field(3) TYPE c, " extra_field es un campo que no existe en la estructur original, pero que servira dentro de este programa
END OF fs1.

" Asignacion de valores

fs1-id = 23.
fs1-extra_field = 'xyz'. " usando el campo extra.

WRITE: / fs1-id,
  fs1-extra_field.


```





### Diferencias entre LIKE e INCLUDE



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF fs1,
  my_scustom LIKE scustom, " se incluye una estructura existente pero haciendo uso de LIKE
  extra_field(3) TYPE c, " Campo extra que no existe en la estructura original
END OF fs1.


" Asignacion de valores

fs1-my_scustom-id = 12. " Cuando se utiliza LIKE se agrega una extension entre la variable para acceder a campos de la estructura original
fs1-extra_field = 'xyz'.

WRITE: / fs1-my_scustom-id,
  fs1-extra_field.



```



### Registro como variables multiples y como una variable de tipo char



lo veo mas o menos como un array.



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF fs1,
  c1 VALUE 'A',
  c2 VALUE 'B',
  c3 VALUE 'C',
END OF fs1.


WRITE: / fs1-c1, fs1-c2, fs1-c3,
  / fs1.


fs1 = 'XYZ'.

WRITE: / fs1-c1, fs1-c2, fs1-c3,
  / fs1.


```



### Asignacion que involucra dos registros.

```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

DATA: BEGIN OF fs1,
  c1 VALUE 'A',
  c2 VALUE 'B',
  c3 VALUE 'C',
END OF fs1,
fs2 LIKE fs1.

fs2 = fs1.

WRITE: / fs1-c1, fs1-c2, fs1-c3.


```





## TABLES para definir un registro



Objeto de datos modificables. Sintaxis:

TABLES fs1.



Una tabla o estructura del mismo nombre debe existir en el diccionario de datos.

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

scustom-name = 'SAP AG' .
scustom-country = 'DE'.

WRITE: / scustom-name, scustom-country.


```



Tambien permite acceder a la informacion de una tabla



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

SELECT * FROM scustom INTO scustom ORDER BY id.
  WRITE / scustom-id.
ENDSELECT.


```



NOTA: un campo de tipo registro definida mediante TABLES siempre tiene visibilidad global y externa.





### Definiendo tipos de datos.

TYPES



TYPES t1 [(I] [type t] [decimals d].



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.


TYPES efrain(2) TYPE c.
TYPES char21 TYPE c LENGTH 2.

DATA: v1 TYPE efrain VALUE 'AB',
  v2 TYPE efrain VALUE  'CD' .

WRITE: v1,v2.



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

TYPES: dollars(16) TYPE p DECIMALS 2,
       lira(16) TYPE p DECIMALS 0. "

DATA: BEGIN OF american_sums,
  petty_cash TYPE dollars,
  pay_outs TYPE dollars,
  lump_sums TYPE dollars,
END OF american_sums,

BEGIN OF italian_sums,
  petty_cash TYPE lira,
  pay_outs TYPE lira,
  lump_sums TYPE lira,
END OF italian_sums.

american_sums-pay_outs = '9500.03'.
italian_sums-lump_sums = 5141.

WRITE: / american_sums-pay_outs,
/ italian_sums-lump_sums.



```



### Uso del tipo estructurado.



Pueden reducir la Redundancia y facilitar el mantenimiento.



```abap
*&---------------------------------------------------------------------*
*& Report  ZTX010208_1_EFRAIN
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZTX010208_1_EFRAIN.

" se declara un tipo de dato (address, que a su vez está compuesto por diferentes campo)
TYPES: BEGIN OF address,
  street(25),
  city(20),
  region(7),
  country(15),
  postal_code(9),
  END OF address.


" se crean variables del tipo address (que previamente se generó).
DATA: customer_addr TYPE address,
vendor_addr TYPE address,
employee_addr TYPE address,
shipto_addr TYPE address.

" a customer se le asigna la calle y a al empleado se le asigna el pais.
customer_addr-street = '101 Memory Lane'.
employee_addr-country = 'Transylvania'.


" se imprimen las variables asignadas.
WRITE: / customer_addr-street,
         employee_addr-country.



```



### Grupos de tipo (type-pool)

objeto de diccionario de datos que existe simplemente para contener uno mas tipos de constantes.  Se pueden compartir entre diferentes progrmas



Se crean desde TRX **SE11**.

* Seleccionar "Grupo tipos"

* capturar el nombre del nuevo grupo.

* dar clic en Crear.

* Escribir la descripcion breve

* Grabar

* Objeto local

* En el editor que se abre capturar el tipo



```abap
TYPE-POOL zgt1 .

TYPES: zgt1_dollars(16) TYPE p DECIMALS 2,
       zgt1_lira(16) TYPE p DECIMALS 0.

CONSTANTS: zgt1_gc_warning_threshold TYPE i VALUE 5000,
           zgt1_gc_amalgamation_date LIKE sy-datum VALUE '19970305'.
```



* Verificar la sintaxis y activar

* Ahora puede ocupar este pool en alguno de sus progrmas



```abap

```

















 
