# ФС

1) Выведите содержимое fstab. Что хранится в fstab?  
Файл /etc/fstab содержит информацию о том, какие файловые системы должны быть подключены (монтированы) при загрузке системы. Включает параметры подключения: точки монтирования, тип файловой системы, параметры и прочее.  
![image](https://github.com/user-attachments/assets/e431444a-abd1-4135-861b-dcfed4596171)


2) Добавьте в виртуальную машину ещё один диск
3) Узнайте как ситема видит ваш диск - выведите информацию о блочных устройствах  
![image](https://github.com/user-attachments/assets/30644a22-391c-48db-b314-f02fad63e9d2)

4) С помощью полученной информации создайте на диске таблицу разделов и фаловую систему ext4
![image](https://github.com/user-attachments/assets/4b120f8c-73f7-4227-8eeb-09a719ae762c)
![image](https://github.com/user-attachments/assets/1f1581dd-59b7-4b3f-88f2-8ddc2fb380b1)

5) Примонитруте диск в каталог /mnt  
![image](https://github.com/user-attachments/assets/4529fda4-c7e2-496c-964a-65f1fd1c98c8)

6) Зайдите в каталог и создайте там файлы  
![image](https://github.com/user-attachments/assets/c344aa6d-ed6b-4e24-a9a2-e124c0c85322)

7) Отмонтируйте диск и проверье остались ли файлы
![image](https://github.com/user-attachments/assets/da01025a-375b-4b55-9df6-17188f2c7fa1)

8) Сделайте так чтобы диск автоматически подключался при загрузке систем ( добавьте информацию о нём с fstab)
![image](https://github.com/user-attachments/assets/90e1f422-1032-4ec9-9fc2-41a7ea086d67)

9) Проверьте корретность записанных в fstab данных перед перезагрузкой
mount -a

10) Перезагрущите систему и убедитесь что диск был подключён к системе
reboot
![image](https://github.com/user-attachments/assets/c91b8218-44d0-44c1-8fef-0b345abc1e0f)
