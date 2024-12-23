# samba

1) Установите пакет samba  
apt-get install samba  

2) ЧТо такое побщая папка, зачем оно может быть нужно?
Общая папка — это папка на компьютере, доступная для чтения и/или записи другими пользователями в сети. Она может использоваться для обмена файлами, совместной работы, хранения документов, доступа к медиафайлам и т. д

3) Создайте общую папку без пароля с правами только на чтение файлов
mkdir -p /srv/samba/public_read  
chmod 755 /srv/samba/public_read  
groupadd nogroup  
chown -R nobody:nogroup /srv/samba/public_read  
vim /etc/samba/smb.conf

В /etc/samba/smb.conf нужно добавить в конец:  
[ReadOnlyShare]  
    path = /srv/samba/read_only_share  
    browsable = yes  
    read only = yes  
    guest ok = yes  
    force user = nobody  

systemctl restart smb  
systemctl restart nmb  

4) Создайте общую папку с паролем с правами на чтение и запись
mkdir -p /srv/samba/read_write_share  
chmod 2770 /srv/samba/read_write_share  
chown -R nobody:users /srv/samba/read_write_share  
apt-get install samba-client  
smbpasswd -a marycher  
vim /etc/samba/smb.conf

В /etc/samba/smb.conf нужно добавить в конец:  
[ReadWriteShare]  
    path = /srv/samba/read_write_share  
    browsable = yes  
    read only = no  
    valid users = marycher  
   
systemctl restart smb  
systemctl restart nmb  

5) Создайте общую папку с доступом для какой-то группы с полными правами
groupadd samba_full_access  
usermod -aG samba_full_access marycher  
mkdir -p /srv/samba/full_access_share  
chmod 2770 /srv/samba/full_access_share  
chown -R :samba_full_access /srv/samba/full_access_share  
vim /etc/samba/smb.conf

В /etc/samba/smb.conf нужно добавить в конец:  
[FullAccessShare]  
    path = /srv/samba/full_access_share  
    browsable = yes  
    read only = no  
    valid users = @samba_full_access  
    
systemctl restart smb  
systemctl restart nmb  

6) Создайте общую папку в которой у одной группы будет полный доступ, а у другой только доступ на чтение. Третья группа не должна иметь к ней доступа
useradd user1  
useradd user2  
useradd user3  
passwd user1  
passwd user2  
passwd user3  
groupadd group_full  
groupadd group_read  
groupadd group_no_access  
usermod -aG group_full user1  
usermod -aG group_read user2  
usermod -aG group_no_access user3  
mkdir -p /srv/samba/mixed_access_share  
chmod 2770 /srv/samba/mixed_access_share  
chown -R :group_full /srv/samba/mixed_access_share

В /etc/samba/smb.conf нужно добавить в конец:  
[MixedAccessShare]  
    path = /srv/samba/mixed_access_share  
    browsable = yes  
    read only = no  
    valid users = @group_full @group_read  
    write list = @group_full  
    read list = @group_read  
    force group = group_full  
    create mask = 0660  
    directory mask = 2770  
setfacl -m g:group_full:rwx /srv/samba/mixed_access_share  
setfacl -m g:group_read:rx /srv/samba/mixed_access_share  
setfacl -m g:group_no_access:--- /srv/samba/mixed_access_share  
systemctl restart smb  
systemctl restart nmb  







