# Proyecto NGIX con Vagrant

Este Vagrantfile configura una máquina virtual utilizando Vagrant con una base Debian (bookworm64). Configura una red privada, instala Nginx, Git y ufw, y establece dos sitios web: uno basado en Git y otro que hacemos con cp. La configuración también incluye la creación de un certificado SSL y la gestión de servicios de Nginx y el certificado de un sitio seguro.

## Estructura de Archivos

### Vagrantfile
Este archivo es la configuración principal para Vagrant:

  - Nombre de la VM: ismael
  - IP de red privada: `192.168.57.102`
  - Distribucion: `debian/bookworm64`

  **Sitio Web**
  - Crea un directorio para el contenido web en /var/www/ismael/html.
  - Clona un ejemplo de sitio web estático desde GitHub en este directorio.
  - Establece la propiedad para www-data y permisos.
  - Copia el archivo de configuración del sitio Nginx desde /vagrant/ismael-sites a /etc/nginx/sites-available/.
  - Habilita el sitio mediante un enlace simbólico en /etc/nginx/sites-enabled/.

### ismael-sites
```
server {
    listen 80;
    listen 443 ssl;
    root /var/www/ismael/html;
    server_name ismael www.ismael.com;
    index index.html index.htm index.nginx-debian.html;
    ssl_certificate /etc/ssl/certs/ismael.crt;
    ssl_certificate_key /etc/ssl/private/ismael.key;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    location / {
        try_files $uri $uri/ =404;
    }
}


```

- Raíz del documento: El contenido del servidor está ubicado en el directorio /var/www/ismael/html.
- Nombre del servidor: El nombre del servidor es ismael.
- Le pasamos toda la informacion del certificado que hemos creado




