<h1>VAIO-OPT</h1>
<h2>En este repositorio subire configuraciones generales para mejorar el rendimiento en laptops de dos decadas de antiguedad, con documentación  lo más detallada posible.</h2>
<br/>

> [!IMPORTANT]
> **Son scripts programados en `BASH`, pero por conveniencia les dire comandos. \
> Asi mismo estas optimizaciones no estan destinadas a la automatización por obvios motivos.**

> [!NOTE]
> **Por el momento los scripts solo funcionaran en `Arch Linux`, despues creare otros repositorios. \
> Asegurate de tener las siguientes herramientas para poder ejecutar 3/5 scripts que las necesitan. \
> Asi mismo le recomiendo actualizar su sistema antes de instalar.**

```ella
sudo pacman -Syu
```
<br/>
<h2>Para configurar el la frecuencia del procesador:</h2>

```ella
sudo pacman -S cpupower
```
> **No es necesario pero si recomendable instalar:** \
> **CPU `INTEL`**

```ella
sudo pacman -S intel-ucode
```
> **CPU `AMD`**

```ella
sudo pacman -S amd-ucode
```
<h2>Para subir y bajar el brillo desde terminal:</h2>
<h3>En X11 y Wayland</h3>

> **brightnessctl (funciona con Intel/AMD)**

```ella
sudo pacman -S brightnessctl
```
<h2>Ver el porcentaje de la bateria</h2>
<h3>acpi (uso sencillo en terminal)</h3>

```ella
sudo pacman -S acpi
```
> [!TIP]
> **Recomiendo instalar `tlp` para optimizar la energia**

```ella
sudo pacman -S tlp
```
<br/>
<br/>
<h2>Instalación</h2>
<h3>a. Descargar el zip, extraerlo y mover los archivos a /urs/local/bin (para no tener conflictos de niguna indole).</h3>

```ella
sudo mv * /usr/local/bin
```
<h3>b. Descargar usando git.</h3>

```ella
git clone https://github.com/Cat-Not-Furry/VAIO-OPT.git
```
```ella
cd VAIO-OPT
```
```ella
chmod +x *
```
```ella
sudo mv * /usr/local/bin
```
<br/>
<h2>Descripciones de que hacen.</h2>
<h3>?</h3>
<h4>Por si se te olvida el nombre del comando que deseabas invocar, (que no se te olvide este), \
si usas Routers Cisco ya estaras familiarizado con este simbolo aun asi asegurate de que no tengas algun otro comando con esa nomenclatura, de lo contrario otro buen nombre seria...</h4>

```ella
mv ? -help
```
> **O**

```ella
mv ? scripts-help
```
<h4>Mostrara un tabla con la descripción de lo que hacen los demás comandos.</h4>
<br/>
<h3>bateria</h3>
<br/>
<h4>Con este comando podras ver la bateria que tienes, muy (practico si me lo preguntas).</h4>
<h3>brillo</h3>
<br/>
<h4>Con este comando puedes bajar y subir el brillo desde la terminal por si estas usando una sesion tty o (por si eres un rarito como yo que no quiere mover el mouse para nada).</h4>
<h3>cpu-mode</h3>
<br/>
<h4>Con este comando puedes ajustar la frecuencia de tu procesador, por defecto no es necesario mover nada a menosque quieras una frecuencia en especifico</h4>

> [!WARNING]
> **Si usas mucho `cpu-mode +` procura hacerle manteniminto preventivo más frecuentemente, debido al sobrecalentamiento por usar la frecuencia maxima.**
<h2>update-arch</h2>
<br/>
<h4>Con este comando actualizas el sistema operativo, para mayor seguridad necesitas confirmar la descarga de los paquetes.</h4>
<br/>
Creditos a DeepSeek y a ChatGPT, por ayudarme a optimizar mi laptop.<br/>
https://www.deepseek.com/<br/>
https://chatgpt.com/.
