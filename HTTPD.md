## Servicio 02 – Servidor Web (Apache / httpd)

# Instalación del servidor web Apache

```bash
sudo dnf install -y httpd
```

### 2. Respaldo de configuración y preparación de la plantilla base

```bash
sudo cp -p /etc/httpd/conf/httpd.conf /root/backups_interciclo/httpd.conf.bak_$(date +%F_%H%M)
sudo cp /usr/share/httpd/noindex/index.html /var/www/html/index.html
sudo cp -p /var/www/html/index.html /root/backups_interciclo/index.html.bak_$(date +%F_%H%M)
sudo ls -lh /root/backups_interciclo/
```
<img width="743" height="100" alt="image" src="https://github.com/user-attachments/assets/bddae17f-3dbd-4835-ae78-662e669c593b" />


### 3. Habilitación en firewall y arranque del servicio

```bash
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --reload
sudo systemctl enable --now httpd
systemctl status httpd
```
<img width="804" height="81" alt="image" src="https://github.com/user-attachments/assets/cc5db5d5-603a-417b-a749-14cd722d4e73" />

### 4. Personalización del portal web (`/var/www/html/index.html`)

- Copia del logotipo institucional a la raíz del servidor web y asignación de permisos y contexto SELinux:

```bash
sudo cp ~/Imágenes/logo.jpeg /var/www/html/logo.jpg
sudo chown apache:apache /var/www/html/logo.jpg
sudo chmod 644 /var/www/html/logo.jpg
sudo restorecon -v /var/www/html/logo.jpg
```

- Edición del archivo `/var/www/html/index.html` (`sudo nano /var/www/html/index.html`):
  - En la barra superior (`<nav class="navbar ..."><div class="container">`), se integró el logotipo institucional junto al logotipo de CentOS:

```html
<img src="logo.jpg" alt="Logo" style="height:55px; margin-left:15px; margin-right:auto; vertical-align:middle;">
```
<img width="1915" height="1019" alt="image" src="https://github.com/user-attachments/assets/6b355239-e082-44c3-858e-680b171caca9" />
