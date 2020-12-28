# Windows下配置Jdk环境变量

* 1、选择“此电脑”，单击右键，选择“属性”

* 2、选择“高级系统设置”，选择“环境变量”

* 3、“系统变量”中选择“新建”，弹出的窗口中输入变量名：JAVA_HOME和变量值（Jdk路径）：F:\Program Files (x86)\WorkSoftware\Conf\Java\jdk1.8.0_40 。然后选择“确定”

* 4、选择“path”，单击“编辑”，变量值一栏输入;%JAVA_HOME%\bin; %JAVA_HOME%\jre\bin 然后单击“确定”

* 5、“系统变量”中选择“新建”，在弹出的窗口中输入变量名：CLASSPATH和变量值：.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar;然后单击“确定”。这样变量就OK了