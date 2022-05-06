# Docker 安装

---

1. 设置存储库

    sudo apt-get update

2. 安装包以允许通过HTTPS使用存储库

 ```
    sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
 ```

3. 添加Docker的官方GPG密钥

 ```$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -```

4. 验证已有指纹秘钥

    $ sudo apt-key fingerprint 0EBFCD88
    > pub   4096R/0EBFCD88 2017-02-22
      ​    Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
    uid                  Docker Release (CE deb)  `<docker@docker.com>`
    sub   4096R/F273FCD8 2017-02-22
    
5. 设置稳定存储库

    ```
    $ sudo add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $ (lsb_release -cs) \
        stable"
    ```

6. 安装Docker CE

    ```
    $ sudo apt-get update
    $ sudo apt-get install docker-ce
    ```

7. 通过运行hello-world映像验证是否正确安装了Docker CE

   ```
   sudo docker container run hello-world
   ```

8. 以非root用户身份管理Docker

   To create the `docker` group and add your user:

   ```
   $ sudo groupadd docker
   ```

   Add your user to the `docker` group.

   ```
   $ sudo usermod -aG docker $USER
   ```

9. Log out and log back in so that your group membership is re-evaluated.

10. Verify that you can run `docker` commands without `sudo`.

    ```
    $ docker run hello-world
    ```

11. Docker

    ```
    问题:
         docker: Error response from daemon: ... : net/http: TLS handshake timeout.
    
    解决办法:
    使用国内的Docker仓库daocloud
    $ echo "DOCKER_OPTS=\"\$DOCKER_OPTS --registry-mirror=http://f2d6cb40.m.daocloud.io\"" | sudo tee -a /etc/default/docker
    $ sudo service docker restart
    
    ```



