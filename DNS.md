
# Servicio 01 – Servidor DNS named / BIND 
Instalar BIND y sacar copia de seguridad previa:
- sudo dnf install -y bind
- sudo cp -p /etc/named.conf /root/backups_interciclo/named.conf.bak_$(date +%F_%H%M)
# Editar /etc/named.conf (sudo nano /etc/named.conf): Dentro de options { ... }, deja estas líneas así:
```plaintext
listen-on port 53 { 127.0.0.1; 192.168.18.200; };
allow-query     { localhost; 192.168.18.0/24; };
```

Y al final del archivo agrega tu zona directa:

```plaintext
zone "nievecela.lab" IN {
    type master;
    file "nievecela.lab.zone";
    allow-update { none; };
};
```
# Crear el archivo de zona directa /var/named/nievecela.lab.zone:
- sudo cp -p /var/named/named.localhost /var/named/nievecela.lab.zone
- sudo nano /var/named/nievecela.lab.zone
Reemplaza todo su contenido por:
```plaintext
$TTL 86400
@   IN  SOA ns1.nievecela.lab. admin.nievecela.lab. (
            2026112001  ; Serial
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
# Permisos, validación, firewall y arranque:

- sudo chown root:named /var/named/nievecela.lab.zone
- sudo named-checkconf
- sudo named-checkzone nievecela.lab /var/named/nievecela.lab.zone
- sudo firewall-cmd --add-service=dns --permanent
- sudo firewall-cmd --reload
- sudo systemctl enable --now named
- Verificación: dig @192.168.18.200 web.nievecela.lab +short debe devolver 192.168.18.200.
