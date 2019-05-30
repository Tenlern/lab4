# Отчет по лабораторной работе №4

### Выполнил 

Cтудент | группа
--------|-------
Ляшенко Илья Александрович | БВБО-01-16

Процесс выполнения:

### Задание 1.

1. В VirtualBox была создана виртуальная машина с характеристиками
Ос | Linux Debian 9.8.0
ОП | 1 гб
ssd1 | vmdk 5 гб
ssd2 | vmdk 5 гб
2. Была произведена установка ОС на визуальную машину по инструкции лабораторной работ. Были создан зеркальный raid с тремя логическими дисками root, var, log в группе system.
!(https://github.com/Tenlern/lapk4/screenshots/part.png "Разметка")
!(https://github.com/Tenlern/lapk4/screenshots/start_lsblk.png "Распределение памяти на начало работы")
!(https://github.com/Tenlern/lapk4/screenshots/stat.png "Результаты cat /proc/mdstat,pvs, vgs, pvs")

### Задание 2.

1. Был удален ssd1 и заменен на ssd3 того же размера.
!(https://github.com/Tenlern/lapk4/screenshots/new_disk.png "/proc/mdstat на момент отказа ssd1")
2. После на новый накопитель была скопирована разметка из ssd2 и раздел sda2 был добавлен в рейд массив
!(https://github.com/Tenlern/lapk4/screenshots/sync.png "Процесс добавление раздела sda2")
3. Были скопированы данные с загрузочного раздела sdb1 в sda1 и установлен grub на новый диск.
В итоге, мы перенесли таблицу разделов с рабочего диска на новый, добавили его в существующий рейд и провели синхронизацию, после чего настроили загрузочный раздел на новый накопитель.

### Задание 3.

1. Была произведена эмуляция отказа ssd2.
!(https://github.com/Tenlern/lapk4/screenshots/refuse_ssd2.png "Состояние системы на момент отказа")
Был добавлен ssd4 объемом 6 гб,
!(https://github.com/Tenlern/lapk4/screenshots/ssd4.png)
Перенесена разметка c ssd3,
!(https://github.com/Tenlern/lapk4/screenshots/ssd4part.png "Состояние дисков после переноса")
был скопирован /boot и установлен grub.
После этих действий был создан новый рейд массив
!(https://github.com/Tenlern/lapk4/screenshots/new_array.png "Состояние дисков после создания нового рейда")
2. Был создан физический том рейд массива для дальнейшей работы с логическими томами и включен в группу system
!(https://github.com/Tenlern/lapk4/screenshots/pvcreate.png)
!(https://github.com/Tenlern/lapk4/screenshots/logical.png "Результат vgdisplay system -v, pvs, vgs, lvs -a -o+devices")
3. Были перенесены логические тома со старого рейда md0 на новый md63
!(https://github.com/Tenlern/lapk4/screenshots/new_stat.png)
4. Удален ssd3 и добавлены ssd5 объемом 6гб, hdd1 и hdd2 объемом 12гб. Повторена операция по переносу разделов и копированию /boot на новый ssd5.
5. Был увеличен объем второго раздела при помощи утилиты fdisk,
!(https://github.com/Tenlern/lapk4/screenshots/extended_sdb2.png)
после чего раздел добавлен в существующий рейд.
!(https://github.com/Tenlern/lapk4/screenshots/new_raid_device.png)
6. Также был расщирен второй раздел ssd4 и увеличен максимальный размер рейда и логических томов root и var
!(https://github.com/Tenlern/lapk4/screenshots/extend.png "Расширение логических томов")
7. Был создан новый рейд на основе hdd1 и hdd2, где была создана группа  data и логический том var_log
!(https://github.com/Tenlern/lapk4/screenshots/new_log.png "Новый рейд и логический том")
8. Была произведена синхронизация разделов system-log и data-var_log
9. Перемонтирован /var/log на data-var_log 
!(https://github.com/Tenlern/lapk4/screenshots/log.png)
Финальная разметка дисков 
!(https://github.com/Tenlern/lapk4/screenshots/statf.png)