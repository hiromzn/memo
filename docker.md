
## �R�}���h�̌n

- image
  - Docker Hub �̃C���[�W������ (search)
  - Docker Hub ����g�������C���[�W���擾���� (pull)
  - �擾�ς݃C���[�W�ꗗ (images)
  - �C���[�W����R���e�i�𐶐����ċN�� (run)
  - ����Ȃ��Ȃ�����C���[�W���폜 (rmi)

- container
  - ��~���̃R���e�i���N�� (start)
  - �R���e�i���~ (stop)
  - �R���e�i���ŃR�}���h���s (exec)
  - �R���e�i���C���[�W�� (commit)
  - ���s���R���e�i�ꗗ (ps)
  - ��~�����܂߂��S�ẴR���e�i�ꗗ (ps -a)
  - �z�X�g�ƃR���e�i�Ԃł̃t�@�C���R�s�[ (cp)
  - �R���e�i���폜 (rm)

#### sample operation

```
docker search nginx
docker pull nginx
docker run -d -p 80:80 -v $(pwd)/nginx/log:/var/log/nginx --name webserver nginx
# -d : background run
# -p host_port:container_port : port mapping
# -v host_dir:cont_dir : mount directory
# --name cont_name
docker exec -it webserver /bin/bash
docker cp webserver:/etc/nginx/nginx.conf .
```

#### Trouble shoot tips

#### commands

- basic

  ```
  $ docker --version
  ```
  
- run

  ```
  $ docker run docker/whalessay cowsay boo
  $ docker run --name=w1 docker/whalessay cowsay boo
  $ docker ps -a
  $ docker container ls -a
  CONTAINER ID  IMAGE           COMMAND       CREATED         STATUS                      PORTS   NAMES
  525d1fedb133  docker/whalesay "cowsay boo"  16 minutes ago  Exited (0) 16 minutes ago           w1
  b73451d9e1e6  docker/whalesay "cowsay boo"  2 hours ago     Exited (0) 2 hours ago              elated_heisenberg
  $ docker container rm w1
  ```

- sample operation

  ```
  $ docker ps
  $ docker exec <docker_name> ls

  ######
  # login into container
  ######
  $ docker exec -it <docker_name> bash
  [root@awx]# hostname
  awx
  [root@awx]# ^D
  $
  ```

#### proxy

- docker daemon

  - docker hub�Ƃ̂����ȂǂŗL��

```
# vi /etc/sysconfig/docker
HTTPS_PROXY=http://web-proxy.jp.hpecorp.net:8080
HTTP_PROXY=http://web-proxy.jp.hpecorp.net:8080
```

- docker file

```
# Dockerfile���Ŋ��ϐ���ݒ�
FROM ubuntu:13.10
ENV http_proxy <HTTP_PROXY>
ENV https_proxy <HTTPS_PROXY>
RUN apt-get -y update
```

- docker container

  - docker�R���e�i������O���l�b�g���[�N�Ƃ��Ƃ肷��ꍇ


```
# docker run��-e�I�v�V�������g����http_proxy���ϐ���ݒ�

$ docker run -d -e "http_proxy=<HTTP_PROXY>" \
	progrium/buildstep /build/builder
```


#### packages

## docker hub

```
$ docker login

$ docker search awx

$ docker pull ansible/awx_web
$ docker pull ansible/awx_task

$ docker images

REPOSITORY                       TAG                 IMAGE ID            CREATED             SIZE
docker.io/ansible/awx_task       2.0.1               72d87d7cb877        33 hours ago        891 MB
docker.io/ansible/awx_task       latest              72d87d7cb877        33 hours ago        891 MB
docker.io/ansible/awx_web        latest              8d38a55b3df2        33 hours ago        861 MB
docker.io/postgres               9.6                 89bf0dc0dee0        11 days ago         229 MB
docker.io/memcached              alpine              01e7979fa175        2 weeks ago         8.97 MB
docker.io/ansible/awx_web        2.0.1               3a0327def874        2 weeks ago         998 MB
docker.io/ansible/awx_rabbitmq   3.7.4               e08fe791079e        7 months ago        85.6 MB

```
