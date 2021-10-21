- 通过apt下载jdk

```bash
sudo apt-get install openjdk-8-jdk
```

- 配置环境变量

在`.bashrc`中添加：

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

- 在vscode中安装`code runner`插件
- 右键`RUN CODE`或者快捷键`Ctrl + Alt + n`即可

