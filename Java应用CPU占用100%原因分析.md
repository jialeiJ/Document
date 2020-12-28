# Java应用CPU占用100%原因分析

### 1、使用top命令查看cpu占用资源较高的进程PID

    根据top命令，发现PID为28555的Java进程占用CPU高达200%，出现故障，
    通过ps aux | grep PID命令，可以进一步确定是tomcat进程出现了问题：
	top - 17:57:40 up 350 days, 2:53, 2 users, load average: 2.08, 1.96, 1.55
	Tasks: 142 total, 1 running, 137 sleeping, 4 stopped, 0 zombie
	Cpu(s): 13.8%us, 10.8%sy, 0.0%ni, 74.4%id, 0.4%wa, 0.0%hi, 0.6%si, 0.0%st
	Mem: 8149296k total, 7585800k used, 563496k free, 440632k buffers
	Swap: 20482864k total, 3112k used, 20479752k free, 3095020k cached
	
	  PID     USER     PR     NI     VIRT    RES    SHR    S     %CPU     %MEM     TIME     COMMAND 
	28555     root     18      0    2356m   796m   9200    S    200.0     10.0  108:46.96     java
	19148     root     20      0    1306m   404m   7932    S      0.3      5.1   18:24.97     java
	30096     root     15      0    1342m   743m   7952    S      0.3      9.3   48:18.35     java

### 2、ps查看指定进程中各个线程占用CPU的状态
    
    首先显示线程列表:查看指定进程中各个线程占用CPU的状态，选出耗时最多、最繁忙的线程id
    ps -mp 28555 -o THREAD,tid,time
	
	USER     %CPU     PRI    SCNT     WCHAN    USER     SYSTEM     TID     TIME
	root     77.4      -      -         -       -         -         -      02:23:56
	root      0.0      21     -      184467     -         -       28555    00:00:00
	root      0.0      22     -         -       -         -       28556    00:00:04
	root      0.0      23     -      184466     -         -       28557    00:00:00
	root      0.0      23     -      184466     -         -       28558    00:00:00
	root      0.0      24     -      184466     -         -       28559    00:00:00
	root      0.0      24     -      184466     -         -       28801    00:00:00
	root     60.4      21     -         -       -         -       28802    01:52:00


### 3、打印线程的堆栈信息

	找到了耗时最高的线程28802，占用CPU时间快两个小时了！
	其次将需要的线程ID转换为16进制格式：
	printf “%x\n” tid
	最后jstack打印线程的堆栈信息
	jstack pid |grep tid -A 30
	
	printf "%x\n" tid, eg: printf "%x\n" 28802 ---> 7082 
	jstack 28555 | grep 7082 -A 60
	
	
	