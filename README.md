# Reporte de Inventario con UNION y UNION ALL - RetailChain
-- 1. ¿Cuántas filas devuelve cada consulta y por qué son distintas?
- La Consulta 1 (UNION) devuelve 11 filas.
- La Consulta 2 (UNION ALL) devuelve 14 filas.

Son distintas porque `UNION` eliminó 3 filas duplicadas correspondientes a los productos que comparten exactamente el mismo `id_producto`, `nombre_producto` y `categoria` 
en ambas sucursales: el Monitor 4K (103), el Teclado Mecánico (104) y el SSD Externo (106). 
La Webcam no se eliminó porque, aunque el nombre es el mismo, su `id_producto` es distinto (107 en el Norte y 111 en el Sur), 
por lo que para el motor de SQL son filas diferentes.

-- 2. ¿Por qué UNION ALL es más eficiente que UNION?
`UNION ALL` es más rápido y eficiente porque simplemente agarra los resultados de la segunda tabla y los "pega" debajo de los de la primera. 
Por el contrario, `UNION` consume más recursos (CPU y memoria) porque tiene que realizar una operación interna adicional: 
agrupar, ordenar y comparar todas las filas resultantes para encontrar y eliminar los duplicados antes de mostrar el resultado final.

-- 3. ¿En qué casos de negocio usarías cada uno?
- **UNION:** Lo usaría para unificar dos bases de datos de clientes (por ejemplo, si mi empresa compra a un competidor y fusionamos sistemas).
- Necesito una lista de correos electrónicos únicos para enviar una campaña de marketing;
- si uso UNION ALL, le enviaría el mismo correo dos veces a las personas que ya eran clientes de ambas empresas.
- **UNION ALL:** Lo usaría para juntar tablas históricas de ventas separadas por año (ej. `ventas_2022` UNION ALL `ventas_2023`).
- Como es imposible que una venta de 2022 esté duplicada en 2023, no tiene sentido gastar recursos buscando duplicados;
- solo necesito sumar todas las transacciones lo más rápido posible para hacer un balance histórico.

-- 4. ¿Qué pasa si las columnas de ambas consultas no coinciden en número o tipo?
Si intento hacer un UNION donde el primer SELECT pide 4 columnas y el segundo SELECT pide 3, SQL arrojará un **error de sintaxis**
(por ejemplo: *"The used SELECT statements have a different number of columns"*). Para que cualquier operador UNION funcione, 
ambas consultas deben tener exactamente la misma cantidad de columnas, en el mismo orden, y los tipos de datos de las columnas correspondientes deben ser compatibles.
