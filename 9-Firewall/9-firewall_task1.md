# firewall


**1) Установите iptables**  
apt-get install iptables  

**2) Проверьте осталась ли возможность подключения по ssh к вашему серверу**  
iptables -L -v  
ssh student@95.31.204.147 -p 235  
![image](https://github.com/user-attachments/assets/0ebadc4b-3868-4d4c-bb07-cb788c853eb2)  
осталась

**3) Почему может пропасть такая возможность?**  
После установки и настройки iptables могут быть добавлены правила, которые блокируют доступ к SSH. Если нет правил для разрешения доступа на порт 22, сервер может не принимать входящие соединения по этому порту.  

**4) Откройте нужный порт на сервере чтобы восстановить подключение**  
iptables -A INPUT -p tcp --dport 235 -j ACCEPT

**5) Это будет udp или tcp прот?**  
tcp  

**6) Сохраняются ли записанные вами правила после перезагрузки?**  
Нет, не сохраняются  

**7) Как их сохранить?**  
vim /etc/systemd/system/iptables-restore.service  
![image](https://github.com/user-attachments/assets/e07b2b13-1287-4daa-85a0-38ca676e4a77)  

systemctl daemon-reload  
sudo systemctl enable iptables-restore.service  
sudo systemctl start iptables-restore.service  
