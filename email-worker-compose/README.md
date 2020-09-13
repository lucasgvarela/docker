Minor improvements have been done to this version compared to the original course.

1. You can see in everytime I use the volumes to map a folder from my local pc to the container, I put the :z in the end of the line, that way SELinux will work fine without blocking the volume mapping.
2. The version used in the course for psycopg2==2.7.1 was broken "ImportError: /usr/local/lib/python3.6/site-packages/psycopg2/.libs/libresolv-2-c4c53def.5.so: symbol __res_maybe_init version GLIBC_PRIVATE not defined in file libc.so.6 with link time reference" to fix that I updated the version of psycopg2==2.8.6 this is the latest version available today.

Some tips:
1. To scale more than one worker use "docker-compose up -d --scale worker=3"
e.g.
[lucas@lucasmachine email-worker-compose]$ docker-compose ps -a
Name   Command   State   Ports
--
$ docker-compose up -d --scale worker=3
Creating network "email-worker-compose_banco" with the default driver
Creating network "email-worker-compose_fila" with the default driver
Creating network "email-worker-compose_web" with the default driver
Creating email-worker-compose_queue_1 ... done
Creating email-worker-compose_db_1     ... done
Creating email-worker-compose_worker_1 ... done
Creating email-worker-compose_worker_2 ... done
Creating email-worker-compose_worker_3 ... done
Creating email-worker-compose_app_1    ... done
Creating email-worker-compose_frontend_1 ... done
[lucas@lucasmachine email-worker-compose]$ docker-compose ps -a 
             Name                            Command               State         Ports       
---------------------------------------------------------------------------------------------
email-worker-compose_app_1        bash ./app.sh                    Up                        
email-worker-compose_db_1         docker-entrypoint.sh postgres    Up      5432/tcp          
email-worker-compose_frontend_1   nginx -g daemon off;             Up      0.0.0.0:80->80/tcp
email-worker-compose_queue_1      docker-entrypoint.sh redis ...   Up      6379/tcp          
email-worker-compose_worker_1     /usr/local/bin/python work ...   Up                        
email-worker-compose_worker_2     /usr/local/bin/python work ...   Up                        
email-worker-compose_worker_3     /usr/local/bin/python work ...   Up         
2. To follow the logs of a specific container use "docker-compose logs -t -f worker"
e.g.
$ docker-compose logs -t -f worker
Attaching to email-worker-compose_worker_1
worker_1    | 2020-09-13T16:19:01.169581000Z Aguardando mensagens ...
worker_1    | 2020-09-13T16:19:16.132389000Z Mandando a mensagem: test
worker_1    | 2020-09-13T16:19:43.158761000Z Mensagem test enviada