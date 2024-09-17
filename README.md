-- 1 Consulta para listar todas las antigüedades disponibles para la venta:

-- "Obtén una lista de todas las piezas antiguas que están actualmente disponibles para la
-- venta, incluyendo el nombre de la pieza, su categoría, precio y estado de conservación."

```sql
SELECT 
    a.Descripcion AS Nombre_Pieza,
    c.Nombre_Categoria AS Categoria,
    a.Precio,
    a.Condicion AS Estado_Conservacion
FROM 
    Antiguedad a
JOIN 
    Categoria c ON a.Categoria_id = c.Categoria_id
WHERE 
    a.Estado = 'en venta';
```sql


-- 2 Consulta para buscar antigüedades por categoría y rango de precio:
-- "Busca todas las antigüedades dentro de una categoría específica (por ejemplo, 'Muebles')
-- que tengan un precio dentro de un rango determinado (por ejemplo, entre 500 y 2000
-- dólares)."

```sql

SELECT 
    a.Descripcion AS Nombre_Pieza,
    c.Nombre_Categoria AS Categoria,
    a.Precio,
    a.Condicion AS Estado_Conservacion
FROM 
    Antiguedad a
JOIN 
    Categoria c ON a.Categoria_id = c.Categoria_id
WHERE 
    c.Nombre_Categoria = 'Muebles' 
    AND a.Precio BETWEEN 750.00 AND 1200.00
    AND a.Estado = 'en venta';
```sql
    
    
-- 3 Consulta para mostrar el historial de ventas de un cliente específico:
-- "Muestra todas las piezas antiguas que un cliente específico ha vendido, incluyendo la fecha
-- de la venta, el precio de venta y el comprador."

```sql

SELECT 
    a.Descripcion AS Nombre_Pieza,
    t.Fecha_transaccion AS Fecha_Venta,
    t.Cantidad_Pagada AS Precio_Venta,
    u.Nombre AS Comprador
FROM 
    Transaccion t
JOIN 
    Antiguedad a ON t.Antiguedad_id = a.ANTIGUEDAD_ID
JOIN 
    Usuario u ON t.Comprador_id = u.usuario_id
WHERE 
    a.Vendedor_id = 1
    AND t.Estado_pago = 'completado';
```sql
    
    
-- 4 Consulta para obtener el total de ventas realizadas en un periodo de tiempo:
-- "Calcula el total de ventas realizadas en un período específico, por ejemplo, durante el último
-- mes."

```sql

SELECT 
    SUM(t.Cantidad_Pagada) AS Total_Ventas
FROM 
    Transaccion t
WHERE 
    t.Fecha_transaccion >= DATE_SUB(CURDATE(), INTERVAL 1 MONTH)
    AND t.Estado_pago = 'completado';
```sql

    
-- 5 Consulta para encontrar los clientes más activos (con más compras realizadas):
-- "Identifica los clientes que han realizado la mayor cantidad de compras en la plataforma."

```sql

SELECT 
    u.Nombre AS Cliente,
    COUNT(t.Transaccion_id) AS Cantidad_Compras
FROM 
    Transaccion t
JOIN 
    Usuario u ON t.Comprador_id = u.usuario_id
WHERE 
    t.Estado_pago = 'completado'
GROUP BY 
    u.usuario_id, u.Nombre
ORDER BY 
    Cantidad_Compras DESC;
```sql
    
-- 7 Consulta para listar las antigüedades vendidas en un rango de fechas específico:
-- "Obtén una lista de todas las piezas antiguas que se han vendido dentro de un rango de
-- fechas específico, incluyendo la información del vendedor y comprador."

```sql

SELECT 
    a.Descripcion AS Nombre_Pieza,
    t.Fecha_transaccion AS Fecha_Venta,
    t.Cantidad_Pagada AS Precio_Venta,
    v.Nombre AS Vendedor,
    c.Nombre AS Comprador
FROM 
    Transaccion t
JOIN 
    Antiguedad a ON t.Antiguedad_id = a.ANTIGUEDAD_ID
JOIN 
    Usuario v ON a.Vendedor_id = v.usuario_id
JOIN 
    Usuario c ON t.Comprador_id = c.usuario_id
WHERE 
    t.Fecha_transaccion BETWEEN '2024-01-01' AND '2024-12-31' -- Reemplaza con el rango de fechas deseado
    AND t.Estado_pago = 'completado';
```sql
    
-- 8 Consulta para listar las antigüedades vendidas en un rango de fechas específico:
-- "Obtén una lista de todas las piezas antiguas que se han vendido dentro de un rango de
-- fechas específico, incluyendo la información del vendedor y comprador."

```sql

SELECT 
    c.Nombre_Categoria AS Categoria,
    COUNT(a.ANTIGUEDAD_ID) AS Cantidad_Articulos
FROM 
    Antiguedad a
JOIN 
    Categoria c ON a.Categoria_id = c.Categoria_id
WHERE 
    a.Estado = 'en venta'
GROUP BY 
    c.Categoria_id, c.Nombre_Categoria
ORDER BY 
    Cantidad_Articulos DESC;
```sql
