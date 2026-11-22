# Diseño e Implementación de un Servidor Linux para Cocina Repuestos SAC

Proyecto académico del curso de **Sistemas Operativos** (Carrera de Ingeniería de Sistemas — Universidad Tecnológica del Perú, Sede Sur). El proyecto aborda la centralización de archivos, control de accesos por áreas (**Ventas** y **Almacén**), gestión de recursos y disponibilidad de información para la pyme **Cocina Repuestos SAC** en Lima, Perú.

## Integrantes y Datos del Curso

* **Curso:** Sistemas Operativos
* **Docente:** Espinal Teves, José Antonio
* **Integrantes:**
  * Palomares Dueñas, Israel Adrián (U18302252)
  * De La Rosa Castilla, Maycol Bryan (U24261083)
* **Ciclo / Año:** V Ciclo — 2026

---

## Evolución del Proyecto

### 1. Avance 1 — Prototipo en Máquina Virtual (Ubuntu Linux)
* **Entorno:** Máquina virtual en **VMware Workstation Pro 17** con **Ubuntu 26.04.1 LTS**, 4000 MB de RAM asignados, disco virtual de 32 GB y 2 procesadores.
* **Usuarios, grupos y carpetas:** Creación de grupos (`ventas_rep`, `almacen_rep`), usuarios (`cajero_israel`, `almacenero_maycol`, `admin_tienda`) y estructura inicial bajo `/srv/cocina_repuestos/{ventas,almacen,catalogo}`.
* **Monitoreo, procesos y memoria:** Uso de comandos básicos (`ls`, `cat`, `more`, `less`, `tee`), gestión de procesos (`ps`, `top`, `renice`, `nohup`, `kill`), análisis de memoria (`vmstat`, `free -h`, `swapon`/`swapoff`) y marco teórico del algoritmo **Round Robin**.
* **Sistemas de archivos:** Simulación de disco y sistema de archivos `ext4` mediante `dd`, `mkfs.ext4`, `mount -o loop`, `df` y `du`.

### 2. Avance 2 y Proyecto Final — Servidor Físico sobre Arduino UNO Q (Debian Linux)
* **Migración a hardware dedicado:** Implementación física en placa **Arduino UNO Q** (procesador Qualcomm Dragonwing de 4 núcleos ARM a 2,0 GHz, microcontrolador STM32, 4 GB LPDDR4 de RAM, 32 GB de almacenamiento eMMC, Wi-Fi 5 y conectividad vía Hub USB-C) con **Debian Linux** (`aarch64`).
* **Gestión de archivos, usuarios y permisos:** Estructura en `/srv/servidor_tienda` (`ventas`, `almacen`, `comun`), grupos `ventas_rep` y `almacen_rep`, usuarios `cajero_israel`, `almacenero_adrian` y `admin_tienda` con permisos `770`.
* **Archivos compartidos en red local (Samba):** Recursos compartidos `Ventas_Repuestos` y `Almacen_Repuestos` accesibles desde equipos Windows en la red local.
* **Montaje, desmontaje y E/S:** Configuración de puntos de montaje en `/mnt/disco_usb` y monitoreo de almacenamiento (`df -h`), memoria (`free -h`) y carga (`top`), cuidando los ciclos de escritura de la memoria eMMC.
* **Contenedores:** Instalación y verificación de **Docker** (`docker.io`) en arquitectura `arm64`.
* **Medidas de seguridad (Proyecto Final):**
  * Firewall con **nftables** (`policy drop` por defecto en entrada, permitiendo tráfico local hacia SSH puerto `22` y Samba `137-139`, `445`).
  * Hardening de **SSH** (`PermitRootLogin no`, `MaxAuthTries 3`) y **sshguard** contra intentos repetidos de acceso.
  * Endurecimiento de **Samba** (`server min protocol = SMB3_02`, `map to guest = Never`, `restrict anonymous = 2` y máscaras `0660` / `0770`).
  * Análisis antimalware programado con **ClamAV** (con `clamav-daemon` deshabilitado y tareas programadas vía `cron` en horario no laboral para cuidar RAM y ciclos de escritura en eMMC).

---

## Estructura Sugerida del Repositorio

```text
├── README.md
├── docs/
│   ├── Avance 1.pdf
│   ├── Avance 2.pdf
│   └── Proyecto Final.docx
└── evidencias/
    ├── Servidor Funcional.mp4
    └── Implementando Seguridad.mp4