# BASH

**1) Что такое шебанг?**  
Шебанг — это специальная строка в начале скрипта, которая указывает операционной системе, какой интерпретатор использовать для выполнения данного скрипта. 

**2) Обязательно ли исполняемый файл дожен иметь соотвествующее расширение?**  
Нет, для исполняемого файла не требуется наличие расширения, если он имеет права на выполнение.  

**3) Напишите скрипт который выполнит автоматически действия из блока работы с файлами. ( не забудьте включить set -euo pipefail для того что бы ваш скрипт было удобнее отлаживать. Опишите что включают эти флаги)**  
- set -e: прерывает выполнение скрипта, если команда завершилась с ошибкой.
- set -u: генерирует ошибку при обращении к необъявленной переменной.
- set -o pipefail: обеспечивает возврат ошибки, если любая команда в пайплайне завершилась неудачно.

![image](https://github.com/user-attachments/assets/067074c0-71b8-4064-b657-493e2e3ef1ba)  
![image](https://github.com/user-attachments/assets/9d13bd36-c9d4-46cc-9b9a-e91321901da1)    

```
#!/bin/bash

# Настройка строгого режима для улучшения отладки
set -euo pipefail

# Проверка прав суперпользователя
if [[ $EUID -ne 0 ]]; then
  echo "Этот скрипт требует прав суперпользователя. Запустите его с sudo." >&2
  exit 1
fi

# Определяем переменные
DISK="/dev/sdb" # Имя нового диска (измените при необходимости)
MOUNT_POINT="/mnt/newdisk"
PARTITION="${DISK}1"

# Функция для вывода информации о блочных устройствах
function show_devices {
  echo "Информация о блочных устройствах:"
  lsblk
}

# Функция для создания раздела и файловой системы
function prepare_disk {
  echo "Создаем таблицу разделов и файловую систему на $DISK..."
  (
    echo o    # Новая таблица разделов
    echo n    # Новый раздел
    echo p    # Основной раздел
    echo 1    # Номер раздела
    echo      # Первый сектор (по умолчанию)
    echo      # Последний сектор (по умолчанию)
    echo w    # Запись изменений
  ) | fdisk "$DISK"

  partprobe "$DISK"

  if [[ ! -b "$PARTITION" ]]; then
    echo "Ошибка: Раздел $PARTITION не был создан." >&2
    exit 1
  fi

  mkfs.ext4 "$PARTITION"
}

# Функция для монтирования диска
function mount_disk {
  echo "Монтируем $PARTITION в $MOUNT_POINT..."
  mkdir -p "$MOUNT_POINT"
  mount "$PARTITION" "$MOUNT_POINT"
}

# Функция для создания тестовых файлов
function create_files {
  echo "Создаем файлы в $MOUNT_POINT..."
  touch "$MOUNT_POINT/file1.txt" "$MOUNT_POINT/file2.txt"
  ls -l "$MOUNT_POINT"
}

# Функция для добавления записи в /etc/fstab
function update_fstab {
  echo "Добавляем запись в /etc/fstab..."
  UUID=$(blkid -s UUID -o value "$PARTITION")
  if grep -q "UUID=$UUID" /etc/fstab; then
    echo "Запись для $UUID уже существует в /etc/fstab."
  else
    echo "UUID=$UUID $MOUNT_POINT ext4 defaults 0 2" >> /etc/fstab
  fi
}

# Функция для проверки /etc/fstab
function verify_fstab {
  echo "Проверяем /etc/fstab..."
  mount -a
  if mountpoint -q "$MOUNT_POINT"; then
    echo "Диск успешно смонтирован из /etc/fstab."
  else
    echo "Ошибка монтирования. Проверьте записи в /etc/fstab." >&2
    exit 1
  fi
}

# Основной блок выполнения
function main {
  show_devices
  prepare_disk
  mount_disk
  create_files
  umount "$MOUNT_POINT"
  update_fstab
  verify_fstab
  echo "Все шаги успешно выполнены!"
}

main
```
