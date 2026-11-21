
## Servicio 01 – Servidor DNS (named / BIND)

 -  Instalar BIND y sus herramientas de consulta:
* `sudo dnf install -y bind bind-utils`

### Respaldar configuración original:

* `sudo cp -p /etc/named.conf /root/backups_interciclo/named.conf.bak_$(date +%F_%H%M)`
* Verificar : `sudo ls -lh /root/backups_interciclo/`

### Editar `/etc/named.conf`:

* `sudo nano /etc/named.conf`
* Dentro de `options { ... }`, deja estas dos líneas con tu IP:
```text
listen-on port 53 { 127.0.0.1; 192.168.18.205; };
allow-query     { localhost; 192.168.18.0/24; };
````
* Al final del archivo agrega tu zona directa:

```text
zone "nievecela.lab" IN {
    type master;
    file "nievecela.lab.zone";
    allow-update { none; };
};
```

* Guarda, sal y valida sintaxis con `sudo named-checkconf`.

### Crear y editar la zona `/var/named/nievecela.lab.zone`:

* `sudo cp -p /var/named/named.localhost /var/named/nievecela.lab.zone`
* `sudo nano /var/named/nievecela.lab.zone`

  - Reemplaza todo el contenido por:

```text
$TTL 86400
@   IN  SOA ns1.nievecela.lab. admin.nievecela.lab. (
            2026112101  ; Serial
            3600        ; Refresh
            1800        ; Retry
            604800      ; Expire
            86400 )     ; Minimum TTL

@       IN  NS  ns1.nievecela.lab.

ns1     IN  A   192.168.18.205
srv     IN  A   192.168.18.205
web     IN  A   192.168.18.205
ssh     IN  A   192.168.18.205
admin   IN  A   192.168.18.205
```

### Permisos, firewall y arranque del servicio:

* `sudo chown root:named /var/named/nievecela.lab.zone`
* `sudo named-checkzone nievecela.lab /var/named/nievecela.lab.zone`
* `sudo firewall-cmd --add-service=dns --permanent`
* `sudo firewall-cmd --reload`
* `sudo systemctl enable --now named`
* `dig @192.168.18.205 web.nievecela.lab +short`

  * **Verificación:** `named-checkzone` devuelve `OK` y `dig` responde `192.168.18.205`.
