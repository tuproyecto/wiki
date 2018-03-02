<!-- TITLE: Correccion De Usuario Y Archivo De Servidor -->
<!-- SUBTITLE: Corrección del propietario del archivo autorization_key y corrección de archivo -->

# Crear un snap del servidor
Ir a EC2

Opción Snapshot

Crear Snapshot

Seleccionar el volumen sobre el que se quiere realizar el snap

Adicionar nombre y descripcion

luego boton Create
# Crear un volumen
Seleccionar el snapshot, luego oprimir Actions y seleccionar la opción Create Volume

Ir a la opcion Volumes o escoger el volumen que acabamos de crear. 

# Montar el servidor de recuperación
Seleccionar el volumen creado y oprimir el boton de Actions

Seleccionar la opcion Attach Volumen 

Seleccionar la instancia de recuperacion

y en Device la opcion en donde se montara para este caso /dev/sdf
# Corregir el archivo
Inicar la instancia de recuperacion y conectarse
Montar el volumen

```sh
sudo mount /dev/xvdf /mnt
```

ir a volumen

```sh
cd /mnt/home/admin/.ssh
```

ver el usuario propietario del archivo authorized_keys, este debera ser admin

```sh
ls -l
```

Verificar que el archivo este bien, este archivo deber tener exactamente el mismo valor que la llave .pem de conexion

```sh
cat authorized_keys
```

Verificar la llave de conexion, normlamente en el equipo desde el que nos intentamos conectar

```sh
ssh-keygen -yh -f devtuproyecto.pem
```

Si el archivo authorized_keys esta diferente se debe editar para dejarla exactamente como la llave de conexion
Desocupamos el archivo, esto se debe hacer con el usuario root

```sh
sudo -s
> authorized_keys
```

y luego agregamos el valor la la llave .pem

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

revisamos que el propietario sea admin

```sh
ls -l
```

Desmontamos el volumen

```sh
exit
cd /
sudo umount /mnt
```

nos salimos de la instancia

```sh
exit
```

Detenemos la instancia
En volumenes desmontamos el volumen de la instancia de recuperacion 
y lo montamos como columen principal en la instancia imagen en este caso /dev/sda
Inicamos esta instancia y nos conectamos.

Si permite la conexion el problema esta resuelto

