# Collection of frequently used Linux commands

Handy list of oft-used Linux commands that I will never remember. Not intended to be an all-purpose cheatsheet, highly specific to my limited usage. These commands work in `bash`. For `fish`, the variable substitutions and wildcards need to be changed.

- [List directory contents with size](#list-directory-contents-with-size)
- [Count number of items in directory](#count-number-of-items-in-directory)
- [Using rsync](#using-rsync)
- [Monitor GPU usage](#monitor-gpu-usage)
- [Essential Installs in Ubuntu LTS Minimal](#essential-installs-in-ubuntu-lts-minimal)
- [Session management using screen](#session-management-using-screen)
- [Docker 101](#docker-101)

## List directory contents with size

```bash
du -sch * .[!.]* | sort -rh
```

## Count number of items in directory

```bash
ls <directory path> -1 | wc -l
```

## Using rsync

```bash
# https://www.danielms.site/blog/rsync-cheatsheet/
# -avzPn --stats -h --> archive verbose compressed progress-bar dry-run human-readable-stats

# pull files from remote server

rsync -avzPn --stats -h username@remote_host:/home/username/directory_we_want ~/local/directory

# push files to remote
rsync -avzPn ~/local/directory username@remote_host:/home/username/destination_dir

# using ssh key
rsync -avzPn -e "ssh -i ~/ec2_keyfile.pem" user@remote:/home/folder /tmp/local_system/
```

## Monitor GPU usage

```bash
# Check GPU usage and update every 1 s
watch -n1 nvidia-smi
```

## Essential Installs in Ubuntu LTS Minimal

```bash
sudo apt install fish wget bzip2 curl git gcc g++ python3-dev build-essential
```

## Session management using screen

```bash
# detach a session
Ctrl + a + d

# list all sessions
screen -ls

# resume session
screen -r

# run command and detach
screen -d -m <command>
```

## Docker 101

```bash
# docker-compose
docker-compose up --build
docker-compose down

# build a docker image tagged my_image with context in the current working directory
docker build --tag my_image:1.0 .

# run a detached container on 8080, remove after shutdown, with pseudo-tty, named container_name
# with $HOME/data mounted on $HOME/app/data, shared memory increased to 10GB, from image ubuntu:18.04
docker run -d -p 8080:8080 --rm --tty --name container_name --volume $HOME/data:$HOME/app/data --shm-size 10G ubuntu:18.04

# remove all containers
docker container rm $(docker container ls -aq)

# remove all dangling images
docker image prune -a
```
