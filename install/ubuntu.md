## Ubuntu安装

ubuntu 16.04，php7.0，root用户为例, 建议使用阿里云镜像源  

## step 1
安装Apache：  
>apt-get install apache2

## step 2
安装php以及相关组件。php建议安装7.0，大于等于5.5.9其他版本也可以  
>apt-get install php7.0 php-mbstring php-gd php-mcrypt php-mongodb  
  
安装apache的php解析组件：  
>apt-get install libapache2-mod-php7.0

## step 3
安装mongodb：  
>apt-get install mongodb  
  
安装完成后，测试mongodb是否安装成功，创建数据库和用户，示例如下图：  
![](http://actionview.cn/mongo-db-user.png)  

## step 4
全局安装composer：  
>curl -sS https://getcomposer.org/installer | php  
>mv composer.phar /usr/local/bin/composer  
  
下载程序：  
>cd /var/www/  
>git clone https://github.com/lxerxa/actionview.git actionview  
  
安装依赖：  
>cd actionview  
>composer install --no-dev  

执行配置脚本：  
>sh config.sh  
  
修改数据库连接参数，在拷贝后的.env文件中，示例如下：  
>cp .env.example .env  
  
>DB_HOST=127.0.0.1  
>DB_DATABASE=actionviewdb  
>DB_USERNAME=actionview  
>DB_PASSWORD=secret  
  
执行db数据初始化脚本：  
>mongorestore -h 127.0.0.1 -u actionview -p secret -d actionviewdb --drop ./dbdata  
  
配置Apache：  
> DocumentRoot /var/www/actionview/public  
> &lt;Directory /var/www/actionview/public&gt;  
> &emsp;Options FollowSymLinks  
> &emsp;Order deny,allow  
> &emsp;AllowOverride All  
> &lt;/Directory&gt;  
  
启用rewrite模块，重新启动Apache：  
>a2enmod rewrite  
>service apache2 stop  
>service apache2 start  

## step 5
安装完成，祝好运！  
访问系统：http://xxx.xxx.xxx.xxx，管理员登录：user: admin@action.view, password: actionview  

## step 6
邮件通知配置，如不需要可忽略此步骤。  

在.env文件里配置参数，示例如下：  
>MAIL_DRIVER=smtp  
>MAIL_HOST=smpt.126.com  
>MAIL_PORT=465  
>MAIL_USERNAME=xxx  
>MAIL_PASSWORD=xxx  
>MAIL_ENCRYPTION=ssl  
>MAIL_ADDRESS=xxx@126.com  
>HTTP_HOST=http://www.actionview.cn  

crontab里添加：  
>\* \* \* \* \* php /var/www/actionview/artisan schedule:run >> /dev/null 2>&1  
  
重新启动cron服务：  
> service cron restart  
