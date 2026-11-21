4. Servicio 02 – Servidor Web httpd / Apache 
# Instalar Apache y sacar copia de seguridad previa:
- sudo dnf install -y httpd
- sudo cp -p /etc/httpd/conf/httpd.conf /root/backups_interciclo/httpd.conf.bak_$(date +%F_%H%M)

# Abrir firewall y arrancar Apache (sin tocar el index.html por defecto):
- sudo firewall-cmd --add-service=http --permanent
- sudo firewall-cmd --reload
- sudo systemctl enable --now httpd
- Verificación: sudo systemctl status httpd  ------->debe responder active.
