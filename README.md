# docker-compose学习笔记

### docker-compose.yml 的一些基本语法

>bulid 
指定Dockerfile所在文件的路径，docker-compose会利用这个构建镜像
build /path/to/dir

> command 
覆盖容器启动后默认执行的指令
command : bundle exec thin p 2333

>links 
镜像连接到其他的服务容器，如nginx要链接到我的php-fpm
links:
 - my_php_fpm
 
>external_links

连接到docker-compose.yml外部的容器,使用方法跟links类似

external_links:
  - extract_links1
  
>ports:
  端口映射使用，格式为 主机端口:容器端口
  ports:
    - "81:80"
 
>expose 
  暴露端口，不能映射到宿主机，只能被连接的服务访问
  expose:
    - "2000"
    - "3000"
    
>volumes
  卷挂在路径设置，可以设置两种模式
  1.ro 相当于复制一份不可以改变的配置
  2.rw 主机配置可以改变容器配置
  格式 主机目录:容器目录
 volumes:
  - /path/to/local/:/path/to/container
 
> volumes_from

  从另一个服务或容器挂载它的所有卷。
  volumes_from:
    - service_name
    - container_name
    
>environment
    设置环境变量，可以使用数组或者字典两种格式，如果只给定变量的名称就会自动获取compose 主机上的值
   environment:
    - RACK_ENV=development
    - SESSION_SECRET
    
>env_file
  从文件中获取环境变量，可以为单独的文件路径或列表。
如果通过 docker-compose -f FILE 指定了模板文件，则 env_file 中路径会基于模板文件路径。
如果有变量名称与 environment 指令冲突，则以后者为准。
env_file: .env

>env_file:
  - ./common.env
  - ./apps/web.env
  - /opt/secrets.env
环境变量文件中每一行必须符合格式，支持 # 开头的注释行。
 ```shell
  #common.env: Set Rails/Rack environment
 ```
RACK_ENV=development

>extends
  基于已有的服务进行扩展。例如我们已经有了一个 webapp 服务，模板文件为 common.yml。
 common.yml
 ```shell
   webapp:
    build: ./webapp
    environment:
     - DEBUG=false
     - SEND_EMAILS=false
 ```
编写一个新的 development.yml 文件，使用 common.yml 中的 webapp 服务进行扩展。
```shell
web:
  extends:
  file: common.yml
  service: webapp
ports:
   - "8000:8000"
links:
    - db
environment:
  - DEBUG=true
db:
  image: postgres
```
后者会自动继承 common.yml 中的 webapp 服务及相关环节变量

>net 
设置网络模式。使用和 docker client 的 --net 参数一样的值。
  net: "bridge"
  net: "none"
  net: "container:[name or id]"
  net: "host"
  
>pid 
 跟主机系统共享进程命名空间。打开该选项的容器可以相互通过进程 ID 来访问和操作。
  pid: "host"

>dns 
 配置 DNS 服务器。可以是一个值，也可以是一个列表。
  dns: 8.8.8.8
  dns:
  - 8.8.8.8
  - 9.9.9.9
  
  


 
