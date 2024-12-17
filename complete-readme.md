# Ejercicios Laravel y Base de Datos

## 1. Configuración Inicial
```bash
# Clonar repositorio e inicializar Git
git clone https://github.com/Ernst-Martin/ud3_ejercicios.git
git init
```

## 2. Creación del Proyecto Laravel
```bash
composer create-project laravel/laravel .
git add .
git commit -m "Hello World ejercicios UD3"
git push origin main
```

## 3. Configuración de Docker y MariaDB
```bash
# Crear Dockerfile
FROM mariadb:latest
ENV MYSQL_ROOT_PASSWORD=m1_s3cr3t
ENV MYSQL_USER=ig
ENV MYSQL_PASSWORD=m1_s3cr3t
EXPOSE 3306
CMD ["mariadbd"]

# Crear docker-compose.yml
version: '3'
services:
  db:
    build: .
    container_name: mariadb-server
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: m1_s3cr3t
      MYSQL_USER: ig
      MYSQL_PASSWORD: m1_s3cr3t
```

## 4. Ejercicio de Base de Datos test1 y test2
```bash
# Crear bases de datos
CREATE DATABASE test1;
CREATE DATABASE test2;

# Crear migración
php artisan make:migration my_test_migration

# Añadir campo 'apellido'
php artisan make:migration add_apellido_to_alumnos --table=alumnos

# Crear seeder
php artisan make:seeder AlumnosTableSeeder
```

## 5. Proyecto Gestión de Notas

### Crear Base de Datos
```bash
CREATE DATABASE gestion_notas;
GRANT ALL PRIVILEGES ON gestion_notas.* TO 'ig'@'%';
```

### Crear Migraciones
```bash
php artisan make:migration create_alumnos_table
php artisan make:migration create_asignaturas_table
php artisan make:migration create_notas_table
```

### Estructura de las Tablas

#### Alumnos
```php
Schema::create('alumnos', function (Blueprint $table) {
    $table->id();
    $table->string('nombre');
    $table->string('email')->unique();
    $table->timestamps();
});
```

#### Asignaturas
```php
Schema::create('asignaturas', function (Blueprint $table) {
    $table->id();
    $table->string('nombre');
    $table->text('descripcion');
    $table->timestamps();
});
```

#### Notas
```php
Schema::create('notas', function (Blueprint $table) {
    $table->id();
    $table->foreignId('alumno_id')->constrained('alumnos');
    $table->foreignId('asignatura_id')->constrained('asignaturas');
    $table->decimal('nota', 4, 2);
    $table->timestamps();
});
```

### Crear y Ejecutar Seeders
```bash
# Crear seeders
php artisan make:seeder AlumnosTableSeeder
php artisan make:seeder AsignaturasTableSeeder
php artisan make:seeder NotasTableSeeder

# Ejecutar seeders
php artisan db:seed
```

### Verificación de Datos
```sql
USE gestion_notas;
SELECT * FROM alumnos;
SELECT * FROM asignaturas;
SELECT * FROM notas;

-- Consulta relacionada
SELECT 
    alumnos.nombre as alumno,
    asignaturas.nombre as asignatura,
    notas.nota
FROM notas
JOIN alumnos ON notas.alumno_id = alumnos.id
JOIN asignaturas ON notas.asignatura_id = asignaturas.id;
```

## Comandos Útiles
```bash
php artisan migrate        # Ejecutar migraciones
php artisan migrate:fresh  # Refrescar migraciones
php artisan config:clear  # Limpiar configuración
docker-compose up -d      # Iniciar contenedor
```
