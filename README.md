# Sniffer de red en Python
 
Analizador de paquetes con interfaz gráfica (Tkinter) construido sobre Scapy.
Captura tráfico en vivo, lo muestra en una tabla y permite inspeccionar cada paquete por capas.
 
**Protocolos que analiza:** Ethernet, IPv4, IPv6, ARP, ICMPv4, ICMPv6, TCP, UDP, DNS, DHCP, HTTP y NFS.
 
> HTTPS no se puede interpretar porque el contenido va cifrado. Solo se analiza HTTP en texto plano.
 
## Archivo principal
 
```
sniffer_final_2.0_icmp.py
```
 
El resto de los archivos `.py` del repositorio son versiones anteriores del desarrollo (1.0, 1.2, 1.3, GUI, beta, 2.3, 2.4, etc.).
 
## Requisitos
 
| Requisito | Windows | Linux |
|---|---|---|
| Python 3.9 o superior | Sí | Sí |
| Scapy | Sí | Sí |
| psutil | Sí | Sí |
| Tkinter | Incluido con Python | Se instala aparte |
| Captura de paquetes | **Npcap** | libpcap (suele venir instalada) |
| Permisos | Administrador | `sudo` / root |
 
`netifaces` es opcional y no hace falta instalarlo.
 
---
 
## Windows
 
### 1. Instalar Python
 
Descárgalo de <https://www.python.org/downloads/> y durante la instalación marca **"Add Python to PATH"**. Comprueba la instalación:
 
```powershell
python --version
```
 
### 2. Instalar Npcap (obligatorio)
 
Descárgalo de <https://npcap.com/> e instálalo dejando marcada la opción **"Install Npcap in WinPcap API-compatible Mode"**. Sin Npcap el programa no captura nada.
 
### 3. Instalar las librerías
 
```powershell
pip install scapy psutil
```
 
### 4. Ejecutar como administrador
 
Abre PowerShell con clic derecho, **Ejecutar como administrador**, entra a la carpeta del proyecto y corre:
 
```powershell
cd C:\ruta\a\Sniffer_redes-main
python sniffer_final_2.0_icmp.py
```
 
Si usas VS Code, ábrelo también como administrador.
 
### Notas para Windows
 
- El tráfico de loopback (`127.0.0.1`) normalmente no se ve con Npcap.
- Si no aparecen paquetes, revisa que en el selector de tarjeta de red esté elegida la que usas realmente (Wi-Fi o Ethernet), no un adaptador virtual o de VPN.
---
 
## Linux (Ubuntu / Debian y derivadas)
 
### 1. Instalar Python, Tkinter y las librerías
 
Opción recomendada, con paquetes del sistema (funciona bien con `sudo`):
 
```bash
sudo apt update
sudo apt install python3 python3-tk python3-scapy python3-psutil
```
 
Si prefieres `pip`, en Ubuntu 23.04 o posterior el comando `pip install` directo da el error `externally-managed-environment`. Usa un entorno virtual:
 
```bash
sudo apt install python3 python3-venv python3-tk
python3 -m venv venv
source venv/bin/activate
pip install scapy psutil
```
 
### 2. Ejecutar con permisos de superusuario
 
La captura de paquetes requiere root.
 
Con los paquetes de `apt`:
 
```bash
cd /ruta/a/Sniffer_redes-main
sudo python3 sniffer_final_2.0_icmp.py
```
 
Con entorno virtual (`sudo` debe usar el Python del entorno):
 
```bash
sudo ./venv/bin/python sniffer_final_2.0_icmp.py
```
 
### Notas para Linux
 
- Si aparece el error `no display name and no $DISPLAY environment variable` o `couldn't connect to display`, `sudo` no está pasando la sesión gráfica. Prueba:
```bash
sudo -E python3 sniffer_final_2.0_icmp.py
```
 
  Si sigue fallando (común en Wayland), da permiso al usuario root para abrir ventanas y vuelve a intentar:
 
```bash
xhost +si:localuser:root
```
 
- A diferencia de Windows, en Linux sí se ve el tráfico de loopback (`lo`).
---
 
## Uso
 
1. Elige la tarjeta de red en el selector.
2. Pulsa el botón de iniciar captura y genera tráfico (abre una página, haz `ping 8.8.8.8`, etc.).
3. Usa el filtro para quedarte con un protocolo.
4. Haz doble clic en una fila para ver el análisis por capas y la explicación de cada campo.
5. Pulsa detener cuando termines.
---
 
## Ejecutables ya compilados (opcional)
 
Si solo quieres usar el programa sin instalar Python, hay versiones compiladas con PyInstaller. Los enlaces de descarga están en `enlace de descarga.md`.
 
### Windows
 
Descomprime el `.rar`, ejecuta el `.exe` como administrador. Npcap sigue siendo necesario.
 
### Linux
 
El ejecutable se generó con PyInstaller en modo `--onedir`, por lo que la estructura es:
 
```
Sniffer_Final_Linux/
├── Sniffer_Final_Linux      (ejecutable)
└── _internal/
```
 
No intentes ejecutar la carpeta. Entra a ella y ejecuta el binario:
 
```bash
cd /ruta/a/Sniffer_Final_Linux
chmod +x Sniffer_Final_Linux
sudo ./Sniffer_Final_Linux
```
 
Si aparece `-bash: ./Sniffer_Final_Linux: Es un directorio`, significa que se intentó ejecutar la carpeta en lugar del binario interno.
 
> Los ejecutables compilados corresponden a una versión anterior y no incluyen el detalle ICMP añadido en `sniffer_final_2.0_icmp.py`.
 
---
 
## Problemas comunes
 
| Síntoma | Causa probable | Solución |
|---|---|---|
| `No libpcap provider available` (Windows) | Npcap no instalado o sin modo WinPcap | Reinstalar Npcap con la opción de compatibilidad |
| `ModuleNotFoundError: No module named 'psutil'` | Falta una librería | `pip install psutil` |
| `ModuleNotFoundError: No module named 'tkinter'` (Linux) | Falta Tkinter | `sudo apt install python3-tk` |
| `Operation not permitted` o `Permission denied` | Sin permisos de captura | Ejecutar como administrador o con `sudo` |
| La tabla se queda vacía | Tarjeta de red equivocada | Elegir otra en el selector |
| `python` no se reconoce (Windows) | Python no está en el PATH | Reinstalar marcando "Add Python to PATH" |
 
