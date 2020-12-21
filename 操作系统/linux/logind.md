# logind.conf 配置文件

> /etc/systemd/logind.conf

* HandlePowerKey 按下电源键后的行为，默认power off  
* HandleSleepKey 按下挂起键后的行为，默认suspend  
* HandleHibernateKey 按下休眠键后的行为，默认hibernate  
* HandleLidSwitch 合上笔记本盖后的行为，默认suspend

* 可选配置:  
  * ignore 忽略，跳过  
  * poweroff 关机  
  * reboot 重启  
  * halt 挂起, 进程关闭  
  * lock 仅锁屏，计算机继续工作  
  * suspend: shell内建指令，可暂停目前正在执行的shell。若要恢复，则必须使用SIGCONT信息。所有的进程都会暂停，但不是消失
  * hibernate: 让笔记本进入休眠状态  
 hybrid-sleep: 混合睡眠，主要是为台式机设计的，是睡眠和休眠的结合体，当你选择Hybird时，系统会像休眠一样把内存里的数据从头到尾复制到硬盘里 ，然后进入睡眠状态，即内存和CPU还是活动的，其他设置不活动，这样你想用电脑时就可以快速恢复到之前的状态了，笔记本一般不用这个功能
