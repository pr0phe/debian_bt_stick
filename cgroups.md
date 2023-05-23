# How-to man to switch cgroup to v1 for Home Assistant

Довольно актуальным оказался вопрос с которым я столкнулся при попытке поставить HA Supervised на Debian 11. <br>
HA ругается и говорит что все ему не так, в том числе на cgoups, при чуть более глубоком поиске ответ ищется моментально, но я решил собирать все полезные инструкции в одном месте поэтому это тоже будет тут.

Опасность! :) <br>
Changing cgroup version <br>
Changing cgroup version requires rebooting the entire system. <br>

Теория: <br>
On systemd-based systems, cgroup v2 can be enabled by adding systemd.unified_cgroup_hierarchy=1 to the kernel cmdline. To revert the cgroup version to v1, you need to set systemd.unified_cgroup_hierarchy=0 instead.

Практика в 3 шага:

1. Открываем в редакторе файл груб
```
sudo nano /etc/default/grub
```

2. Ищем 
```
GRUB_CMDLINE_LINUX_DEFAULT="systemd.unified_cgroup_hierarchy=1"
```
меняем на 
```
GRUB_CMDLINE_LINUX_DEFAULT="systemd.unified_cgroup_hierarchy=0"
```
на самом деле там вместо единички в конце можетбыть что-то другое, но нам нужна именно первая версия, которая обозначатеся 0.

3. Обновляем
```
sudo update-grub
sudo reboot
```

Наслаждаемся.

