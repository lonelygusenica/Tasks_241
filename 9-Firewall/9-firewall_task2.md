# firewall

**1) Удалите iptables и установите firewalld**  
apt-get remove iptables  
apt-get install firewalld  

**2)Попробуйте так-же проверить возможность подключения по ssh**  
![image](https://github.com/user-attachments/assets/f6e05ce4-5ffd-488a-9a63-215bbe07f91d)  
Подключиться всё также можно.

**3) Если её нет то откройте порт**  
firewall-cmd --zone=public --add-port=234/tcp --permanent  
firewall-cmd --reload  

**4) Выведите список открытых портов с помощью firewall-cmd**
firewall-cmd --zone=public --list-ports  
![image](https://github.com/user-attachments/assets/8d1d2cc6-3589-488a-b004-4f2aeecd13c5)  


**5) Можно ли там добавить порты по названию сервиса?**  
Да, можно  

**6) На вашей Локальной виртуальной машине попробуйте подключиться к серверу samba из предыдущих заданий**  
smbclient -L //192.168.0.130 -U marycher  
Подключиться можно.

**7) Если не получилось то откройте нужные порты**  
firewall-cmd --zone=public --add-service=samba --permanent  
firewall-cmd --reload  

**8) Сделайте так чтобы изменения были постоянными**  
При использовании параметра --permanent изменения сохраняются в конфигурации firewalld.  
