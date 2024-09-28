一.在window下安装docker
第一步，配置windows环境
①处理器开启虚拟化
处理器是否开启虚拟化可以直接在“任务管理器--性能--[CPU](https://www.smzdm.com/fenlei/cpu/)“中查看，如果虚拟化显示”已启用“就说明没问题，如果没启用就需要进[主板](https://www.smzdm.com/fenlei/zhuban/)BIOS中开启，具体开启方法可以[百度](https://pinpai.smzdm.com/3357/)自己的主板型号开启。
![[1727088923839 1.png]]
②window环境设置
电脑桌面使用快捷键 `win + r` 键入 `OptionalFeatures`，“确定”之后打开 Windows 功能。
![[1727089077550.png]]
然后在“Windows 功能”中勾选Hyper-V、Windows虚拟机监控程序平台、容器、适用于Linux的Windows子系统这四项，点“确定”。
![[1727089109182.png]]

提示重启系统，点“立即重新启动”。
![[1727089136805.png]]
第二步安装dockers
①下载docker windows版本
重启之后打开Docker官网下载Windows版本的安装程序，下载页面地址：[https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/ "https://www.docker.com/products/docker-desktop/")
![[1727089592233 1.png]]
②安装docker
下载好之后直接双击安装，安装过程中这里有两个选项，记得都勾选上。
![[1727089256284.png]]
之后它就会自动下载安装必要的程序到本地，时间视自己的网络而定。
![[1727089298281.png]]
出现上图界面就说明安装完成，点“Close”关闭安装界面。
③配置docker
打开docker后，这里点“Accept”接受。
![[1727089682375.png]]

然后，选择”skip“跳过
![[1727089764446.png]]
然后就进入到docker页面了 
我这里已经运行了cloud_kbs容器了
![[1727089826964.png]]
二、安装Obsidian
https://obsidian.md/
在该网站下载安装即可，安装完成打开后将会让你配置本地存储笔记的位置，选择合适位置即可
我的位置在下面图片所示目录下
![[1727090203096.png]]


三，配置docker-compose文件，部署Cloud_KBS容器
第一步，配置compose文件
①在合适的位置创建文件夹 CLoud_KBS
![[1727090437519.png]]
②到CLoud_KBS，创建docker-compose.yaml.txt文件
并将下面内容复制到该txt文件

。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。
networks:
  cloud-kbs:
    name: cloud-kbs

services:
  cloud-kbs:
    image: marshalw/cloud-kbs
    container_name: cloud-kbs
    init: true
    restart: always
    networks:
      - cloud-kbs
    volumes:
      - ${OBSIDIAN_VAULT_ROOT}:/data
      - ./index:/index
      - /etc/localtime:/etc/localtime:ro
    ports:
      - 9800:8501
    environment:
      - LLM_BASE_URL=https://open.bigmodel.cn/api/paas/v4/
      - LLM_MODEL_NAME=GLM-4-Flash
      - EMBEDDING_MODEL_NAME=embedding-3
    env_file:
      - .env
。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。
③创建txt文件，打开后将一下内容复制到该文件
并将OBSIDIAN_VAULT_ROOT该为自己的本地笔记库的路径
我的为D:/software/Obsidian/vault
然后讲 ZHIPUAI_API_KEY改为自己的智谱的api key
。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。
OBSIDIAN_VAULT_ROOT="D:/software/Obsidian/vault"
VAULT_NAME=my-vault

ZHIPUAI_API_KEY=xxxx
。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。
第二步，拉取镜像，部署容器
①在CLoud_KBS文件夹下，在路径框内，输入cmd，进入命令窗
![[1727090991003.png]]
②输入命令 docker-compose up -d 拉取镜像（最后启动代理服务），并运行
![[1727091106127.png]]
我这已经拉取完成 直接运行容器了
![[1727091351642.png]]

最后一步，访问： [http://localhost:9800](http://localhost:9800/)
进入Cloud_KBS管理页面
![[1727091263776.png]]
