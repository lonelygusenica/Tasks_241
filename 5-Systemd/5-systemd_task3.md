# Systemd

**1) Посмотретите журналы ssh**  
journalctl -u sshd  
![image](https://github.com/user-attachments/assets/4324bfde-85b3-466c-83cb-225442eb3436)  

**2) Выведите журналы в реальном времени.**  
journalctl -u sshd -f  
![image](https://github.com/user-attachments/assets/d5bda7fc-694b-48f5-984e-c84d90ef5ce7)

**4) Можно ли без комады journalctl прочитать логи systemd?**  
да, можно

**5) Сколько будет 2-2?** 0
