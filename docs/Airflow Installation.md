아래 설치 명령들은 우분투를 기반으로 한다.

http://www.marknagelberg.com/getting-started-with-airflow-using-docker/

## Docker 설치

먼저 Docker를 설치하기 위헤 apt 패키지 매니저 자체를 업데이트한다:

```
$ sudo apt-get update
$ sudo apt-get install     apt-transport-https     ca-certificates     curl     gnupg-agent     software-properties-common

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88
$ sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
$ sudo apt-get update
```

이제 Docker를 설치하고 다음으로 hello-world를 실행하여 설치가 제대로 되었는지 확인한다.

```
$  sudo apt-get install docker-ce docker-ce-cli containerd.io
$  sudo docker run hello-world
```

## Docker Compose 설치

```
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
```

## Airflow 설치

간단 설치 방법:

```
$ docker pull puckel/docker-airflow

$ docker images
```

더 자세히 설치하려면 https://medium.com/@xnuinside/quick-guide-how-to-run-apache-airflow-cluster-in-docker-compose-615eb8abd67a를 참고

## Airflow 실행

먼저 dags 폴더를 하나 만든다
```
$ pwd
/home/ubuntu/
$ mkdir dags
$ ls -tl
total 4
drwxrwxr-x 2 ubuntu ubuntu 4096 Aug  3 02:43 dags
```

다음으로 이 폴더를 dags 폴더로 지정해서 Airflow를 실행한다.
```
sudo docker run -d -p 8080:8080 -v /home/ubuntu/dags:/usr/local/airflow/dags puckel/docker-airflow webserver
```
![](images/airflow-docker.png)


## Airflow DAG 폴더로 이동하기

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

```
$ sudo docker stop angry_wu
```

## data-engineering repo 받기

```
$ git clone git@github.com:keeyong/data-engineering.git
```


