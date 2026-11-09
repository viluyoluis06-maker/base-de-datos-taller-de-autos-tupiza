PASO DEL DIAGRAMA EER A TABLAS

Proyecto: Sistema de Taller de Autos
Cada entidad fuerte se convierte en una tabla. Su identificador se convierte en clave primaria (PK), los atributos simples pasan a ser columnas y los atributos compuestos se separan en sus partes.
1. Tabla CLIENTE

cliente (CI 
      (PK),
    nombre,
    apellidos,
    calle,
    zona
)
"CLIENTE" es una entidad fuerte porque tiene su propio identificador CI, que permite reconocer a cada cliente.
- CI: es la clave primaria (PK)
- Nombre y apellidos: son atributos simples
- Calle y zona: son las partes del atributo compuesto dirección
Si un cliente tiene varios teléfonos, estos se manejarán en otra  tabla independiente.
2. Tabla AUTO

auto (
    placa (PK),
    marca,
    modelo,
    año,
    CI_cliente (FK) → cliente
)

AUTO es una entidad fuerte porque tiene su propio identificador placa
- Placa: es la clave primaria
- Marca, modelo y año: son atributos simples
- CI_cliente: es una clave foránea que conecta el automóvil con su propietario en CLIENTE
Un cliente puede tener varios automóviles, pero cada automóvil pertenece a un solo cliente

3. Tabla ORDEN_TRABAJO
orden_trabajo (
    nro_orden (PK),
    fecha_ingreso,
    fecha_salida,
    monto_total,
    placa (FK) → auto
)

ORDEN_TRABAJO es una entidad fuerte porque tiene su propio identificador nro_orden.
- Nro_orden: es la clave primaria
- Fecha_ingreso y fecha_salida: son atributos simples
- Monto_total: registra el costo de la orden
- Placa: es una clave foránea que conecta la orden con el automóvil
Un automóvil puede tener varias órdenes de trabajo, pero cada orden corresponde a un solo auto

4. Tabla MECANICO
mecanico (
    CI_mecanico (PK),
    nombre,
    especialidad,
    CI_jefe (FK) → mecanico
)


MECANICO es una entidad fuerte porque tiene su propio identificador CI_mecanico.
- CI_mecanico: es la clave primaria.
- Nombre y especialidad: son atributos simples.
- CI_jefe: es una clave foránea que se relaciona con la misma tabla MECANICO.
Esto representa la relación SUPERVISA, donde un mecánico puede supervisar a otros mecánicos.

5. Tabla REPUESTO
repuesto (
    cod_repuesto (PK),
    nombre,
    precio_unitario,
    stock
)

REPUESTO es una entidad fuerte porque tiene su propio identificador cod_repuesto.
- Cod_repuesto: es la clave primaria.
- Nombre: identifica el repuesto.
- Precio_unitario: registra su precio.
- Stock: indica cuántas unidades están disponibles.

Generalización y especialización se convierte en tablas de subtipos
La entidad AUTO tiene dos tipos de automóviles: PARTICULAR y FLOTA.

Se crea una tabla principal llamada AUTO y una tabla para cada subtipo.

6. Tabla PARTICULAR
particular (
    placa (PK, FK) → auto
)


PARTICULAR representa los automóviles de uso particular.

La placa es PK y FK al mismo tiempo porque identifica al automóvil y lo relaciona con la tabla principal AUTO.

7. Tabla FLOTA
flota (
    placa (PK, FK) → auto,
    nombre_empresa,
    RUC
)


FLOTA representa los automóviles que pertenecen a una empresa.
- Placa: es PK y FK.
- Nombre_empresa: registra el nombre de la empresa.
- RUC: identifica tributariamente a la empresa.
La placa permite relacionar cada vehículo de FLOTA con su registro correspondiente en AUTO.


Relaciones muchos a muchos (N:M)
utiliza las claves primarias de ambas entidades como claves foráneas y, juntas, forman una clave primaria compuesta.
8. Tabla ATIENDE
atiende (
    nro_orden (PK, FK) → orden_trabajo,
    CI_mecanico (PK, FK) → mecanico,
    horas_trabajo
)

ATIENDE representa la relación entre MECANICO y ORDEN_TRABAJO.
- Nro_orden: identifica la orden de trabajo.
- CI_mecanico: identifica al mecánico.
- Horas_trabajo: registra cuánto tiempo trabajó el mecánico en esa orden.
La PK es compuesta porque utiliza dos identificadores:

nro_orden + CI_mecanico

Esto permite registrar varios mecánicos en una misma orden y que un mecánico participe en diferentes órdenes.

9. Tabla UTILIZA
utiliza (
    nro_orden (PK, FK) → orden_trabajo,
    cod_repuesto (PK, FK) → repuesto,
    cantidad_usada
)

UTILIZA representa la relación entre ORDEN_TRABAJO y REPUESTO.
- Nro_orden: identifica la orden.
- Cod_repuesto: identifica el repuesto utilizado.
- Cantidad_usada: indica cuántas unidades se utilizaron.
Su PK también es compuesta:

nro_orden + cod_repuesto

Esto permite registrar varios repuestos en una misma orden y utilizar un mismo repuesto en diferentes órdenes.

Atributos multivaluado  todos en una tabla independiente
10. Tabla CLIENTE_TELEFONO
cliente_telefono (
    CI (PK, FK) → cliente,
    telefono (PK)
)


Esta tabla se crea porque un cliente puede tener varios números de teléfono.
- CI: es PK y FK porque identifica al cliente y lo conecta con CLIENTE.
- Telefono: permite registrar cada número.