# Systemd

**1) Что такое systemd юнит?**  
Юнит - это основной элемент управления в systemd, представляющий ресурс или действие

**2)Проверье статус любого systemd юнита, какую информацию выводит эта кманда?**  
systemctl status cups.service  
 
**3) ПОпробуйте оставновить сервис.**  
systemctl stop cups.service  
![image](https://github.com/user-attachments/assets/7980b6a4-5189-49b4-9847-37e7a62d1569)  

**4) Перезапустите его.**  
systemctl restart sshd.service  
systemctl status sshd.service  
![image](https://github.com/user-attachments/assets/f7fe765c-6fac-42ad-9ebb-59945a3aa405)  


**5) УДалите из автозагрузки**  
systemctl disable cups.service    
systemctl is-enabled cups.service   

**6) Верните обратно**  
systemctl enable sshd.service    
systemctl is-enabled sshd.service    
![image](https://github.com/user-attachments/assets/9c39e144-b555-4fd2-8b4a-de30f6b6ef8d)  

**7) Что такое таймеры?**
Таймеры (.timer) в systemd — это способ запланировать выполнение действий, ассоциированных с другими юнитами.
susystem
