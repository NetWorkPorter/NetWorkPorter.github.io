## Windows端实现文字转语音

> Windows 端实现文字转语音可以使用jacob类库实现

### 参考资料

- [jacob-project(GitHub)](https://github.com/freemansoft/jacob-project)
- [用Java实现简单的语音朗读(CSDN)](https://blog.csdn.net/john_bian/article/details/79025914)

### 安装调试

> 1.下载类库
>> 下载地址：https://github.com/freemansoft/jacob-project/releases/download/Root_B-1_20/jacob-1.20.zip

> 2.解压类库

> 3.将解压后的jacob.jar文件复制到项目中
>> 在项目pom.xml中添加依赖
>> ~~~xml
>>        <!-- 引入本地jar -->
>>        <dependency>
>>            <groupId>jacob</groupId>
>>            <artifactId>jacob</artifactId>
>>            <version>1.0.0</version>
>>            <scope>system</scope>
>>            <systemPath>${basedir}/lib/jacob.jar</systemPath>
>>       </dependency>
>> ~~~
> 4.在项目中引入jacob.dll文件
>> 此处不知道如何在项目中添加jacob.dll文件，所以将jacob.dll文件复制到windows的system32中(TODO 这里待后续优化)

> 5.编写执行代码
>> ~~~java
>>     public static void Broadcast(String text) {
>>        ActiveXComponent sap = new ActiveXComponent("Sapi.SpVoice");
>>        Dispatch sapo = sap.getObject();
>>        try {
>>            // 音量 0-100
>>            sap.setProperty("Volume", 100);
>>            // 语速 -10 +10
>>            sap.setProperty("Rate", 2);
>>            // 执行朗读
>>            Dispatch.call(sapo, "Speak", text);
>>        } catch (Exception e) {
>>            e.printStackTrace();
>>       } finally {
>>            sapo.safeRelease();
>>            sap.safeRelease();
>>        }
>>    }
>> ~~~

## Spring Boot 基础

### 关闭时执行代码

### 日志格式及存储

> 日志级别: debug, info, warn, error.

> 日志格式: %d: 时间, %p: 日志级别, %m: 日志内容, %c: 日志所属类名, %L: 日志所属行号.

> 日志存储: 默认存储在当前目录下的 log 文件夹下.

> 日志配置: logback.xml

###        