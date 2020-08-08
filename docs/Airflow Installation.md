아래 설치 명령들은 우분투를 기반으로 한다.

http://www.marknagelberg.com/getting-started-with-airflow-using-docker/

## Docker 설치

먼저 Docker를 설치하기 위헤 apt 패키지 매니저 자체를 업데이트한다:

```
$ sudo apt-get update
$ sudo apt-get install -y apt-transport-https     ca-certificates     curl     gnupg-agent     software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88
$ sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
$ sudo apt-get update
```

이제 Docker를 설치한다.

```
$  sudo apt-get install -y docker-ce docker-ce-cli containerd.io
```

다음으로 hello-world를 실행하여 설치가 제대로 되었는지 확인한다. 출력문에 "Hello from Docker!"가 있어야 한다.

```
$  sudo docker run hello-world
```



## Airflow 설치

먼저 dags 폴더를 하나 만든다. 이 폴더 아래 Python으로 만든 DAG 코드가 존재해야 한다.
```
$ pwd
/home/ubuntu/
$ mkdir dags
$ ls -tl
total 4
drwxrwxr-x 2 ubuntu ubuntu 4096 Aug  3 02:43 dags
```

### Docker를 이용해 Airflow 설치

간단 설치 방법:

```
$ sudo docker pull puckel/docker-airflow
```

Airflow가 설치된 Docker 이미지 이름을 보는 방법 (puckel/docker-airflow를 찾는다)

```
$ sudo docker images 
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
puckel/docker-airflow   latest              ce92b0f4d1d5        5 months ago        797MB
hello-world             latest              bf756fb1ae65        7 months ago        13.3kB
```

더 자세히 설치하려면 https://medium.com/@xnuinside/quick-guide-how-to-run-apache-airflow-cluster-in-docker-compose-615eb8abd67a를 참고

### Airflow 실행

다음으로 이 폴더를 dags 폴더로 지정해서 Airflow를 실행한다.
```
sudo docker run -d -p 8080:8080 -v /home/ubuntu/dags:/usr/local/airflow/dags puckel/docker-airflow webserver
```

### Airflow 웹서버 방문

http://호스트이름:8080/

![](images/airflow-docker.png)


## 기타 


### Airflow DAG 폴더로 이동하기

"sudo docker ps"를 명령을 실행하여 NAMES 컬럼밑에 나오는 이름을 기억한다. 이를 가지고 "sudo docker exec -ti"으로 컨테이너 안으로 이동한다.

```
$ sudo docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                                        NAMES
a56dbb111b5b        puckel/docker-airflow   "/entrypoint.sh webs…"   21 minutes ago      Up 21 minutes       5555/tcp, 8793/tcp, 0.0.0.0:8080->8080/tcp   angry_wu

$ sudo docker exec -ti angry_wu bash
airflow@a56dbb111b5b:~$ pwd
/usr/local/airflow
airflow@a56dbb111b5b:~$ airflow list_dags
-------------------------------------------------------------------
DAGS
-------------------------------------------------------------------
example_bash_operator
example_branch_dop_operator_v3
example_branch_operator
example_complex
example_external_task_marker_child
example_external_task_marker_parent
example_http_operator
example_passing_params_via_test_command
example_pig_operator
example_python_operator
example_short_circuit_operator
example_skip_dag
example_subdag_operator
example_subdag_operator.section-1
example_subdag_operator.section-2
example_trigger_controller_dag
example_trigger_target_dag
example_xcom
latest_only
latest_only_with_trigger
test_utils
tutorial
```

## Airflow Container 실행 중단하기

먼저 Airflow Docker instance의 이름을 알아낸다 (앞서 sudo docker ps 명령을 사용하여 이름을 알아낸다) 
```
$ sudo docker stop angry_wu
```

## data-engineering repo 받기

```
$ git clone https://github.com/keeyong/data-engineering.git
```


