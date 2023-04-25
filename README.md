# foxreports
Generador de reportes en VFP para uso general desde cualquier app

## OBJETIVO
La idea general es poder ofrecer la potencia, versatilidad y facilidad de uso del generador de reportes de VFP a cualquier otra aplicacion capaz de utilizar ActiveX o ejecutar programas externos.

### FUNCIONAMIENTO GENERAL
La solucion estaria compuesta de un EXE principal llamado FOXREPORTS.EXE el cual sera capaz de ejecutar distintas acciones segun los parametros pasados al invocarlo:

|Comando|Accion|
|-------|------|
|foxreports version|Mostrar la version actual de FoxReports|
|foxreports run reporte [parametros]|Ejecutar el reporte indicado en el parametro "reporte"|
|foxreports edit reporte|Crear/modificar un reporte con el nombre indicado en el parametro "reporte"|
|foxreports export reporte -pdf destino.pdf |Exportar el reporte "reporte" en formato PDF y almacenarlo en el archivo "destino.pdf"|
|foxreports expopr reporte -ds destino.dbf|Exportar la fuente de datos del reporte "reporte" en el archivo "destino.dbf"|
|foxreports config|Mostrar el dialogo de configuracion general de foxreports|
|foxreports set config value|Cambiar el valor de una configuracion especifica de foxreports|
|foxreports connection add id parametros|Define o actualiza una conexion de datos|


### COMPOSICION DE UN REPORTE
Todo reporte definido estaria compuesto de 3 archivos:

* reporte.frx
* reporte.frt
* reporte.cfg

Los archivos FRX y FRT almacenarian el formato del reporte, mientras que el archivo CFG almacenaria la configuracion del mismos. Este archivo CFG seria un archivo XML con la siguiente estructura:

    <?xml version="1.0">
    <report name="" dstype="" caption="">
        <datasource id="">
          <query></query>
        </datasource>
    </report
    
El parametro ''dstype'' determina el tipo de fuente de datos a utilizar:

|dstype|descripcion|
|------|-----------|
|sql|Query SQL sobre datos remotos|
|fox|Base de datos fox|

Por ejemplo:

    foxreports connection add mybd "c:\datos\datos.dbc"
    
    <?xml version="1.0">
    <report name="clientes" dstype="fox" caption="Clientes">
        <datasource id="mybd">
          <query>
            SELECT 0
            USE clientes ORDER BY xcodigo
          </query>
        </datasource>
    </report
    
    
    foxreports connection add mybdsql "Driver={SQL Server};server=10.0.0.1;uid=ruser;pwd=ruser;dbname=datos;"
    
    <?xml version="1.0">
    <report name="clientes" dstype="sql" caption="Clientes">
        <datasource id="mybdsql">
          <query>
            SELECT codigo,nombre,estatus
              FROM clientes
             ORDER BY codigo
          </query>
        </datasource>
    </report    
    
