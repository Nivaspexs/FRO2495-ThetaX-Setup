##  Instalación en un Nuevo PC (Paso a Paso)

Sigue estos pasos en orden para compilar los drivers y usar la cámara en un ordenador nuevo. Probado en sistemas basados en Debian/Ubuntu.

### 1. Instalar Dependencias Previas
Abre una terminal y asegúrate de tener los compiladores, cabeceras del kernel actual y GStreamer con todos sus plugins (buenos, malos y feos).

```bash
sudo apt update
sudo apt install -y build-essential cmake pkg-config libusb-1.0-0-dev \
libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-good \
gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly linux-headers-$(uname -r)
```

### 2. Compilar el Módulo Virtual (`v4l2loopback`)
Necesitamos el módulo que va a "fingir" ser una cámara web para Google Meet.

```bash
cd v4l2loopback
make
sudo make install
sudo depmod -a
cd ..
```

> *Tip: En un futuro, al reiniciar tu PC, puedes cargar el dispositivo virtual ejecutando:*
> `sudo modprobe v4l2loopback exclusive_caps=1 card_label="Ricoh THETA"`

### 3. Compilar la Librería Base (`libuvc-theta`)
Esta es la librería encargada de comunicarse con la cámara a bajo nivel mediante USB.

```bash
cd libuvc-theta
mkdir -p build && cd build
cmake ..
make
cd ../..
```

### 4. Compilar la Aplicación de Streaming (`gst_loopback`)
Este programa enlaza la librería UVC con el módulo de kernel a través de un *pipeline* optimizado de GStreamer.

```bash
cd libuvc-theta-sample/gst
make
cd ../..
```

---
