# Servicio 01: Servidor DNS (named / BIND)

**1. Instalar BIND y sus herramientas de consulta (dig / nslookup):**
```bash
sudo dnf install -y bind bind-utils
```
* **Verificación:** Termina con `¡Listo!` o `Complete!`.

**2. Respaldar la configuración original y verificar la copia:**
```bash
sudo cp -p /etc/named.conf /root/backups_interciclo/named.conf.bak_$(date +%F_%H%M)
sudo ls -lh /root/backups_interciclo/
```
* **Verificación:** Se lista el archivo `named.conf.bak_...` dentro de `/root/backups_interciclo/` sin errores.

**3. Editar `/etc/named.conf`:**
```bash
sudo nano /etc/named.conf
```
Dentro del bloque `options { ... }`, deja estas dos líneas con la IP `192.168.18.200`:
```text
listen-on port 53 { 127.0.0.1; 192.168.18.200; };
allow-query     { localhost; 192.168.18.0/24; };
```
Al final del archivo agrega tu zona directa:
```text
zone "nievecela.lab" IN {
    type master;
    file "nievecela.lab.zone";
    allow-update { none; };
};
```
Guarda (`Ctrl + O`, `Enter`) y sal (`Ctrl + X`).
* **Verificación:** Ejecuta `sudo named-checkconf`; si no muestra errores, la sintaxis es correcta.

**4. Crear, verificar y editar el archivo de zona `/var/named/nievecela.lab.zone`:**
```bash
sudo cp -p /var/named/named.localhost /var/named/nievecela.lab.zone
sudo ls -lh /var/named/nievecela.lab.zone
sudo nano /var/named/nievecela.lab.zone
```
Reemplaza todo su contenido por:
```text
$TTL 86400
@   IN  SOA ns1.nievecela.lab. admin.nievecela.lab. (
            2026112101  ; Serial
            3600        ; Refresh
            1800        ; Retry
            604800      ; Expire
            86400 )     ; Minimum TTL

@       IN  NS  ns1.nievecela.lab.

ns1     IN  A   192.168.18.200
srv     IN  A   192.168.18.200
web     IN  A   192.168.18.200
ssh     IN  A   192.168.18.200
admin   IN  A   192.168.18.200
```
Guarda (`Ctrl + O`, `Enter`) y sal (`Ctrl + X`).
* **Verificación:** El archivo queda guardado con todos los registros apuntando a `192.168.18.200`.

**5. Asignar permisos, validar zona, abrir firewall y arrancar DNS:**
```bash
sudo chown root:named /var/named/nievecela.lab.zone
sudo named-checkzone nievecela.lab /var/named/nievecela.lab.zone
sudo firewall-cmd --add-service=dns --permanent
sudo firewall-cmd --reload
sudo systemctl enable --now named
dig @192.168.18.200 web.nievecela.lab +short
```
* **Verificación:** `named-checkzone` devuelve `OK` y `dig` responde `192.168.18.200`.
