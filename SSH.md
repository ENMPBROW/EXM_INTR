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

<img width="786" height="539" alt="image" src="https://github.com/user-attachments/assets/59dff840-3a50-49cd-928d-7b2ed5a6f6a0" />

# En tu Windows (CMD como Administrador):

- netsh interface ip set dns name="Wi-Fi 2" static 192.168.18.205
- ipconfig /flushdns
- nslookup web.nievecela.lab
- nslookup ssh.nievecela.lab
<img width="754" height="518" alt="image" src="https://github.com/user-attachments/assets/aa765b83-f75c-42f0-840e-cdea5e1301d4" />

# Pruebas visuales en Windows:
o	Abre Chrome y entra a [http://web.nievecela.lab](http://web.nievecela.lab) (cargará la página por defecto de CentOS Stream / Apache).
o	Abre MobaXterm y conéctate a ssh.nievecela.lab en el puerto 2237 con tu usuario esteban.

<img width="1919" height="385" alt="image" src="https://github.com/user-attachments/assets/8897f160-a0e8-4d13-a73e-f754f7e177a5" />

