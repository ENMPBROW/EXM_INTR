# Identificación del Nodo y Conectividad

**1. Verificar identidad, hostname e IP:**
```bash
whoami
hostname
ip a
```
* **Verificación:** Muestra `esteban`, `srv-nievecela` y `192.168.18.205/24` en la interfaz `enp0s3`.

**2. Probar salida a Internet con 10 paquetes:**
```bash
ping -c 10 8.8.8.8
```
* **Verificación:** Termina con `10 packets transmitted, 10 received, 0% packet loss`.

---

# Carpeta Central de Respaldos

**1. Crear el directorio dedicado de respaldos y verificarlo:**
```bash
sudo mkdir -p /root/backups_interciclo
sudo ls -ld /root/backups_interciclo
```
* **Verificación:** Muestra el directorio `/root/backups_interciclo` creado con permisos `drwxr-xr-x`.


<img width="829" height="751" alt="image" src="https://github.com/user-attachments/assets/b85242d2-f7f2-449b-82e5-ab91813adfa8" />

