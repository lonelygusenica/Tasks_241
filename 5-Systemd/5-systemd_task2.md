# Systemd

**1) Создайте скрипт который создаёт папку заполняет её файлами ( имена 1-4 ) и записывает в них информацию о текущей дате, версии ядра, имени компьютера и списе всех файлов в домашнем каталоге пользователя от которого выполняется скрипт( не забудьте сдлеать проверку на существование файлов и папок)**  
```
#!/bin/bash

# Определение домашнего каталога пользователя
HOME_DIR=$(getent passwd "$(whoami)" | cut -d: -f6)

# Установка рабочей директории
cd "$HOME_DIR" || { echo "Не удалось перейти в домашний каталог"; exit 1; }

# Имя создаваемой директории
TARGET_DIR="info_storage"

# Проверка существования директории, создание при необходимости
if [ ! -d "$TARGET_DIR" ]; then
    mkdir -p "$TARGET_DIR"
    echo "Создана директория: $TARGET_DIR"
else
    echo "Директория $TARGET_DIR уже существует"
fi

# Переход в целевую директорию
cd "$TARGET_DIR" || { echo "Не удалось перейти в директорию $TARGET_DIR"; exit 1; }

# Создание и заполнение файлов
for FILE_NUM in {1..4}; do
    FILE_NAME="file_$FILE_NUM.txt"
    
    if [ ! -f "$FILE_NAME" ]; then
        {
            echo "Дата: $(date)"
            echo "Версия ядра: $(uname -r)"
            echo "Имя компьютера: $(hostname)"
            echo "Файлы в домашней директории:"
            ls -1 "$HOME_DIR"
        } > "$FILE_NAME"
        echo "Создан файл: $FILE_NAME"
    else
        echo "Файл $FILE_NAME уже существует"
    fi
done

echo "Выполнение скрипта завершено."
```

**2) Создайте юнит который будет вызывать этот скрипт при запуске. Проверьте**  
vim /etc/systemd/system/create_files.service    
![image](https://github.com/user-attachments/assets/273ebb55-f0e7-476f-8fb6-16506a7cd180)    
  


![image](https://github.com/user-attachments/assets/91c72099-2d6b-4bba-9233-332a1aa7092e)    


**3) Создайте таймер который будет вызывать выполнение одноимённого systemd юнита каждые 5 минут.**    
vim /etc/systemd/system/create_files.timer  
![image](https://github.com/user-attachments/assets/7e4e5576-77fe-4f66-ad0e-5b9296490e8a)    
  
![image](https://github.com/user-attachments/assets/f846515b-34d5-4ae9-95b6-1ec054c39883)    



**4) От какого пользователя вызыаются юниты поумолчанию?**  
root  

**5) Создайте пользователя от имени которого будет выполняться ваш скрипт.**  
useradd -m -s /bin/bash script_user  
sudo passwd script_user  

**6) Дополните юнит информацией о пользователе от которого должен выплняться скрипт.**  
vim /etc/systemd/system/create_files.service  
![image](https://github.com/user-attachments/assets/f6ac531d-0973-4862-9b04-3bcdf30d16d8)    

**7) Дополните ваш скрипт так, что бы он независимо от местоположения всега выполнялся в домашней папке того кто его вызывает.**  
скрипт в 1 пункте уже удовлетворяет этому условию 
