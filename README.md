# NIfuNiFa
![](https://qiu.itq.edu.ec/Principal/imgLogin/ITQ.png)


# Proyecto Final
##Desarrollo en Software
####Asignatura: Administracion de Base de Datos
####Docente: Sebastian Landazuri
####Nivel: Segundo Nivel
###Integrantes: 
####Steven Tanquino 
####Andres Ortiz
####Danny Morales
<br>
####Tema: Base de Datos Para Una Biblioteca
![](https://st4.depositphotos.com/8499796/39354/v/450/depositphotos_393546848-stock-illustration-book-store-logo-book-store.jpg)
<center>Quito, 1 de Febrero del 2024
<br>
<br>




##**Tabla de Contenido**

<div class="texto-izquierda">
##Introduccion 
######Esta es una base de datos de una  biblioteca que desempeña un papel crucial en la organización, acceso y gestión eficiente de los recursos bibliográficos, así como en la administración de servicios para los amantes de la lectura y el buen desempeño que tendra la biblioteca para con el publico.

##Herramientas 
######Crear una base de datos implica varias etapas y requiere diferentes herramientas. ###Aquí hay un resumen de los pasos y herramientas esenciales:

####Sistema de Gestión de Bases de Datos (SGBD):

#####Elección del SGBD:
######Selecciona un SGBD adecuado para tus necesidades, como MySQL, PostgreSQL, Microsoft SQL Server, Oracle Database o SQLite.
###Herramienta de Administración del SGBD:

####Interfaz de Usuario o Cliente SQL:
######Utiliza herramientas gráficas o de línea de comandos como MySQL Workbench, pgAdmin, SSMS o Oracle SQL Developer para interactuar con el SGBD.
###Diseño de la Base de Datos:

####Modelado de Datos:
######Diseña la estructura de la base de datos utilizando herramientas como MySQL Workbench, ERWin o draw.io.
###Creación del Esquema de la Base de Datos:

####Script SQL o Herramienta de Creación de Esquemas:
######Define la estructura mediante un script SQL ejecutable en tu herramienta de administración SQL.
###Implementación del Esquema:

####Ejecución del Script SQL:
######Utiliza la herramienta de administración SQL para ejecutar el script y crear tablas, índices y otros elementos.
####Configuración y Mantenimiento:

####Configuración del SGBD:
###Ajusta las configuraciones según tus requisitos.

####Mantenimiento Continuo:
######Realiza tareas de mantenimiento como copias de seguridad, optimización de consultas y monitoreo del rendimiento.
###Lenguaje de Consultas:

####SQL (Structured Query Language): 
######Utiliza SQL para interactuar con la base de datos, realizando operaciones de consulta y manipulación.


<br>
<br>
<br>

##Consultas para el proyecto de gestión de biblioteca

####1. Recupera la información detallada de los libros prestados actualmente, incluyendo los datos de los usuarios y la fecha de devolución esperada.

SELECT　

	Select
    libros.lib_nombrelibro AS Titulo_Libro,
    usuarios.usu_nombre AS Nombre_Usuario,
    usuarios.usu_apellido AS Apellido_Usuario,
    usuarios_prestamos.usu_pre_id AS Prestamos_usuario,
    prestamos.pre_entrega AS Fecha_Prestamo,
    prestamos.pre_devolucion AS Fecha_Devolucion_Esperada
    FROM
    usuarios_prestamos
    JOIN
    usuarios ON usuarios.usu_id = libros.lib_usu_id

    WHERE
    prestamos.pre_devolucion IS NULL;



<div align = "center">
<img src = "">
</div>
####2. Encuentra el nombre y la cantidad total de libros de cada categoría disponibles en el inventario.

SELECT　
	
	SELECT
    categorias.cat_tipo_categoria AS Nombre_Categoria,
    COUNT(libros.lib_id) AS Cantidad_Total
	FROM
    libros
	JOIN
    categorias ON libros.lib_cat_id = categorias.cat_id
	WHERE
    libros.lib_id = '5';```
####3. Obtén la cantidad total de multas acumuladas por cada usuario, ordenadas de mayor a menor.

SELECT

	SELECT	
    usuarios.usu_nombre AS Nombre_Usuario,
    usuarios.usu_apellido AS Apellido_Usuario,
    COUNT(sanciones.san_id) AS Codigo_multas
	FROM
    usuarios
	LEFT JOIN
    sanciones ON usuarios.usu_id = sanciones.san_id
	GROUP BY
    usuarios.usu_id
	ORDER BY
    Codigo_multas DESC;

####4. Identifica los usuarios que tienen multas pendientes y han excedido el límite de préstamos permitidos.
SELECT

	SELECT		
    usuarios.usu_nombre AS Nombre_Usuario,
    usuarios.usu_apellido AS Apellido_Usuario
	FROM
    usuarios
	JOIN
    prestamos ON usuarios.usu_id = prestamos.pre_id
	LEFT JOIN
    sanciones ON sanciones.san_id = sanciones.san_fecha_real_devolucion;
	
####5. Encuentra los libros más populares basados en el número de préstamos, limitando los resultados a los 10 más solicitados.

	SELECT
    libros.lib_nombrelibro AS Titulo_Libro,
    COUNT(prestamos.pre_id) AS Numero_Prestamos
	FROM
    libros
	LEFT JOIN
    prestamos ON libros.lib_id = prestamos.pre_id
	GROUP BY
    libros.lib_id
	ORDER BY
    pre_id DESC
	LIMIT 10;
	

####6. Recupera la cantidad total de libros adquiridos de cada proveedor en el último año.

	SELECT
    editoriales.edi_nombre AS Nombre_Proveedor,
    COUNT(usuarios. ) AS Cantidad_Total_Libros
	FROM
  	 libros
	JOIN
    editoriales ON editoriales.edi_id = libros.lib_edi_id
	JOIN
    libros ON editoriales.lib_edi_id = libros.lib_nombrelibro
	WHERE
    editoriales.edi_fecha_publicacion >= CURDATE() - INTERVAL 1 YEAR;
	
   
####7. Encuentra la cantidad total de préstamos realizados por cada usuario en el último trimestre.

	SELECT
    usuarios.nombre AS Nombre_Usuario,
    usuarios.apellido AS Apellido_Usuario,
    COUNT(prestamos.id_prestamo) AS Cantidad_Total_Prestamos
	FROM
    usuarios
	JOIN
    prestamos ON usuarios.id_usuario = prestamos.id_usuario
	WHERE
    prestamos.fecha_prestamo >= CURDATE() - INTERVAL 3 MONTH
	GROUP BY
    usuarios.id_usuario;

####8. Identifica a los usuarios que han realizado préstamos y no han devuelto ningún libro hasta el momento.

	SELECT
    usuarios.nombre AS Nombre_Usuario,
    usuarios.apellido AS Apellido_Usuario
	FROM
    usuarios
	JOIN
    prestamos ON usuarios.id_usuario = prestamos.id_usuario
	WHERE
    prestamos.fecha_devolucion IS NULL;
	
####9. Recupera la cantidad promedio de días que los libros han estado en préstamo, agrupados por categoría.

	SELECT
    categorias.nombre AS Nombre_Categoria,
    AVG(DATEDIFF(prestamos.fecha_devolucion, prestamos.fecha_prestamo)) 
	AS 	Promedio_Dias_Prestamo
	FROM
    prestamos
	JOIN
    libros ON prestamos.id_libro = libros.id_libro
	JOIN
    categorias ON libros.id_categoria = categorias.id_categoria
	WHERE
    prestamos.fecha_devolucion IS NOT NULL
	GROUP BY
    categorias.id_categoria;
	
####10. Encuentra el autor con más libros en el inventario y la cantidad total de libros de ese autor.
	SELECT
    autores.nombre AS Nombre_Autor,
    COUNT(libros.id_libro) AS Cantidad_Total_Libros
	FROM
    autores
	JOIN
    libros ON autores.id_autor = libros.id_autor
	GROUP BY
    autores.id_autor
	ORDER BY
    Cantidad_Total_Libros DESC
	LIMIT 1;
	
####11. Recupera la cantidad total de libros por categoría que han sido prestados más de tres veces en el último mes.

	SELECT
    categorias.nombre AS Nombre_Categoria,
    COUNT(libros.id_libro) AS Cantidad_Total_Libros
	FROM
    prestamos
	JOIN
    libros ON prestamos.id_libro = libros.id_libro
	JOIN
    categorias ON libros.id_categoria = categorias.id_categoria
	WHERE
    prestamos.fecha_prestamo >= CURDATE() - INTERVAL 1 MONTH
	GROUP BY
    categorias.id_categoria
	HAVING
    COUNT(prestamos.id_prestamo) > 3;
	
####12. Identifica a los usuarios que han recibido reseñas con una puntuación promedio superior a 4.5.

	SELECT
    usuarios.nombre AS Nombre_Usuario,
    usuarios.apellido AS Apellido_Usuario
	FROM
    usuarios
	JOIN
    reseñas ON usuarios.id_usuario = reseñas.id_usuario
	GROUP BY
    usuarios.id_usuario
	HAVING
    AVG(reseñas.puntuacion) > 4.5;
	
####13. Encuentra el proveedor que ha suministrado la mayor cantidad de libros de cierta categoría.

	SELECT
    proveedores.nombre AS Nombre_Proveedor,
    COUNT(adquisiciones.id_libro) AS Cantidad_Total_Libros
	FROM
    proveedores
	JOIN
    adquisiciones ON proveedores.id_proveedor = adquisiciones.id_proveedor
	JOIN
    libros ON adquisiciones.id_libro = libros.id_libro
	JOIN
    categorias ON libros.id_categoria = categorias.id_categoria
	WHERE
    categorias.nombre = 'Nombre_Categoria' -- Reemplaza 'Nombre_Categoria' con 
	el 	nombre de la categoría específica
	GROUP BY
    proveedores.id_proveedor
ORDER BY
    Cantidad_Total_Libros DESC
LIMIT 1;
	
	
####14. Recupera la cantidad total de préstamos por día de la semana, ordenados por día.

SELECT
    DAYNAME(fecha_prestamo) AS Dia_Semana,
    COUNT(id_prestamo) AS Cantidad_Total_Prestamos
FROM
    prestamos
GROUP BY
    Dia_Semana
ORDER BY
    DAYOFWEEK(fecha_prestamo);
	
####15. Encuentra los libros que han estado en préstamo por más de 30 días y aún no han sido devueltos.

	SELECT
    libros.titulo AS Titulo_Libro,
    prestamos.fecha_prestamo AS Fecha_Prestamo,
    CURDATE() - prestamos.fecha_prestamo AS Dias_En_Prestamo
	FROM
    prestamos
	JOIN
    libros ON prestamos.id_libro = libros.id_libro
	WHERE
    prestamos.fecha_devolucion IS NULL
    AND CURDATE() - prestamos.fecha_prestamo > 30;
	
####16. Identifica a los usuarios que han realizado préstamos de libros raros, categorizados como 'Edición Especial'.
SELECT
    usuarios.nombre AS Nombre_Usuario,
    usuarios.apellido AS Apellido_Usuario,
    libros.titulo AS Titulo_Libro
FROM
    usuarios
JOIN
    prestamos ON usuarios.id_usuario = prestamos.id_usuario
JOIN
    libros ON prestamos.id_libro = libros.id_libro
WHERE
    libros.categoria = 'Edición Especial';
####17. Recupera la cantidad total de préstamos y la suma de multas para cada usuario en el último año.

	SELECT
    usuarios.nombre AS Nombre_Usuario,
    usuarios.apellido AS Apellido_Usuario,
    COUNT(prestamos.id_prestamo) AS Cantidad_Total_Prestamos,
    SUM(multas.monto) AS Suma_Multas
	FROM
    usuarios
	LEFT JOIN
    prestamos ON usuarios.id_usuario = prestamos.id_usuario
	LEFT JOIN
    multas ON usuarios.id_usuario = multas.id_usuario
	WHERE
    prestamos.fecha_prestamo >= CURDATE() - INTERVAL 1 YEAR
	GROUP BY
    usuarios.id_usuario;
	
	
####18. Encuentra el usuario que ha realizado el mayor número de préstamos y la cantidad total de libros prestados por él.

	SELECT
    usuarios.nombre AS Nombre_Usuario,
    usuarios.apellido AS Apellido_Usuario,
    COUNT(prestamos.id_prestamo) AS Cantidad_Prestamos,
    COUNT(DISTINCT prestamos.id_libro) AS Cantidad_Libros_Prestados
	FROM
    usuarios
	JOIN
    prestamos ON usuarios.id_usuario = prestamos.id_usuario
	GROUP BY
    usuarios.id_usuario
	ORDER BY
    Cantidad_Prestamos DESC
	LIMIT 1;
	
	
####19. Identifica los libros que están disponibles en el inventario y no han sido prestados en los últimos seis meses.

	SELECT
    libros.titulo AS Titulo_Libro,
    libros.id_libro AS ID_Libro
	FROM
    libros
	LEFT JOIN
    prestamos ON libros.id_libro = prestamos.id_libro
	WHERE
    prestamos.fecha_prestamo IS NULL OR prestamos.fecha_prestamo 
	< 	CURDATE()INTERVAL 6 MONTH;
####20. Recupera la cantidad total de préstamos y la suma de multas por cada usuario, pero solo para aquellos con más de 5 préstamos.

	SELECT
    usuarios.nombre AS Nombre_Usuario,
    usuarios.apellido AS Apellido_Usuario,
    COUNT(prestamos.id_prestamo) AS Cantidad_Total_Prestamos,
    SUM(multas.monto) AS Suma_Multas
	FROM
    usuarios
	JOIN
    prestamos ON usuarios.id_usuario = prestamos.id_usuario
	LEFT JOIN
    multas ON usuarios.id_usuario = multas.id_usuario
	GROUP BY
    usuarios.id_usuario
	HAVING
    Cantidad_Total_Prestamos > 5;
	
####21. Encuentra los libros con la puntuación promedio más alta según las reseñas de los usuarios.

	 SELECT
    libros.titulo AS Titulo_Libro,
    AVG(reseñas.puntuacion) AS Puntuacion_Promedio
	FROM
    libros
	LEFT JOIN
    reseñas ON libros.id_libro = reseñas.id_libro
	GROUP BY
    libros.id_libro
	ORDER BY
    Puntuacion_Promedio DESC
	LIMIT 10;
####22. Identifica a los usuarios que han acumulado multas por un monto total superior a $50.

	SELECT
    usuarios.nombre AS Nombre_Usuario,
    usuarios.apellido AS Apellido_Usuario,
    SUM(multas.monto) AS Monto_Total_Multas
	FROM
    usuarios
	LEFT JOIN
    multas ON usuarios.id_usuario = multas.id_usuario
	GROUP BY
    usuarios.id_usuario
	HAVING
    Monto_Total_Multas > 50;
	
####23. Recupera la cantidad total de préstamos por mes, agrupados por año.

	SELECT
    YEAR(fecha_prestamo) AS Anio,
    MONTH(fecha_prestamo) AS Mes,
    COUNT(id_prestamo) AS Cantidad_Total_Prestamos
	FROM
    prestamos
	GROUP BY
    Anio, Mes
	ORDER BY
    Anio, Mes;
####24. Encuentra la cantidad total de libros prestados por cada usuario en el último mes, excluyendo aquellos que tienen multas pendientes.

	SELECT
    usuarios.nombre AS Nombre_Usuario,
    usuarios.apellido AS Apellido_Usuario,
    COUNT(DISTINCT prestamos.id_libro) AS Cantidad_Libros_Prestados
	FROM
    usuarios
	JOIN
    prestamos ON usuarios.id_usuario = prestamos.id_usuario
	LEFT JOIN
    multas ON usuarios.id_usuario = multas.id_usuario
	WHERE
    prestamos.fecha_prestamo >= CURDATE() - INTERVAL 1 MONTH
    AND multas.id_multa IS NULL
	GROUP BY
    usuarios.id_usuario;
####25. Identifica a los usuarios que han devuelto libros con retraso en más de tres ocasiones en el último año.
	SELECT
    usuarios.nombre AS Nombre_Usuario,
    usuarios.apellido AS Apellido_Usuario,
    COUNT(DISTINCT multas.id_multa) AS Cantidad_Retrasos
	FROM
    usuarios
	JOIN
    multas ON usuarios.id_usuario = multas.id_usuario
	JOIN
    prestamos ON usuarios.id_usuario = prestamos.id_usuario
	WHERE
    multas.fecha_multa >= CURDATE() - INTERVAL 1 YEAR
    AND prestamos.fecha_devolucion 
	< prestamos.fecha_devolucion_real
	GROUP BY
    usuarios.id_usuario
HAVING
    Cantidad_Retrasos > 3;


<BR>
<br>

###End
