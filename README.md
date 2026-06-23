
# Configuracion Inicial

Comandos para instalar todo lo necesario para trabajar desde Cachy OS.



## Comandos basicos
### Yay - Para instalar todos los paquetes de AUR
```
sudo pacman -S --needed git base-devel
cd ~
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

### Zen Browser - Navegador

```
sudo pacman -S zen-browser-bin
```

### Discover - Tienda de aplicaciones por flatpak

```
sudo pacman -S discover flatpak
```
## Entorno de Desarrollo
### Java - Necesario para Netbeans y JDK

```
sudo pacman -S jdk8-openjdk
sudo pacman -Syu subversion
```

### NetBeans 8.2

[NetBeans](https://drive.google.com/file/d/1TXWv4wZEboH92jGmIUzdRO1HiflAD1oh/view?usp=drive_link)


```
chmod +x netbeans-8.2-linux.sh
./netbeans-8.2-linux.sh
```

### MariaDB - MYSQL

```
sudo pacman -S mysql
sudo mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
sudo systemctl start mysqld
sudo systemctl enable mysql.service
sudo mysql_secure_installation
```

### Navicat - Se puede usar la imagen de la pagina oficial o con flatpak

```
flatpak install https://dn.navicat.com/flatpak/flatpakref/navicat17/com.navicat.premium.es.flatpakref
flatpak run com.navicat.premium.es
```

### Reactivar Navicat
Si pasa el tiempo de prueba de Navicat ejecutar el siguiente comando para reiniciar el tiempo de prueba

[Navi.sh](https://drive.google.com/file/d/194XAhLmKGK2-eZZTq4uJZg5KzLvEaW8S/view?usp=drive_link)


`Primera vez ejecutar el siguiente comando`
```
chmod +x navi.sh
```

Reiniciar periodo de prueba
```
./navi.sh
```

### Visual Studio Code - Opcional ~Se puede usar nano para configurar el archivo de docker~

```
yay -S visual-studio-code-bin
```

### Docker - Necesario para SQL Server 2017

```
sudo pacman -S docker docker-compose
sudo systemctl enable --now docker
sudo usermod -aG docker nombreUsuario
getent group docker
```


## Configuracion de Netbeans

En la ruta de `/home/miguel/glassfish-4.1.1/glassfish/domains/domain1/lib/` colocar los conectores de SQL

[SQL](https://drive.google.com/file/d/1DZrLiURWvrv_B1N0yD5QlXysvNaOeu5F/view?usp=drive_link)

[MySQL](https://drive.google.com/file/d/1VOd8Qh8eEpPG7XeHKNEgJ2K-aayjU3wA/view?usp=drive_link)

### Maven

En la ruta `/home/miguel/netbeans-8.2/java/maven/conf/` colocar el siguiente archivo para sobrescribir el ya existente

[Settings.xml](https://drive.google.com/file/d/17XxLOFj6wP8rnousVCKSkNkeQ42y-ouQ/view?usp=drive_link)
## Comandos para Docker

[Docker-compose.yml](https://drive.google.com/file/d/1QMz7jCdBTzAHc3R5iXEGlmWfCIldA6QN/view?usp=drive_link)

```
cd ~/Documentos/Docker/
```

### levantar
```
sudo docker-compose up -d
```

### bajar
```
sudo docker-compose down
```

### ver logs si algo falla
```
sudo docker logs sqlserver2017
```

### verificar que corre
```
sudo docker ps
```
## Restaurar .BAK

[Respaldo Base de datos](https://drive.google.com/file/d/1MwuXZyZQin60WAzjLdUOcbSG7XV_CR0s/view?usp=drive_link)

### Copiar el .BAK al contenedor

```
docker cp ~/Descargas/tmp/ARCHIVO.BAK sqlserver2017:/var/opt/mssql/backup/ARCHIVO.BAK
```

### SIGAF

```
sudo docker exec -it sqlserver2017 /opt/mssql-tools/bin/sqlcmd \
  -S localhost -U SA -P "Root123!" \
  -Q "RESTORE DATABASE [SIGAF]
      FROM DISK = '/var/opt/mssql/backup/SIGAF.bak'
      WITH MOVE 'SIGAF'     TO '/var/opt/mssql/data/SIGAF.mdf',
           MOVE 'SIGAF_log' TO '/var/opt/mssql/data/SIGAF_log.ldf',
           REPLACE, STATS = 10"
