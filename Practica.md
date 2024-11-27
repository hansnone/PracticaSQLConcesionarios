# Actividad concesionarios.

Crear la bbdd y usarla:

```sql
create database concesionarios;
use concesionarios;
```

Crear las tablas *tiendas* y *vehiculos*.

```sql
create table tiendas (
    id int not null,
    nombre varchar(50) not null,
    ciudad varchar(50) not null,
    numerotrabajadores int not null,
    superficie int not null,
    primary key (id)
);
create table vehiculos (
    matricula varchar(50) not null,
    marca varchar(50) not null,
    modelo varchar(50) not null,
    color varchar(50) not null,
    cilindrada int not null,
    antiguedad year not null,
    kilometros int not null,
    precio int not null,
    idtienda int not null,
    foreign key(idtienda) references tiendas(id)
);
```

Insertar valores en las tablas *vehiculos* y *tiendas*.

```sql
insert into tiendas (
        id,
        nombre,
        ciudad,
        numerotrabajadores,
        superficie
)
values 
    (1, 'auto 2', 'madrid', 5, 1250),
    (2, 'multimarcatotal', 'madrid', 5, 1750),
    (3, 'carauto', 'madrid', 10, 2000),
    (4, 'turismos diaz', 'barcelona', 5, 1000),
    (5, 'barna car', 'barcelona', 15, 3000);

insert into vehiculos (
    matricula,
    marca,
    modelo,
    color,
    cilindrada,
    antiguedad,
    kilometros,
    precio,
    idtienda
)
values 
    ('1213-CRX', 'Seat', 'León', 'Negro', 1.8, 2004, 180000, 2000, 1),
    ('3243-HTN', 'Seat', 'Altea', 'Rojo', 1.2, 2013, 85000, 7900, 2),
    ('6643-KBM', 'Seat', 'Ibiza', 'Blanco', 1.0, 2017, 25000, 10900, 2),
    ('8265-HZL', 'Nissan', 'Juke', 'Blanco', 1.6, 2014, 110000, 13900, 3),
    ('8919-HHH', 'Nissan', 'Qashqai', 'Gris', 2.0, 2011, 200000, 8500, 4),
    ('7623-GRS', 'Volkswagen', 'Tiguan', 'Gris', 2.0, 2010, 130000, 12000, 1),
    ('4901-KPS', 'Volkswagen', 'Polo', 'Azul', 1.0, 2018, 10000, 11500, 2),
    ('6841-LBN', 'Volkswagen', 'Golf', 'Rojo', 1.6, 2019, 15000, 22500, 5);
```

## Realiza las siguientes consultas:

1. Consulta que muestre todas las tiendas de más de 1500 metros, ordenadas por el nombre de la tienda.

```sql
select * from tiendas
where superficie >= 1500 order by nombre;
```

2. Consulta que muestre la marca y el modelo de los vehículos que sean blancos su antigüedad sea inferior a 2012.

```sql
select * from vehiculos
where color = 'blanco' and antiguedad <= 2012;
```

3. Consulta que muestre el nombre de la tienda, la marca, el modelo y el precio del vehículo.

```sql
select * from vehiculos;
```

4. ¿Cuántos vehículos tenemos de cada marca?

```sql
select count(marca) as cantidad, marca from vehiculos
group by marca order by marca;
```

5. ¿Cuál es el importe total de los vehículos de cada tienda?

```sql
select id, nombre, sum(precio) from vehiculos
inner join tiendas on idtienda = id
group by idtienda;
```

6. Mostrar la marca, modelo, km y la tienda de cada vehículo. Si un vehículo no está en ninguna tienda también debe salir.

```sql
select marca, modelo, kilometros from vehiculos;
```

7. Mostrar la media de km de los vehículos de la marca Seat.

```sql
select avg(precio) from vehiculos
where marca = 'seat';
```

8. ¿Cuál es la media de km y de precio de los vehículos con una antigüedad inferior a 2015?

```sql
select antiguedad, avg(precio), avg(kilometros) from vehiculos
where antiguedad < 2015
group by antiguedad;
```

9. Mostrar la tienda y la suma de km de sus vehículos, solo de aquellas tiendas que la suma de km de sus vehículos es superior a 150000.

```sql
select nombre, SUM(kilometros)
from vehiculos
inner join tiendas on idtienda = id
group by nombre
having sum(kilometros) >= 150000;
```

10. Mostrar la marca y el modelo de los vehículos que no están en ninguna tienda.

```sql
select marca, modelo, idtienda
from vehiculos
where idtienda = null;
```

11. Mostrar la marca, el modelo, el precio y una nueva columna con un 10% sobre el precio a la que llamaremos descuento de los vehículos con más de 100000 km y un precio menor 10000€.

```sql
select marca, modelo, precio, precio * 0.10 as descuento
from vehiculos
where kilometros > 100000 and precio < 10000;
```

12. Marca y modelo del vehículo de mayor antigüedad.

```sql
select marca, modelo
from vehiculos
where antiguedad = (select min(antiguedad) from vehiculos);
```

13. Marca y modelo de los vehículos que tienen un importe superior al vehículo de la marca Seat más caro.

```sql
select marca, modelo
from vehiculos
where precio > (select max(precio) from vehiculos where marca = 'Seat');
```

14. Qué antigüedad tiene el vehículo con más km que no es de color blanco ni de la marca Wolkswagen.

```sql
select antiguedad
from vehiculos
where color != 'Blanco' and marca != 'Volkswagen' and kilometros = (select max(kilometros) from vehiculos where color != 'Blanco' and marca != 'Volkswagen');
```
