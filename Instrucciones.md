## COPIAS REMOTAS
Generar claves : ssh-keygen -t rsa
Conectarse: ssh IP
Clave a servidor remoto: nano .ssh/authorized_keys --Agregar comentario y poner clave.
Copia remota: rsync -avz -e ssh /desde usuario@IP:/hasta
Copia segura: scp /archivoACopiar usuario@IP:/hasta

## CERTIFICADOS Y SERVIDOR

sudo apt-get install apache2

### Agregamos ssl
sudo a2enmod ssl
sudo systemctl restart apache2

### Crear sitio
Crear sitio: sudo mkdir -p /var/www/nombreSitio.com/public_html
Dar permisos: sudo chown -R $florencia:$florencia /var/www/nombreSitio.com/public_html
Otros permisos: sudo chmod -R 777 /var/www
Crear index: nano /var/www/nombreSitio.com/public_html/index.html

### Crear configuración
Copiar configuración default: sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-avaiblable/nombreSitio.conf
Editar archivo: sudo nano /etc/apache2/sites-available/nombreSitio.conf 
Editamos: ServerAdmin: tuMail, ServerName: nombreSitio.com, ServerAlias: www.nombreSitio.com, DocumentRoot: sacamos /html y ponemos /var/www/nombreSitio.com/public_html

### Editamos hosts
Editamos: sudo nano /etc/hosts 
Agregamos: IPMaquinaVirtual nombreSitio.com

### Publicamos sitio
sudo a2ensite nombreSitio.conf
sudo a2dissite 000-default.conf

### Hacemos restart
restart: sudo service apache2 restart o systemctl restart apache2.service

### Creamos certificado
Comando: sudo open ssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt


Editamos archivo: sudo nano /etc/apache2/sites-available/nombreSitio.conf
Editamos: 
SSLEngine on
SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
Cambiamos puerto a 443.
Agregamos:
<VirtualHost *:80>
    ServerName nombreSitio.com
    Redirect / https://nombreSitio.com/
</VirtualHost>
Verificar que no hay errores: sudo apache2ctl configtest
reload: systemctl reload apache2
restart: sudo service apache2 restart o systemctl restart apache2.service

## GPG

Generar clave con gpg:
gpg --gen-key

### Intercambiar claves

Exportar: gpg --armor --output florencia.asc --export floojeda99@gmail.com
Importar: gpg --import vale.asc
Verificar: gpg --list-keys

### Cifrar

Para cifrar: gpg --output ruta/donde/guardo/el/doc/cifrado/.gpg --encrypt --recipient vrostan001@ikasle.egu.eus ruta/donde/esta/el/doc/a/cifrar

Para descifrar:
gpg --output ruta/donde/guardo/el/doc/Descifrado --decrypt ruta/del/doc/a/cifrar (RECORDAR: si esta en home es solo el nombre)


borrar key: gpg --delete-key key-ID

### Firmar y verificar firma

Para firmar: gpg --output ruta/donde/guardo/el/doc/firmado/.sig --sign ruta/donde/esta/el/doc/a/firmar

Para verificar firma: gpg --output ruta/donde/quedara/guardado/el/doc/firmado --verify ruta/donde/guardo/el/doc/firmado/.sig


maual: https://gnupg.org/gph/es/manual.html
