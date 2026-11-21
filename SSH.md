# Servicio 03 – Acceso Remoto SSH en Puerto 2237 
- Sacar copia de seguridad previa de SSH:
- sudo cp -p /etc/ssh/sshd_config /root/backups_interciclo/sshd_config.bak_$(date +%F_%H%M)
# Cambiar el puerto al 2237 en /etc/ssh/sshd_config:
- sudo nano /etc/ssh/sshd_config
- Busca #Port 22, quita el # y cámbialo a Port 2237. Guarda y sal.
# Configurar SELinux, firewall y reiniciar SSH:

- sudo dnf install -y policycoreutils-python-utils
- sudo semanage port -a -t ssh_port_t -p tcp 2237
- sudo firewall-cmd --add-port=2237/tcp --permanent
- sudo firewall-cmd --remove-service=ssh --permanent
- sudo firewall-cmd --reload
- sudo systemctl restart sshd


# Verificación Final en Servidor y Cliente Windows

- sudo ls -la /root/backups_interciclo/
- sudo firewall-cmd --list-all
- getenforce
# En tu Windows (CMD como Administrador):
DOS
- netsh interface ip set dns name="Wi-Fi 2" static 192.168.18.200
- ipconfig /flushdns
- nslookup web.nievecela.lab
- nslookup ssh.nievecela.lab
# Pruebas visuales en Windows:
o	Abre Chrome y entra a [http://web.nievecela.lab](http://web.nievecela.lab) (cargará la página por defecto de CentOS Stream / Apache).
o	Abre MobaXterm y conéctate a ssh.nievecela.lab en el puerto 2237 con tu usuario esteban.
