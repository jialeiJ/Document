# BASE64Encoder

### 场景说明
BASE64Encoder配置：

    sun.misc.BASE64Encoder/BASE64Decoder类不属于JDK标准库范畴，但在JDK中包含了该类，可以直接使用。
    
# 一、Eclipse配置：
    
   * 解决方法
	    1. 右键项目
	    2. -->Bulid Path
	    3. -->Configure Build Path
	    4. -->选择JRE System Library
	    5. -->单击Access rules
	    6. -->Edit
	    7. -->Add
	    8. -->Resolution:Accessible  Rule Pattern:**