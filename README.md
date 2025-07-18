# Seminario_Sistemas_Operativos
Seminario de Sistemas Operativos - Estudiante 10

# Configuración de NTP en Servidor Linux usando Webmin 📡

---

# Introducción 📌

Webmin es una interfaz web para la administración de sistemas Unix/Linux. Permite realizar tareas administrativas como gestión de usuarios, servicios, configuración de red, instalación de paquetes, y más, todo desde el navegador y sin necesidad de usar la terminal directamente.

En esta tarea, se utilizó **Webmin** para instalar y configurar el servicio **NTP (Network Time Protocol)**, con el objetivo de mantener el servidor sincronizado con servidores horarios oficiales en Internet.

# Paso a paso 🛠️
---

## 1. Acceso al servidor y a Webmin 🔑

- Se ingresó al servidor mediante Webmin con credenciales administrativas.

![image](https://github.com/user-attachments/assets/ec0ad286-e014-4600-ab8e-ef4b8b0b8f13)
![image](https://github.com/user-attachments/assets/675c2520-1f15-4af8-a22c-89f12eef1902)

## 2. Verificación del sistema operativo 🖥️

- Se verificó que el sistema operativo es **Ubuntu 22.04.5 LTS** desde el panel.

## 3. Instalación del servicio NTP (`ntpd`) ⚙️

- Se accedió al módulo:  
  `Tools > Command Shell`  

  ![image](https://github.com/user-attachments/assets/778b0383-0267-4fb0-bc08-d1ff533005ce)

- Se ejecutó el siguiente comando combinado:

  ```bash
  apt update && apt install ntp -y

![image](https://github.com/user-attachments/assets/47899565-eb5c-4fdb-80d2-251e889d3b39)

Durante la instalación, se eliminó automáticamente el servicio `systemd-timesyncd`, que viene por defecto, y fue reemplazado por `ntpd`.

## 4. Verificación del estado del servicio ✅

- Se verificó que el servicio estuviera activo.

![image](https://github.com/user-attachments/assets/127ece56-3688-453b-94cb-3c5ff2ea9a98)

  El resultado indicó que el servicio estaba **activo (running)** correctamente.

- También se puede verificar desde **Command Shell** con el siguiente comando:

  ```bash
  systemctl status ntp

## 5. Configuración desde Webmin 🕒

- En Webmin, se ingresó a:

  **Hardware > System Time**

- En el campo de **servidores NTP**, se ingresaron: 
0.pool.ntp.org
1.pool.ntp.org
2.pool.ntp.org


- Se marcaron las siguientes opciones:

- ✅ **Synchronize when Webmin starts** → Yes  
- ✅ **Synchronize on schedule** → Yes  
  - **Horas**: 6 (cada 6 horas)  
  - **Minutos**: 0  

- Se hizo clic en **Sync and Apply** para guardar la configuración.

![image](https://github.com/user-attachments/assets/d129a149-7bb4-4b7e-8c4e-1fc7c50bd501)

## 6. Intento de sincronización manual desde Webmin ⚠️

- Al presionar el botón para sincronizar la hora manualmente, apareció el siguiente error: adj_systime: Operation not permitted
- Esto ocurre porque Webmin utiliza el comando `sntp` sin privilegios de **root**, lo cual **no afecta** al servicio `ntpd` que ya está corriendo correctamente.

## 7. Verificación de la sincronización del sistema 🔍

- Se usó el siguiente comando para confirmar que `ntpd` estaba sincronizando correctamente:

  ```bash
  timedatectl status

![image](https://github.com/user-attachments/assets/246579cd-bf45-4260-af3f-232bc8e3d5d5)

Resultado obtenido: 
  
    System clock synchronized: yes
    NTP service: n/a

Esto es correcto, ya que timedatectl no detecta ntpd, pero confirma que el sistema sí está sincronizado.

- Se ejecuto el siguiente comando:

      ntpd -p

- Se mostró una lista de servidores NTP activos.

![image](https://github.com/user-attachments/assets/3fa3541e-59aa-45eb-ab1b-d2983edf23c8)

- El servidor con asterisco `*` indica el servidor **actualmente en uso**.

- Otros servidores marcados con `+` están disponibles como **respaldo**.

# Conclusión 📝

Durante esta práctica se aprendió a instalar y configurar correctamente el servicio NTP en un servidor Linux utilizando la interfaz web de Webmin, y a diferenciar entre `systemd-timesyncd` y el servicio clásico `ntpd`.

También se aprendió a verificar el estado de sincronización con herramientas de terminal (`timedatectl`, `ntpq`) y a interpretar sus salidas.

# Observaciones adicionales ⚠️

- Webmin no puede sincronizar la hora manualmente usando `sntp` sin permisos elevados, lo cual genera un error visual, pero **no afecta** el funcionamiento de `ntpd`.

- El servidor quedó sincronizado correctamente con varios servidores de tiempo.

---

### Autor

**César Eduardo Guerra Gil**  
1890-18-5953  
Universidad Mariano Gálvez de Guatemala  
[cguerrag2@miumg.edu.gt](mailto:cguerrag2@miumg.edu.gt)
