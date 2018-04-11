<!-- TITLE: Instalación y configuración COPO -->
<!-- SUBTITLE: A quick summary of Instalacion Y Configuracion Copo -->

# Clonar el proyecto
Realizamos una copia de la versión actual del proyecto en github

```text
git clone https://github.com/tuproyecto/plataforma.git
```

En un equipo que no tenga configurada la base de datos mongo db editamos los archivos composer.json quitando la linea 
```text
"jenssegers/mongodb": "^3.0",
```

y del archivo config/app.php comentareamos la linea

```text
Jenssegers\Mongodb\MongodbServiceProvider::class,
```

asignamos permisos a la carpeta storage 
```text
sudo chmod -R 777 storage
```

luego ejecutamos el comando 

```text
sudo composer install --no-scripts
```
Creamos el archivo .env
```text
cp .env.local .env
```
Generamos la llave
```text
php artisan key:generate
```
Creamos o si ya existe damos permisos sobre la carpeta css/institution
```text
mkdir css/institution
chmod 777 css/institution/
```
