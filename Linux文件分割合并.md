# Linux文件分割合并

### 一、按大小分割
	
	file=fileName
    split -b 200M {file} {file}_

### 二、按个数分割

	file=fileName
    split -n 10 -d {file} {file}_
	
### 三、分割禁止生成空文件
	
	file=fileName
    split -n 10 -e -d {file} {file}_
	
### 四、按行数分割
	
	file=fileName
    split -l 100 {file} {file}_
	
### 五、文件合并
	
	cat fileName_* > fileName
	

	
### 查看文件md5，检验文件是否损坏
	md5sum fileNmae
