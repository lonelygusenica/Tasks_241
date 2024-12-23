# Systemd

**1) Что такое systemd юнит?**  
Systemd — это система инициализации и менеджер служб для Linux.
Units — это основные объекты, с которыми работает systemd. Каждый юнит описывает ресурс, которым systemd управляет

**2)Проверье статус любого systemd юнита, какую информацию выводит эта кманда?**  
systemctl status sshd.service  
![image](https://github.com/user-attachments/assets/e9a371a0-2087-4485-8fd4-e875fd59ecc4)  

**3) ПОпробуйте оставновить сервис.**  
systemctl stop sshd.service  
![image](https://github.com/user-attachments/assets/c3426877-0162-43d7-8e79-4bbefebf201c)  

**4) Перезапустите его.**  
systemctl restart sshd.service  
systemctl status sshd.service  
![image](https://github.com/user-attachments/assets/e05c1563-473c-4422-b4c4-ae13408e04d9)  

**5) УДалите из автозагрузки**  
systemctl disable sshd.service  
systemctl is-enabled sshd.service  
![image](https://github.com/user-attachments/assets/ef763b7d-70c1-446a-9333-6d07eca501fb)  

**6) Верните обратно**  
systemctl enable sshd.service  
systemctl is-enabled sshd.service  
![image](https://github.com/user-attachments/assets/d43d0811-832e-44aa-8c89-3646c88683ff)  

**7) Что такое таймеры?**
Таймеры в systemd могут запускать различные юниты в определенное время или через заданные интервалы.
