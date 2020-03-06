# tangata-manu-stack-docker

Clone this repository

```
git clone https://github.com/Atomicwallet/tangata-manu-stack-docker.git
```

```
git clone --single-branch --branch master-shelley https://github.com/Emurgo/tangata-manu
cd ./tangata-manu
git submodule update --init --recursive
```

## Run development enviroment

Vagrant and Virtualbox should have installed (MacOS)

```
brew cask install virtualbox
brew cask install vagrant
```

Change project directory

```
cd ./tangata-manu
```

Start vargant

```
vagrant up
```

Give you access to a shell

```
vagrant ssh
```

Start tangata-manu stack

```
vagrant:$ cd /opt/project
vagrant:$ sudo docker-compose up -d
```

Restart containers if needed

```
vagrant:$ sudo docker-compose stop
vagrant:$ sudo docker-compose up -d
```

## Commands

**Show running process**

```
sudo docker ps

```
output

```
CONTAINER ID        IMAGE                         COMMAND                  CREATED              STATUS              PORTS               NAMES
f06d7c52d5d9        tangata-manu-image:latest     "yarn run prod"          About a minute ago   Up 11 seconds                           project_tangata-manu_1
457a96b08381        project_cardano-http-bridge   "/opt/app/cardano-ht…"   34 minutes ago       Up 34 minutes                           project_cardano-http-bridge_1
42ea69854c84        postgres:12.1                 "docker-entrypoint.s…"   34 minutes ago       Up 34 minutes                           project_postgres_1
```

**Logs**

cardano-http-bridge
```
sudo docker logs -f project_cardano-http-bridge_1
```

tangata-manu
```
sudo docker logs -f project_tangata-manu_1
```

**Connect to container**

tangata-manu
```
vagrant:$ sudo docker exec -it  project_tangata-manu_1 /bin/bash
```

**Connect to darabase**

Postgresql
```
vagrant:$ docker exec -it project_postgres_1 psql -h 127.0.0.1 -p 5432 -U postgres -w postgres
```

postgres shell

```
postgres=# 
postgres=#\c yoroi_blockchain_importer
yoroi_blockchain_importer=# \d
                 List of relations
 Schema |        Name         |   Type   |  Owner   
--------+---------------------+----------+----------
 public | bestblock           | table    | postgres
 public | blocks              | table    | postgres
 public | pgmigrations        | table    | postgres
 public | pgmigrations_id_seq | sequence | postgres
 public | transient_snapshots | table    | postgres
 public | tx_addresses        | table    | postgres
 public | txs                 | table    | postgres
 public | utxos               | table    | postgres
 public | utxos_backup        | table    | postgres
(9 rows)
```