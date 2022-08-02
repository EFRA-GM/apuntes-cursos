# Seccion 4 Primer programa en ABAP

1. Use la transaccion **SE38** para ingresar al edito ABAP (**Development Workbench**)

Datos que se solicitan

**Program:** Ingresar el nombre del progrma, 

* Los programas clientes deben comenzar con Y o Z

**Objetos parciales:** podemos elegir si vamos a editar, el codigo fuente, variantes, documentacion. Seleccionar codigo fuente pra definir la fucionalidad.

Opciones

**Crear:** crea un nuevo programa

**Visualizar:** sirve solo para ver codigo y otras caracteristicas, solo lectura

**Modificar:** sirve para editar un programa ya existente.

Para crear un programa, posteriormente se solicitan los atributos del programa

**Titulo:** un nombre descriptivo

**Tipo:** Programa ejecutable

**estatus:** programa de test

* Si es un programa para productivo se puede seleccionar T

Aplicacion: Multiaplicaciones.

Dar clic en **save**

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-26-13-23-06-image.png)

## Opciones de transporte

opciones para indicar si nuestro programa se transportará a otros ambientes

Para pruebas seleccionar "**Local object**"

## Editor ABAP

Se trata de un editor de codigo integrado en el sistema SAP con operaciones basicas para codificar, compilar y probar nuestros programas.

Ejemplo de codigo:

```abap
*&---------------------------------------------------------------------*
*& Report  ZAB_CU_P1018004
*&
*&---------------------------------------------------------------------*
*&
*&
*&---------------------------------------------------------------------*

REPORT ZAB_CU_P1018004.
WRITE 'Hola mundo'.
WRITE '----------------'.
```

* Cada linea debe terminar con un punto.

**Grabar** programa con icono de guardar.

**Verificar** para detectar error de syntaxis

**Activar** el programa. Lo hace disponible para ejecucion en el ambiente que se esta trabajando.

**Directo (Direct processing):** para probar el programa

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-26-13-33-33-image.png)

Tambien puede ejecutar el programa sin ingresar al codigo desde la pantalla principal de la transaccion **SE38**

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-26-13-37-22-image.png)

## Tipos de programa

* **Reportes**: Solo lee informacion y la pone en un contenedor de salida
  
  * Selection screen: se solicitan los parametros para el reporte
  
  * Pantalla de salida

* **Dialogo**: Pueden ser mas complejos, con multiples pantallas que pueden seguir diferentes flujos

Puede ver el codigo fuente de cualquier progrma ABAP. Para ellos, estando dentro de la transaccion, ir a **Sistema** --> **Status...** --> **Programa(dynpro)**

**Dynpros:** pantallas que contiene el programa

## Componentes de un reporte

* **codigo fuente**

* **Atributos**: se pueden definir valores defaulta para parametros en un reporte

* **Elementos de texto**

* **Documentación**

* **Variantes**

## Programas ejecutables

No son programas compilados, son interpetados, se genera un objeto ejecutable pero que es necesario de una instancia SAP para ejecutarlo

## Explorando editor ABAP

* Busqueda

Transaccion **SE80**: Editor de desarrollo integrado con otras herramientas para tratar diferentes tipos de bjetos

Muestra mas detalles y una agrupacion ordenada de los objetos

* Editar

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-26-14-28-26-image.png)

Tambien cuenta con las opciones de verificar, activar y ejecutar.

## Ayuda

* F1: Contiene sintaxis y ejemplos de codigo. Posicionarse sobre la palabra clave y presionar F1

## Encntrar objetos desarrollados

1. Ingresar a la transaccion **SE80**

2. Todos los objetos estan organizados en paquetes

3. Para objetos locales seleccione $TMP

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-26-14-42-55-image.png)

Puede utilizar el comodin Asterisco para encotrar objetos de los que solo recuerda un parte del nombre del programa en la transaccion **SE38**. despues de ecribir presione **F4**

Ejemplo ZAB_*

## Diccionario de dato (DDIC)

Transaccion **SE11**

Utilidad para la definicion de objetos de datos.

Puede crear y almacenar objetos como tablas, estructuras y vistas

el se encarga de generar las sentencias SQL  para generar la tabla real en la base de datos.

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-26-14-52-50-image.png)

**Display** para ve la estructura de la tabla

Puede ver el contenido de la tabla, ingresando parametros en algunos de los campos 

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-26-14-56-27-image.png)

La transacccion SE16N tambien sirve para ver estructura de tablas

### Estructuras

Estructtura de grupo de campos. Se describe nombres de campos, secuencias, y tipos de datos con logitudes

Las estructuras no almacenan datos

# Recuperar datos de la base de datos

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

SELECT * FROM scustom
INTO scustom ORDER BY id.
WRITE / scustom-id.
ENDSELECT.
```

### WorkArea

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

DATA gs_scustom LIKE scustom.

SELECT * FROM scustom
INTO gs_scustom ORDER BY id.
WRITE / scustom-id.

ENDSELECT.
```

### WHERE

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

DATA gs_scustom LIKE scustom.

SELECT * FROM scustom
INTO gs_scustom
WHERE id < '00000040'
ORDER BY id.
WRITE / gs_scustom-id.

ENDSELECT.
```

## Variables del sistema

Todas las variables de sistema inician con SYST. Campos SY

Estan definidos en la estructura **SYST**.

se actualizan automaticamente y no tiene que definirlas en el programa

**SY-DATUM**: Fecha actual del sistema

**SY-SUBRC**: desepues de un ENDSELECT. si se han encontrado filas el valor es 0, de lo contrario el valor será 4

**SY-DBCNT**: Cuenta el numero de iteraciones. util para enumerar las filas de un SELECT y al final obtener el total de filas.

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

DATA gs_scustom LIKE scustom.

SELECT * FROM scustom
INTO gs_scustom
WHERE id < '0000004'
ORDER BY id.
WRITE / sy-dbcnt.
WRITE gs_scustom-id.

ENDSELECT.
WRITE / sy-dbcnt.
IF SY-SUBRC NE 0.
  WRITE / 'No se encontraron registros'.
ENDIF.
```

Puede ver mas detalles de la variable dando doble clic en la variable en un programa o desde la transacccion SE11

![](C:\Users\garci\AppData\Roaming\marktext\images\2022-07-27-08-25-02-image.png)

## Operador cadena

Puede utilizar la forma normal

```abap
TABLES ztxxlfa1.
TABLES ztxxlfb1.
```

o la opcion con operador cadena para simplificar en una sola linea

```abap
TABLES: ztxxlfa1, ztxxlfb1.
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

TABLES scustom.

DATA gs_scustom LIKE scustom.

SELECT * FROM scustom
INTO gs_scustom
WHERE id < '0000040'
ORDER BY id.
WRITE: / sy-dbcnt, gs_scustom-id, gs_scustom-name.

ENDSELECT.
WRITE: / sy-dbcnt, 'Numero de registros o filas encontrados'.
IF SY-SUBRC NE 0.
  WRITE / 'No se encontraron registros'.
ENDIF.
```

## SELECT SINGLE

Obtiene un solo registro de la consulta cuando conoce el identificador

No termina con ENDSELECT

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

DATA gs_scustom LIKE scustom.

SELECT SINGLE * FROM scustom " este es un comentario al final de una linea
INTO gs_scustom
WHERE id EQ '0000040'.


IF SY-SUBRC EQ 0.
  WRITE: / sy-dbcnt, gs_scustom-id, gs_scustom-name.
  ELSE.
    WRITE / 'No se encontraron registros'.
ENDIF.
```
