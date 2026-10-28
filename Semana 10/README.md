# Comercial - Semana 10

Proyecto de práctica Django: aplicación `catalogo`, listado de categorías con
datos del modelo y modelos para categorías, tipos de pago, clientes y roles.

## Preparar el entorno

Desde esta carpeta, en PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

La configuración predeterminada usa SQLite para que el proyecto funcione sin
un servidor de base de datos. Para usar MariaDB como en la guía, cree la base
de datos `comercial` y configure estas variables antes de ejecutar Django:

```powershell
$env:DB_ENGINE = "mysql"
$env:DB_NAME = "comercial"
$env:DB_USER = "root"
$env:DB_PASSWORD = "su contraseña"
$env:DB_HOST = "127.0.0.1"
$env:DB_PORT = "3307"
```

`mysqlclient` es el controlador de MySQL/MariaDB requerido para ese modo. No se
incluyen credenciales reales en el proyecto.
La guía indica Django 6.1 y sus capturas muestran Django 6.1.1. Esta versión
requiere Python 3.12 o posterior; use ese intérprete para crear el entorno
virtual e instalar las dependencias.

## Migraciones y servidor

```powershell
python .\manage.py makemigrations catalogo
python .\manage.py migrate
python .\manage.py check
python .\manage.py runserver
```

Abra <http://127.0.0.1:8000/categoria/> para ver el listado. La migración
inicial agrega dos categorías de ejemplo (`tv` y `Laptop`) solo cuando la tabla
está vacía. La aplicación muestra las categorías que encuentra en la base de
datos.

## Probar el modelo desde Django Shell

```powershell
python .\manage.py shell
```

En la consola interactiva:

```python
from django.utils import timezone
from catalogo.models import Categoria

ahora = timezone.now()
categoria = Categoria.objects.create(
    nombre="Electrónicos",
    descripcion="Productos electrónicos",
    created_at=ahora,
    updated_at=ahora,
)
print(categoria)
categoria.nombre = "Televisores"
categoria.save()
categoria.delete()
```

Los otros modelos disponibles son `TipoPago`, `Cliente` y `Rol`. Para ejecutar
las pruebas:

```powershell
python .\manage.py test
```

En modo MariaDB también puede inspeccionar las tablas con
`python .\manage.py dbshell` y, dentro de la consola SQL, ejecutar
`SHOW TABLES;` o `DESCRIBE catalogo_categoria;`.