```

### EGOBGDU

```
sudo docker exec -it sqlserver2017 /opt/mssql-tools/bin/sqlcmd \
  -S localhost -U SA -P "Root123!" \
  -Q "RESTORE DATABASE [EGOBGDU]
      FROM DISK = '/var/opt/mssql/backup/EGOBGDU.bak'
      WITH MOVE 'EGOBGDU_Data' TO '/var/opt/mssql/data/EGOBGDU.mdf',
           MOVE 'EGOBGDU_Log'  TO '/var/opt/mssql/data/EGOBGDU_log.ldf',
           REPLACE, STATS = 10"
```

### EGOBGLC  ~este no necesita MOVE explícito, funciona directo~

```
docker cp ~/Descargas/tmp/EGOBGLC.BAK sqlserver2017:/var/opt/mssql/backup/EGOBGLC.BAK
sudo docker exec -it sqlserver2017 /opt/mssql-tools/bin/sqlcmd \
  -S localhost -U SA -P "Root123!" \
  -Q "RESTORE DATABASE [EGOBGLC]
      FROM DISK = '/var/opt/mssql/backup/EGOBGLC.BAK'
      WITH REPLACE, STATS = 10"
```

### EGOBSCI

```
sudo docker exec -it sqlserver2017 /opt/mssql-tools/bin/sqlcmd \
  -S localhost -U SA -P "Root123!" \
  -Q "RESTORE DATABASE [EGOBSCI]
      FROM DISK = '/var/opt/mssql/backup/EGOBSCI.BAK'
      WITH MOVE 'EGOBSCI_Data' TO '/var/opt/mssql/data/EGOBSCI.mdf',
           MOVE 'EGOBSCI_Log'  TO '/var/opt/mssql/data/EGOBSCI_log.ldf',
           REPLACE, STATS = 10"
```

### EGOBSEG

```
sudo docker exec -it sqlserver2017 /opt/mssql-tools/bin/sqlcmd \
  -S localhost -U SA -P "Root123!" \
  -Q "RESTORE DATABASE [EGOBSEG]
      FROM DISK = '/var/opt/mssql/backup/EGOBSEG.BAK'
      WITH MOVE 'EGOBSEG_Data' TO '/var/opt/mssql/data/EGOBSEG.mdf',
           MOVE 'EGOBSEG_Log'  TO '/var/opt/mssql/data/EGOBSEG_log.ldf',
           REPLACE, STATS = 10"
```

### EGOBSADEN

```
sudo docker exec -it sqlserver2017 /opt/mssql-tools/bin/sqlcmd \
  -S localhost -U SA -P "Root123!" \
  -Q "RESTORE DATABASE [EGOBSADEN]
      FROM DISK = '/var/opt/mssql/backup/EGOBSADEN.BAK'
      WITH MOVE 'SADEN_Data' TO '/var/opt/mssql/data/EGOBSADEN.mdf',
           MOVE 'SADEN_Log'  TO '/var/opt/mssql/data/EGOBSADEN_log.ldf',
           REPLACE, STATS = 10"
```

### SB_SACH_20_Intermedia

```
sudo docker exec -it sqlserver2017 /opt/mssql-tools/bin/sqlcmd \
  -S localhost -U SA -P "Root123!" \
  -Q "RESTORE DATABASE [SB_SACH_20_Intermedia]
      FROM DISK = '/var/opt/mssql/backup/SB_SACH_20_Intermedia.BAK'
      WITH MOVE 'SB_SACH_20_Intermedia'     TO '/var/opt/mssql/data/SB_SACH_20_Intermedia.mdf',
           MOVE 'SB_SACH_20_Intermedia_log' TO '/var/opt/mssql/data/SB_SACH_20_Intermedia_log.ldf',
           REPLACE, STATS = 10"
