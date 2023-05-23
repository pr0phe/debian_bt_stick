USB REALTEK RTL8671B bluetooth stick Debian install <br>

Установка драйвера для USB Bluetooth в Debian 11 <br>
Версия работает для стика https://a.aliexpress.com/_DlcRgQh <br>

Чип: REALTEK RTL8671B <br>

Источник: https://debian.pkgs.org/11/debian-nonfree-arm64/firmware-realtek_20210315-3_all.deb.html <br>
Binary Package	http://ftp.de.debian.org/debian/pool/non-free/f/firmware-nonfree/firmware-realtek_20210315-3_all.deb <br>
Source Package	firmware-nonfree <br>
Mirror          ftp.de.debian.org <br>

1. Установка <br>
Скачать пакет и установить можно двумя способами: <br>

* Скачать сам пакет <br>
```
wget http://ftp.de.debian.org/debian/pool/non-free/f/firmware-nonfree/firmware-realtek_20210315-3_all.deb
sudo apt install -y /путь к папке/firmware-realtek_20210315-3_all.deb
```
Если скачали firmware-realtek_20210315-3_all.deb в папку root, то вводим так <br>
```
sudo apt install -y ~/firmware-realtek_20210315-3_all.deb
```

* Или добавить в sources.list ссылку, обновить и установить пакет. <br>
Открываем sources.list <br>
```
sudo nano /etc/apt/sources.list
```

Добавляем ссылки <br>
```
deb http://ftp.de.debian.org/debian stable main <br>
deb http://ftp.de.debian.org/debian-security stable/updates main
```

Обновить пакеты
```
sudo apt-get update
```

Установить прошивку firmware-realtek
```
sudo apt-get install firmware-realtek
```

2. Проверяем работостпособность:

Вывести информацию о доступных bluetooth адаптеров
sudo dmesg | grep -i bluetooth

Если в спсике будет что-то вроде этого, значит драйвер не установлен
bluetooth hci0: Direct firmware load for rtl_bt/rtl8761b_fw.bin failed with error -2
Bluetooth: hci0: RTL: firmware file rtl_bt/rtl8761b_fw.bin not found

Чтобы исправить это, нужно сделать следующее
cd /tmp
wget https://raw.githubusercontent.com/Realtek-OpenSource/android_hardware_realtek/rtk1395/bt/rtkbt/Firmware/BT/rtl8761b_config
wget https://raw.githubusercontent.com/Realtek-OpenSource/android_hardware_realtek/rtk1395/bt/rtkbt/Firmware/BT/rtl8761b_fw
mv rtl8761b_config /lib/firmware/rtl_bt/rtl8761b_config.bin
mv rtl8761b_fw /lib/firmware/rtl_bt/rtl8761b_fw.bin
sudo modprobe btusb
sudo systemctl start bluetooth.service

Перезагружаем Linux
sudo systemctl reboot

Повторно выводим информацию о доступных bluetooth адаптеров, где не должно быть failed with error -2 или not found
sudo dmesg | grep -i bluetooth

Вот этого не должно уже быть. Есди вы это не видите, значит драйвер установлен и USB bluetooth готов к работе
bluetooth hci0: Direct firmware load for rtl_bt/rtl8761b_fw.bin failed with error -2
Bluetooth: hci0: RTL: firmware file rtl_bt/rtl8761b_fw.bin not found

3. Дополнительная информация
Вывести информацию о доступных bluetooth адаптеров
sudo dmesg | grep -i bluetooth

Команда выводит MAC-адрес вашего Bluetooth адаптера и его версию. Если вам нужна только версия протокола, которую поддерживает Bluetooth вашего компьюютера, то используйте команду:
btmgmt info | awk 'BEGIN{split("1.0b 1.1 1.2 2.0 2.1 3.0 4.0 4.1 4.2 5.0 5.1 5.2 5.3",i," ")}$1=="addr"{print $2"\tBluetooth: V"i[$4+1]}'

Узнать только мак адрес bluetooth
Вариант 1
hcitool dev | grep -o "[[:xdigit:]:]\{11,17\}"
Вариант 2
hcitool dev | cut -sf3


Выводим список имеющихся bluetooth
hciconfig -a

Original by https://gist.github.com/DivanX10/7c6ca3f325dfd853b38c04da9dce28a6
