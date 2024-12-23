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
iptables -A INPUT -p tcp --dport 234 -j ACCEPT

**5) Это будет udp или tcp прот?**  
tcp  

**6) Сохраняются ли записанные вами правила после перезагрузки?**  
Нет, не сохраняются  

**7) Как их сохранить?**  
vim /etc/systemd/system/iptables-restore.service  
[Unit]  
Description=Restore iptables firewall rules  
After=network-pre.target  
Wants=network-pre.target  
  
[Service]  
Type=oneshot  
ExecStart=/usr/sbin/iptables-restore /etc/sysconfig/iptables  
RemainAfterExit=yes  

[Install]  
WantedBy=multi-user.target  
After=network-pre.target и Wants=network-pre.target обеспечат запуск этого юнита до запуска сетевых сервисов, чтобы правила были применены до их старта.  

systemctl daemon-reload  
sudo systemctl enable iptables-restore.service  
sudo systemctl start iptables-restore.service  
