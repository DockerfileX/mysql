# MySQL

## 1. 特性

1. mysql
2. TZ=Asia/Shanghai
3. C.UTF-8
4. my.cnf -> utf8mb4

## 2. 拉取与制作标签

1. pull

   在自动构建后，拉取下来

   ```sh
   docker pull nnzbz/mysql
   ```

2. tag

   ```sh
   docker tag nnzbz/mysql:latest nnzbz/mysql:5
   ```

3. push

   ```sh
   docker push nnzbz/mysql:5
   ```

## 3. 创建并运行容器

- 将数据映射到宿主机路径中保存
  
  在宿主机中执行以下命令

  ```sh
  # 如果是重新安装，/var/lib/mysql目录已然有数据，那么这一段可不用执行，直接创建并运行容器就可以了，数据不会被覆盖
  mkdir /var/lib/mysql
  # 添加mysql用户并指定uid为1000
  useradd mysql -u 999
  chown -R mysql:mysql /var/lib/mysql/
  # 或直接
  chown -R 999:999 /var/lib/mysql/
  
  # 创建并运行MySQL的容器
  docker run --name mysql -dp3306:3306 -e MYSQL_ROOT_PASSWORD=root -v /var/lib/mysql:/var/lib/mysql --restart=always  nnzbz/mysql
  ```

  - -v 冒号前一个 `/var/lib/mysql` 是宿主机的路径

- ~~将数据映射到数据卷中保存~~

  ```sh
  # 创建MySQL的数据卷
  docker run --name mysql-data nnzbz/mysql echo "data-only container for MySQL"
  # 创建并运行MySQL的容器
  docker run -dp3306:3306 --restart=always --name mysql -e MYSQL_ROOT_PASSWORD=root --volumes-from mysql-data nnzbz/mysql
  ```

## 4. 其它容器连接MySQL容器

```sh
docker run --name some-app --link some-mysql:mysql -d application-that-uses-mysql
```
