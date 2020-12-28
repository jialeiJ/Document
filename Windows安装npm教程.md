# Windows下安装npm教程

### 1、下载nodejs
登陆官网，便可以看到首页的“INSTALL”按钮，直接点击就会自动下载安装：

* [nodejs](http://nodejs.org/)

### 2、配置系统变量
安装过程中会直接添加path的系统变量，变量值是你的安装路径，例如“F:\Program Files (x86)\WorkSoftware\Nodejs”：

### 3、安装完成后测试是否安装成功

* "方法：在cmd下输入node -v，出现版本提示即NodeJS安装成功"

### 4、npm的安装

* "新版NodeJS中集成了npm，因此npm一并安装好。在cmd命令行输入npm -v测试是否成功安装，出现版本提示即OK"

### 5、安装已完成
* "常规NodeJS的搭建到现在为止已经完成了"

### 6、npm配置
npm作为一个NodeJS的模块管理，很有必要列出一些：

	①、模块路径、cache路径  
	先配置npm的全局模块的存放路径以及cache的路径，
	例如将以上两个文件夹放在NodeJS的主目录下，便在NodeJs下建立"node_global"及"node_cache"两个文件夹。
	
	②、使用cmd命令进行配置  
	启动cmd，输入  
	npm config set prefix "F:\Program Files (x86)\WorkSoftware\Nodejs\node_global"  
	npm config set cache "F:\Program Files (x86)\WorkSoftware\Nodejs\node_cache"
	
	注、如果不进行这一步设置，npm的全局安装包，将不会在node安装文件夹里。
		如果这个步骤出现错误，如：operation not permitted, mkdir 'C:\Program Files\nodejs'，请使用管理员身份打开cmd命令行。  
		
	③、测试
	现在我们来装个模块试试，
	在cmd命令行里面，输入“npm install express -g”（“-g”这个参数意思是装到global目录下，也就是上面说设置的“F:\Program Files (x86)\WorkSoftware\Nodejs\node_global”里面。）。
	待cmd里面的安装过程滚动完成后，会提示“express”装在了哪、版本还有它的目录结构是怎样。如下
	express@3.2.6 F:\Program Files (x86)\WorkSoftware\Nodejs\node_modules\express
	
	④、查看环境变量
	关闭cmd，打开系统对话框，“我的电脑”右键“属性”-“高级系统设置”-“高级”-“环境变量”。
	
	⑤、配置node_path
	进入环境变量对话框，在系统变量下新建"NODE_PATH"，输入”F:\Program Files (x86)\WorkSoftware\Nodejs\node_global\node_modules“。（ps：这一步相当关键。）
	2014.4.19新增：由于改变了module的默认地址，所以上面的用户变量都要跟着改变一下（用户变量"PATH"修改为“F:\Program Files (x86)\WorkSoftware\Nodejs\node_global\”），要不使用module的时候会导致输入命令出现“xxx不是内部或外部命令，也不是可运行的程序或批处理文件”这个错误。
	
	⑥、测试
	以上步骤都OK的话，我们可以再次开启cmd命令行，进入node（或者直接进入Nodejs目录下打开node.exe），输入“require('express')”来测试下node的模块全局路径是否配置正确了。正确的话cmd会列出express的相关信息。如下图（如出错一般都是NODE_PATH的配置不对，可以检查下第④⑤步）