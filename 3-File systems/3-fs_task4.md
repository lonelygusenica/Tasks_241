# ФС

**1) Raid массивы, что такое икакие бывают**  
RAID (Redundant Array of Independent Disks) — это технология объединения нескольких жестких дисков в один логический том для повышения производительности

**2) Добавьте в виртуальную машину 2 диска отформатируйте их в ext4**  
mkfs.ext4 /dev/sdb  
mkfs.ext4 /dev/sdc

**3) Создайте из них raid 0 массив**
mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sdc /dev/sdd
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm.conf

**4)Проверье всё ли работает**
![image](https://github.com/user-attachments/assets/e3620470-93ec-4072-a8e4-f1e016c7b169)

**5) Удалите raid0 и создайте raid1**  
sudo mdadm --stop /dev/md0  
sudo mdadm --remove /dev/md0  
sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sdс /dev/sdd   
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm.conf  
cat /proc/mdstat  

**6) В чём между ними разница?**  
- RAID 0: повышает скорость работы, но не предоставляет избыточности (данные теряются при поломке одного диска).  
- RAID 1: обеспечивает избыточность(данные дублируются на два диска), но не повышает скорость работы по сравнению с RAID 0.  

**7) Есть ли файловые системы которые поддерживают raid массивы без стороненго ПО**  
Btrfs: поддерживает RAID на уровне самой файловой системы  
ZFS: также поддерживает создание RAID-массивов без необходимости в стороннем ПО  

**8) Можно ли создать raid массив во время установки системы?**  
Да