```

### Ver nombres de un .BAK antes de restaurarlos

```
sudo docker exec -it sqlserver2017 /opt/mssql-tools/bin/sqlcmd \
  -S localhost -U SA -P "Root123!" \
  -Q "RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/ARCHIVO.BAK'"
  ```
## Pool-Connection

### Copiar el driver de MSSQL a GlassFish

```
cp ~/.m2/repository/com/microsoft/sqlserver/mssql-jdbc/13.1.1.jre8-preview/mssql-jdbc-13.1.1.jre8-preview.jar \
~/glassfish-4.1.1/glassfish/domains/domain1/lib/
```

### Reiniciar GlassFish para que cargue el driver

```
~/glassfish-4.1.1/bin/asadmin stop-domain domain1
~/glassfish-4.1.1/bin/asadmin start-domain domain1
```

### Creacion de pool-Connection

*pool_sigaf → base SIGAF*

```
~/glassfish-4.1.1/bin/asadmin create-jdbc-connection-pool \
  --datasourceclassname=com.microsoft.sqlserver.jdbc.SQLServerDataSource \
  --restype=javax.sql.DataSource \
  --property User=sa:Password='Root123!':DatabaseName=SIGAF:ServerName=localhost:PortNumber=1433 \
  pool_sigaf
```

*pool_egobgdu → base EGOBGDU*

```
~/glassfish-4.1.1/bin/asadmin create-jdbc-connection-pool \
  --datasourceclassname=com.microsoft.sqlserver.jdbc.SQLServerDataSource \
  --restype=javax.sql.DataSource \
  --property User=sa:Password='Root123!':DatabaseName=egobgdu:ServerName=localhost:PortNumber=1433 \
  pool_egobgdu`

```
*pool_serverBox → base SB_SACH_20_Intermedia*

```
~/glassfish-4.1.1/bin/asadmin create-jdbc-connection-pool \
  --datasourceclassname=com.microsoft.sqlserver.jdbc.SQLServerDataSource \
  --restype=javax.sql.DataSource \
  --property User=sa:Password='Root123!':DatabaseName=SB_SACH_20_Intermedia:ServerName=localhost:PortNumber=1433 \
  pool_serverBox`
```

### *Configurar SSL* ~Sin esto no puede hacer ping~

```
for POOL in pool_sigaf pool_egobgdu pool_serverBox; do
  ~/glassfish-4.1.1/bin/asadmin set "resources.jdbc-connection-pool.$POOL.property.encrypt=false"
  ~/glassfish-4.1.1/bin/asadmin set "resources.jdbc-connection-pool.$POOL.property.trustServerCertificate=true"
done
```
## JDBC Resources

### pool_sigaf
```
~/glassfish-4.1.1/bin/asadmin create-jdbc-resource --connectionpoolid pool_sigaf jdbc/pool_sigaf

~/glassfish-4.1.1/bin/asadmin create-jdbc-resource --connectionpoolid pool_sigaf jdbc/sigaf__pm

~/glassfish-4.1.1/bin/asadmin create-jdbc-resource --connectionpoolid pool_sigaf jdbc/sigaf__nontx
```

### pool_egobgdu

```
~/glassfish-4.1.1/bin/asadmin create-jdbc-resource --connectionpoolid pool_egobgdu jdbc/pool_egobgdu
```

### pool_serverBox

```
~/glassfish-4.1.1/bin/asadmin create-jdbc-resource --connectionpoolid pool_serverBox jdbc/serverBox

~/glassfish-4.1.1/bin/asadmin create-jdbc-resource --connectionpoolid pool_serverBox jdbc/serverBox__pm

~/glassfish-4.1.1/bin/asadmin create-jdbc-resource --connectionpoolid pool_serverBox jdbc/serverBox__nontx
```

### Hacer ping

```
~/glassfish-4.1.1/bin/asadmin ping-connection-pool pool_sigaf

~/glassfish-4.1.1/bin/asadmin ping-connection-pool pool_egobgdu

~/glassfish-4.1.1/bin/asadmin ping-connection-pool pool_serverBox
```
## Scrcpy

Ver la pantalla de la tablet desde la computadora

```
sudo pacman -S scrcpy
```

*Para iniciar y escuchar el audio de la tablet*

```
scrcpy --no-audio
```
## AnyDesk

```
yay -S anydesk-bin
```

## OpenFortiVPN

*Instalar*
```
sudo pacman -S openfortivpn
```

*Conexión* ~usar siempre con --trusted-cert para evitar el prompt~

```
sudo openfortivpn 187.189.117.14:443 --username=g4_1 \
  --trusted-cert 2c0ad75d10c90e24d778cdf2a2d48260d6a250584eeb3c2efdce2cf21dff63b2
  ```
