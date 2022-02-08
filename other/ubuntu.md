# Ubuntu使用记录

## ubuntu下mysql安装

1. 安装mysql

    ```shell
    sudo apt install mysql-server
    sudo usermod -d /var/lib/mysql/ mysql
    ```

2. 停止、启动命令

    ```shell
    sudo service mysql stop
    sudo service mysql start
    ```

3. 连接命令

    ```shell
    mysql -u root -p
    ```
