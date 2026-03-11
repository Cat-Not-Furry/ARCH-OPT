# ARCH-OPT

[![License](https://img.shields.io/badge/license-GPLv3-blue)](LICENSE)
[![Arch Linux](https://img.shields.io/badge/Arch_Linux-1793D1?logo=arch-linux&logoColor=fff)](https://archlinux.org/)
[![Bash](https://img.shields.io/badge/Bash-4EAA25?logo=gnu-bash&logoColor=fff)](https://www.gnu.org/software/bash/)
[![Mantenido?](https://img.shields.io/badge/Mantenido%3F-¡Sí!-green.svg)](https://github.com/Cat-Not-Furry/ARCH-OPT)


Una colección de scripts Bash para darle nueva vida a laptops de dos décadas de antigüedad, con especial cariño a un VAIO del 2005.<br> En este repositorio subiré configuraciones scripts para mejorar el rendimiento en laptops de dos décadas de antiguedad, con documentación  lo más detallada posible.  

> *“¡Por el poder de Torvalds!”* – CNF

## Descripción

**ARCH-OPT** nace de la necesidad de tener un control fino y rápido sobre el rendimiento de un equipo antiguo corriendo Arch Linux. En lugar de recordar largos comandos o andar rebuscando en menús, estos scripts te permiten:

- **Gestionar la frecuencia de la CPU** con solo `+`, `=` o `-`.
- **Optimizar la batería** mediante perfiles TLP y monitoreo instantáneo.
- **Poner fondos de pantalla** aleatorios o elegir uno de una lista interactiva.
- **Configurar monitores externos** con opciones cortas: `p` (portátil), `d` (duplicar), `ex` (extender), `s` (solo externa).
- **Ver temperatura y frecuencia** con colores según el nivel.
- **Poner videos como fondo de pantalla** (por ahora exclusivo de entornos i3).

Todo pensado para ser **rápido, legible y divertido** (sí, hay mensajes con :v y referencias a Windows).

## Características

- **Scripts modulares**: cada uno hace una cosa y la hace bien.
- **Validación de dependencias**: si falta algo, te avisa y te dice cómo instalarlo.
- **Mensajes con colores e iconos** (cuando el terminal lo soporta).
- **Modo interactivo y modo directo** en varios scripts (ej. `fondo s` te muestra una lista y luego eliges número, o `fondo s 3` va directo).
- **Soporte para dos carpetas de fondos**: la principal (`la puede configurar el usuario`) y una secundaria (`carpeta secundaria`), para tener separados los wallpapers "normales" de los "experimentales".
- **Diseñado para hardware antiguo**: valores por defecto conservadores, incrementos pequeños de brillo (2%), governors apropiados, etc.

> [!WARNING]
> **Cabe aclarar que yo uso diariamente estos comandos y por lo tanto puedo dar fe que usarlos de una forma consciente aseguró el mayor rendimiento posible en mi VAIO del 2005 y me ayuda con algunas tareas en mi laptop Thinkpad, sin embargo tómese el tiempo de leer y adaptar el código a su conveniencia si ese es el caso, ya que usted como usuario lo ejecuta bajo su propio riesgo. Aunque repito no me ha dado ningún problema ningún comando que he creado.**

> [!IMPORTANT]
> **Son scripts programados en `BASH`, pero por conveniencia les dire comandos. \
> Asi mismo estas optimizaciones no estan destinadas a la automatización por obvios motivos.**

> [!NOTE]
> **Por el momento los scripts solo funcionaran en `Arch Linux`, después creare otros repositorios. \
> Asegurate de tener las siguientes herramientas para poder ejecutar 5/7 scripts que las necesitan. \
> Asi mismo le recomiendo actualizar su sistema antes de instalar.**

```bash
sudo pacman -Syu
```

**Para configurar el la frecuencia del procesador y ver la frecuencia del mismo:**

```bash
sudo pacman -S cpupower
```
**No es necesario pero si recomendable instalar:**
> **CPU `INTEL`**

```bash
sudo pacman -S intel-ucode
```
> **CPU `AMD`**

```bash
sudo pacman -S amd-ucode
```
**Para subir y bajar el brillo desde terminal:<br> En X11 y Wayland<br> brightnessctl (funciona con Intel/AMD)**

```bash
sudo pacman -S brightnessctl
```
**Ver el porcentaje de la bateria<br> acpi (uso sencillo en terminal)**

```bash
sudo pacman -S acpi
```

**Ver la temperatura del CPU**

```bash
sudo pacman -S lm_sensors bc
```
> [!TIP]
> **Recomiendo instalar `tlp` para optimizar la energia**

```bash
sudo pacman -S tlp
```

**Todos juntos**

```bash
# Básicas para la mayoría de scripts
sudo pacman -S --needed brightnessctl cpupower feh lm_sensors xorg-xrandr bc acpi

# Para fondos animados (life_fondo)
sudo pacman -S --needed mpv jq xdotool
# xwinwrap-git está en AUR, usa tu helper favorito
yay -S xwinwrap-git

# Para transparencias (opción t en fondo)
sudo pacman -S picom

# Para gestión de energía avanzada (carga)
sudo pacman -S tlp
```
## Instalación
**a. Descargar el zip, extraerlo y mover los archivos a /urs/local/bin (para no tener conflictos de niguna indole).**

```bash
sudo mv * /usr/local/bin
```
**b. Descargar usando git.**

```ella
git clone https://github.com/Cat-Not-Furry/ARCH-OPT.git
```
```bash
cd ARCH-OPT-main
chmod +x *
sudo mv * /usr/local/bin
```

## Descripciones de que hacen.
# ?
**Por si se te olvida el nombre del comando que deseabas invocar, (que no se te olvide este), si usas Routers Cisco ya estaras familiarizado con este simbolo aun asi asegurate de que no tengas algun otro comando con esa nomenclatura, de lo contrario otro buen nombre seria...**

```bash
mv ? -help
```
>**O**

```bash
mv ? scripts-help
```
**Mostrara un tabla con la descripción de lo que hacen los demás comandos.**

# fondo
**Establece un fondo para tu WM en X11 de confianza con feh<br> Puedes cambiar los directorios por defecto mediante variables de entorno:**

```bash
# En tu .bashrc o .zshrc
export DIR="$HOME/mis_fondos"
export OTHER_DIR="$HOME/mis_fondos/otra_carpeta"
```
Los scripts que usan fondos (fondo y life_fondo) respetan estas variables.

**O dentro del código**

```bash
xdg-open fondo life_fondo
```

# bateria
**Con este comando podras ver la bateria que tienes, (muy practico si me lo preguntas).**

# frecuencia
**Muestra la frecuencia actual del CPU.**

# temperatura
**Muestra la temperatura actual del CPU.**

# brillo
**Con este comando puedes bajar y subir el brillo desde la terminal por si estas usando una sesion tty o (por si eres un rarito como yo que no quiere mover el mouse para nada).**

# cpu-mode
**Con este comando puedes ajustar la frecuencia de tu procesador, por defecto no es necesario mover nada a menos que quieras una frecuencia en especifico**

> [!WARNING]
> **Si usas mucho `cpu-mode +` procura hacerle mantenimiento preventivo más frecuentemente, debido al sobrecalentamiento por usar la frecuencia máxima.**

# actualizar
**Con este comando actualizas el sistema operativo, para mayor seguridad necesitas confirmar la descarga de los paquetes.**

# ¿Contribuir?
**¿Quieres mejorar algo? ¿Añadir un script nuevo?<br> No soy como el de Yandere Simulator :V<br> Haz un fork del repositorio.<br> Crea una rama (git checkout -b feature/mi-mejora).<br> Haz tus cambios (con comentarios, porfa).<br> Haz commit (git commit -m 'Añadida funcionalidad X').<br> Sube la rama (git push origin feature/mi-mejora).<br> Abre un Pull Request.<br> Mantén el estilo existente: bash con set -euo pipefail, validación de dependencias y, si puedes, añade algún mensaje divertido ;)<br> Licencia<br> GPLv3 License – Haz lo que quieras, pero si lo rompes, te quedas los pedazos.<br><br> Cat-Not-Furry<br> GitHub: @Cat-Not-Furry<br> Este proyecto nació por aburrimiento y porque Windows 10 no corría la malportada.<br><br> Agradecimientos:<br> A Torvalds por Linux.<br> A la comunidad de Arch por su documentación infinita.<br> A los creadores de cpupower, feh, picom, tlp, brightnessctl, xwinwrap, etc.<br> Y a ti, por leer hasta aquí. ¡Ahora pon un fondo aca bien chido con `fondo f`o uno bien jocoso con `fondo p`<br> Creditos a DeepSeek y a ChatGPT, por ayudarme a optimizar mi laptop.<br/> https://www.deepseek.com/ <br> https://chatgpt.com/.**
