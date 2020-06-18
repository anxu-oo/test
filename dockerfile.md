

### **关于dockerfile**

虽然我们可以通过docker  commit命令来手动创建镜像，但是通过Dockerfile文件，可以帮助我们自动创建镜像，并且能够自定义创建过程。本质上，Dockerfile就是由一系列命令和参数构成的脚本，这些命令应用于基础镜像并最终创建一个新的镜像。它简化了从头到尾的构建流程并极大的简化了部署工作。使用dockerfile构建镜像有以下好处：

- 像编程一样构建镜像，支持分层构建以及缓存；
- 可以快速而精确地重新创建镜像以便于维护和升级；
- 便于持续集成；
- 可以在任何地方快速构建镜像

### **Dockerfile指令**

我们需要了解一些基本的Dockerfile 指令，Dockerfile 指令为 Docker 引擎提供了创建容器映像所需的步骤。这些指令按顺序逐一执行。以下是有关一些基本 Dockerfile 指令的详细信息。

| 参数       | 解释                                                         | 例子                                                         |
| ---------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| FROM       | <font size=2>FROM 指令用于设置在新映像创建过程期间将使用的容器映像。</font> | <font size=2>FROM microsoft/dotnet:2.1-aspnetcore-runtime</font> |
| RUN        | <font size=2>RUN 指令指定将要运行并捕获到新容器映像中的命令。 这些命令包括安装软件、创建文件和目录，以及创建环境配置等</font>> | <font size=2>RUN ["yum","install","-y","nginx"]</font>       |
| COPY       | <font size=2>COPY 指令将文件和目录复制到容器的文件系统。文件和目录需位于相对于 Dockerfile 的路径中。</font> | <font size=2>COPY nginx.conf /etc/nginx/nginx.conf</font>    |
| ADD        | <font size=2>ADD 指令与 COPY 指令非常类似，但它包含更多功能。除了将文件从主机复制到容器映像，ADD 指令还可以使用 URL 规范从远程位置复制文件。</font> | <font size=2>ADD https://www.python.org/ftp/python/3.5.1/python-3.5.1.exe /temp/python-3.5.1.exe</font> |
| WORKDIR    | <font size=2>WORKDIR 指令用于为其他 Dockerfile 指令（如 RUN、CMD）设置一个工作目录，并且还设置用于运行容器映像实例的工作目录。</font> | <font size=2>WORKDIR /app</font>                             |
| CMD        | <font size=2>CMD指令用于设置部署容器映像的实例时要运行的默认命令。例如，如果该容器将承载 NGINX  Web 服务器，则 CMD 可能包括用于启动Web服务器的指令，如 nginx.exe。  如果 Dockerfile 中指定了多个CMD 指令，只会计算最后一个指令。</font> | <font size=2>CMD["c:\\Apache24\\bin\\httpd.exe", "-w"]</font> |
| ENTRYPOINT | <font size=2>配置容器启动后执行的命令，并且不可被 docker run 提供的参数覆盖。每个 Dockerfile 中只能有一个ENTRYPOINT，当指定多个时，只有最后一个起效。</font> | <font size=2>ENTRYPOINT ["dotnet", "Magicodes.Admin.Web.Host.dll"]</font> |
| ENV        | <font size=2>ENV命令用于设置环境变量。这些变量以”key=value”的形式存在</font> | <font size=2>ENV VERSION=1.0 DEBUG=on </font>                |
| EXPOSE     | <font size=2>用来指定端口</font>                             | <font size=2>EXPOSE 80</font>                                |

> ```注```:RUN执行每一个指令都会创建一层，并构成新的镜像。当运行多个指令时，会产生一些非常臃肿、非常多层的镜像，不仅仅增加了构建部署的时间，也很容易出错。因此，在很多情况下，我们可以合并指令并运行，例如：RUN apt-get update && apt-get install -y  libgdiplus。在命令过多时，一定要注意格式，比如换行、缩进、注释等，会让维护、排障更为容易，这是一个比较好的习惯。使用换行符时，可能会遇到一些问题，具体可以参阅下节的转义字符。
>
> ```注```:copy 如果源或目标包含空格，请将路径括在方括号和双引号中。

![images](./image/Snipaste_2020-06-18_09-55-36.png)

### **例子**

```yaml
#安装redis
FROM centos:centos7

ENV VER     3.0.0

ENV TARBALL http://download.redis.io/releases/redis-$VER.tar.gz

# ==> Install curl and helper tools...

RUN apt-get update

RUN apt-get install -y  curl make gcc

# ==> Download, compile, and install...

RUN curl -L $TARBALL | tar zxv

WORKDIR  redis-$VER

RUN make

RUN make install

#...

# ==> Clean up...

WORKDIR /

RUN apt-get remove -y --auto-remove curl make gcc

RUN apt-get clean

RUN rm -rf /var/lib/apt/lists/*  /redis-$VER

#...

CMD ["redis-server"]
```

