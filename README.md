# Bluetooth RoboSoccer PUCV

A continuacion se hara una repaso de varias herramientas para administrar dispositivos bluetooth desde Ubuntu 20.04.

## administracion de bluetooth

el primer paso para poder administrar correctamente dispositivos BLE debemos tener los controladores de los adaptadores Bluetooth.
```
sudo apt install bluez*
```
desde aqui se pueden utilizar varias alternativas de administracion, una de ellas es por consola mediante el comando `bluetoothctl`.
Aunque la mejor alternativa es utilizar el administrador blueman, el cual nos una interfaz que nos permite la gestion de dispositivos BLE.
```
sudo apt install blueman
```
Luego nos aseguramos en habilitar el servicio, esto deberia hacerse solo 1 vez, pero en etapas de desarrollo se ha encontrado la necesidad de realizar esto en repetidas veces.
```
modprobe btusb
sudo systemctl enable bluetooth.service
sudo systemctl start bluetooth.service
```

## desarrollo de software

hasta el momento de elaboracion de esta guia, se han estudiado 3 maneras de desarrollar software capaz de utilizar bluetooth LE.
Todo lo explicado a continuacion es realizado en Python3, la ultima version estable es suficiente.

primero se tiene la herramienta Dbus, con esta herramienta se tiene control sobre los procesos de comunicacion que suceden, en este caso, en los adaptadores Bluetooth, con esta alternativa se pueden desarrollar las aplicaciones mas eficientes pero es dificil de ocupar. Esto es el resultado del trabajo de la comunidad Bluetooth internacional, existen cursos disponibles ademas de una guia de estudio para el desarrollo de tecnologias Bluetooth para desarrolldores en linux.

Enlaces importantes:
* https://www.bluetooth.com
* https://www.bluetooth.com/bluetooth-resources/bluetooth-for-linux/

La segunda herramienta es la utilizacion de la API BlueZ, para utilizar esta API en nuetro codigo debemos tener Python3 y pip instalados en nuetro equipo, si se descarga desde la pagina oficial no deberia existir problema, luego instalamos la API.
```
pip install PyBluez
```

con esto ya se pueden realizar ejemplo basicos.
```
#escaneo simple
import bluetooth

nearby_devices = bluetooth.discover_devices(lookup_names=True)
print("Found {} devices.".format(len(nearby_devices)))

for addr, name in nearby_devices:
    print("  {} - {}".format(addr, name))
```

Enlaces importantes:
* https://pybluez.readthedocs.io/en/latest/
* https://github.com/pybluez/pybluez

Posibles Errores:

Puede que salga un error con los headers de BlueZ, para solucionar este problema, debemos ejecutar los siguientes comandos.
```
sudo apt-get install libbluetooth-dev
sudo python3 setup.py install
```

luego debemos ejecutar un archivo en python con `import bluetooth`

Otra alternativa es utilizar comunicacion serial en bluetooth BLE para ello debemos utilizar BLE-serial.

```
pip install ble-serial
```

Enlaces importantes:
* https://github.com/Jakeler/ble-serial
* https://pypi.org/project/ble-serial/
* https://forums.raspberrypi.com/viewtopic.php?t=215331
