<!-- TITLE: Correccion De Usuario Y Archivo De Servidor -->
<!-- SUBTITLE: Corrección del propietario del archivo autorization_key y corrección de archivo -->

# Crear un snap del servidor
Ir a EC2

Opción Snapshot

Crear Snapshot

Seleccionar el volumen sobre el que se quiere realizar el snap

Adicionar nombre y descripción

Luego botón Create
# Crear un volumen
Seleccionar el snapshot, luego oprimir Actions y seleccionar la opción Create Volume

Ir a la opción Volumes o escoger el Volumen que acabamos de crear. 

# Montar el servidor de recuperación
Seleccionar el Volumen creado y oprimir el botón de Actions

Seleccionar la opción Attach Volumen 

Seleccionar la instancia de recuperación

y en Device la opción en donde se montara para este caso /dev/sdf
# Corregir el archivo
Inicar la instancia de recuperación y conectarse
Montar el Volumen

```sh
sudo mount /dev/xvdf /mnt
```

Ir a Volumen

```sh
cd /mnt/home/admin/.ssh
```

Ver el usuario propietario del archivo authorized_keys, este deberá ser admin

```sh
ls -l
```

Verificar que el archivo este bien, este archivo deber tener exactamente el mismo valor que la llave .pem de conexion

```sh
cat authorized_keys
```

Verificar la llave de conexión, normlamente en el equipo desde el que nos intentamos conectar

```sh
ssh-keygen -yh -f devtuproyecto.pem
```

Si el archivo authorized_keys esta diferente se debe editar para dejarla exactamente como la llave de conexión
Desocupamos el archivo, esto se debe hacer con el usuario root

```sh
sudo -s
> authorized_keys
```

Y luego agregamos el valor la llave .pem

```sh
vi authorized_keys
```

Verificamos que el valor este exacto

```sh
cat authorized_keys
```

# Corregir el propietario
Si ya esta bien el archivo procedemos a corregir el usuario propietario

```sh
chown admin:admin authorized_keys
```

Revisamos que el propietario sea admin

```sh
ls -l
```

Desmontamos el Volumen

```sh
exit
cd /
sudo umount /mnt
```

Nos salimos de la instancia

```sh
exit
```

Detenemos la instancia
En Volumenes desmontamos el Volumen de la instancia de recuperación 
y lo montamos como columen principal en la instancia imagen en este caso /dev/sda
Inicamos esta instancia y nos conectamos.

Si permite la conexión el problema esta resuelto

