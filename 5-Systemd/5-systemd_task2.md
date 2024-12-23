# Systemd

**1) Создайте скрипт который создаёт папку заполняет её файлами ( имена 1-4 ) и записывает в них информацию о текущей дате, версии ядра, имени компьютера и списе всех файлов в домашнем каталоге пользователя от которого выполняется скрипт( не забудьте сдлеать проверку на существование файлов и папок)**  
```
#!/bin/bash

# Определение домашнего каталога пользователя
HOME_DIR=$(eval echo "~$USER")

# Переход в домашний каталог
cd "$HOME_DIR" || { echo "Не удалось перейти в домашний каталог"; exit 1; }

# Имя создаваемой папки
FOLDER_NAME="my_folder"

# Проверка существования папки, создание если не существует
if [ ! -d "$FOLDER_NAME" ]; then
    mkdir "$FOLDER_NAME"
    echo "Папка '$FOLDER_NAME' создана."
else
    echo "Папка '$FOLDER_NAME' уже существует."
fi

# Переход в создаваемую папку
cd "$FOLDER_NAME" || { echo "Не удалось перейти в папку '$FOLDER_NAME'"; exit 1; }

# Создание и заполнение файлов
for i in {1..4}; do
    FILE_NAME="$i"
    if [ ! -f "$FILE_NAME" ]; then
        touch "$FILE_NAME"
        echo "Файл '$FILE_NAME' создан."
    else
        echo "Файл '$FILE_NAME' уже существует."
    fi

    # Запись информации в файл
    {
        echo "Текущая дата: $(date)"
        echo "Версия ядра: $(uname -r)"
        echo "Имя компьютера: $(hostname)"
        echo "Список файлов в домашнем каталоге:"
        ls "$HOME_DIR"
    } > "$FILE_NAME"
done

echo "Скрипт выполнен успешно."
```

**2) Создайте юнит который будет вызывать этот скрипт при запуске. Проверьте**  
mv script.sh /usr/local/bin/  
sudo chmod +x /usr/local/bin/script.sh  
touch /etc/systemd/system/create_files.service  
vim /etc/systemd/system/create_files.service  
Содержимое create_files.service:    
[Unit]  
Description=Создание папки и файлов с информацией  
After=network.target  
[Service]  
Type=oneshot  
ExecStart=/usr/local/bin/script.sh  

systemctl daemon-reload  
systemctl start create_files.service  
systemctl status create_files.service  
![image](https://github.com/user-attachments/assets/a5753010-4ad7-4aff-b704-ff1cf622bd6c)


**3) Создайте таймер который будет вызывать выполнение одноимённого systemd юнита каждые 5 минут.**  
touch /etc/systemd/system/create_files.timer  
vim /etc/systemd/system/create_files.timer  
Содержимое create_files.timer:  
[Unit]  
Description=Таймер для создания папки и файлов каждые 5 минут  
[Timer]  
OnBootSec=5min  
OnUnitActiveSec=5min  
Unit=create_files.service  
[Install]  
WantedBy=timers.target  

systemctl daemon-reload  
systemctl enable create_files.timer  
systemctl start create_files.timer  
systemctl status create_files.timer  
![image](https://github.com/user-attachments/assets/c6897218-e885-4a27-9481-e43cb05b775f)  


**4) От какого пользователя вызыаются юниты поумолчанию?**  
root  

**5) Создайте пользователя от имени которого будет выполняться ваш скрипт.**  
useradd -m -s /bin/bash script_user  
sudo passwd script_user  

**6) Дополните юнит информацией о пользователе от которого должен выплняться скрипт.**  
vim /etc/systemd/system/create_files.service  
Содержимое create_files.service:  
[Unit]  
Description=Создание папки и файлов с информацией  
After=network.target  
[Service]  
Type=oneshot  
User=script_user  
ExecStart=/usr/local/bin/create_files.sh  
[Install]  
WantedBy=multi-user.target  

systemctl daemon-reload  
systemctl restart create_files.service  
systemctl restart create_files.timer  

**7) Дополните ваш скрипт так, что бы он независимо от местоположения всега выполнялся в домашней папке того кто его вызывает.**  
```
#!/bin/bash

# Определение домашнего каталога пользователя script_user
HOME_DIR=$(eval echo "~script_user")

# Переход в домашний каталог
cd "$HOME_DIR" || { echo "Не удалось перейти в домашний каталог"; exit 1; }

# Имя создаваемой папки
FOLDER_NAME="my_folder"

# Проверка существования папки, создание если не существует
if [ ! -d "$FOLDER_NAME" ]; then
    mkdir "$FOLDER_NAME"
    echo "Папка '$FOLDER_NAME' создана."
else
    echo "Папка '$FOLDER_NAME' уже существует."
fi

# Переход в создаваемую папку
cd "$FOLDER_NAME" || { echo "Не удалось перейти в папку '$FOLDER_NAME'"; exit 1; }

# Создание и заполнение файлов
for i in {1..4}; do
    FILE_NAME="$i"
    if [ ! -f "$FILE_NAME" ]; then
        touch "$FILE_NAME"
        echo "Файл '$FILE_NAME' создан."
    else
        echo "Файл '$FILE_NAME' уже существует."
    fi

    # Запись информации в файл
    {
        echo "Текущая дата: $(date)"
        echo "Версия ядра: $(uname -r)"
        echo "Имя компьютера: $(hostname)"
        echo "Список файлов в домашнем каталоге:"
        ls "$HOME_DIR"
    } > "$FILE_NAME"
done

echo "Скрипт выполнен успешно."
```
