# Collection of frequently used Linux commands

Handy list of oft-used Linux commands that I will never remember. Not intended to be an all-purpose cheatsheet, highly specific to my limited usage. These commands work in `bash`. For `fish`, the variable substitutions and wildcards need to be changed.

- [Shell 101](#shell-101)
  - [List directory contents with size](#list-directory-contents-with-size)
  - [Count number of items in directory](#count-number-of-items-in-directory)
  - [Monitor GPU usage](#monitor-gpu-usage)
  - [File Permissions](#file-permissions)
  - [SSH 101](#ssh-101)
  - [rsync](#rsync)
  - [Networking](#networking)
  - [fish](#fish)
  - [wget](#wget)
  - [grep](#grep)
- [System Setup](#system-setup)
  - [Essential Installs in Ubuntu LTS Minimal](#essential-installs)
  - [Increasing swap memory](#increasing-swap)
- [Session management using screen](#session-management-using-screen)
- [Docker 101](#docker-101)
- [Git 101](#git-101)
- [make Basics](#make-basics)

## Shell 101

### List directory contents with size

```bash
du -sch * .[!.]* | sort -rh
```

### Count number of items in directory

```bash
ls <directory path> -1 | wc -l
```

### Monitor GPU usage

```bash
# Check GPU usage and update every 1 s
watch -n1 nvidia-smi
```

### File permissions

```bash
# 400 read by owner
# 040 read by group
# 004 read by anybody
# 200 write by owner
# 020 write by group
# 002 write by anybody
# 100 execute by owner
# 010 execute by group
# 001 execute by anybody

sudo chmod XXX <filepath>

# ssh public key file 644 
# ssh private key file 600
# ssh directory 700
```

### SSH 101

```bash
# connect to hostname with key and port forward
ssh -L <remote_port>:localhost:<local_port> -i /path/to/key username@hostname
```

### rsync

```bash
# -avzPn --stats -h --> archive verbose compressed progress-bar dry-run human-readable-stats

# pull files from remote server

rsync -avzPn --stats -h username@remote_host:/home/username/directory_we_want ~/local/directory

# push files to remote
rsync -avzPn ~/local/directory username@remote_host:/home/username/destination_dir

# using ssh key
rsync -avzPn -e "ssh -i ~/ec2_keyfile.pem" user@remote:/home/folder /tmp/local_system/
```

More details on [rsync cheatsheet](https://www.danielms.site/blog/rsync-cheatsheet/)

### Networking

```bash
# Get PID of a process running on port YYYY
sudo netstat -lpn | grep :YYYY
# OR
fuser 8080/tcp (add -k to kill the process too)
```

### fish

```bash
# Set password of user account on GCP VM
sudo passwd $USER
# set fish as default shell
chsh -s `which fish`
```

### wget

```bash
# recursive no-parent Reject no-Host robots.txt X_no_of_dirs
wget -r -np -R "index.html" -nH -e robots=off --cut-dirs=X <download_URL>
```

### grep

```bash
# find pattern in files
grep --include=\*.{py, c, h} --exclude=\*.o --exclude-dir={dir1,dir2,*.dst} -rnw '/path/to/somewhere/' -e "pattern"
```
More `grep` funda [here](https://www.grymoire.com/Unix/Grep.html)

## System Setup
### Essential Installs

```bash
sudo apt install -y fish wget bzip2 curl git gcc g++ python3-dev build-essential vim nano rsync htop tree screen libatlas-base-dev libboost-all-dev libopenblas-dev
```

### Increasing swap 

```bash
# create swap file
sudo fallocate -l 1G /swapfile
# permissions set to root
sudo chmod 600 /swapfile
# setup a linux swap area
sudo mkswap /swapfile
# activate swap
sudo swapon /swapfile
# verify
sudo swapon --show
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

# add session name
screen -S <session name>

# add name to attached session
Ctrl + a + : + sessionname name

# attach to named session
screen -xS name

# detach and attach to session (useful when stuck in ssh)
screen -d -r

# attach to session without detaching
screen -x
```

## Docker 101

```bash
# docker-compose
docker-compose up --build
docker-compose down

# build a docker image tagged my_image with context in the current working directory
docker build --tag my_image:1.0 .

# run a detached container on 8080, remove after shutdown, with interactive pseudo-tty, named container_name
# with $HOME/data mounted on $HOME/app/data, shared memory increased to 10GB, from image ubuntu:18.04
docker run -d -p 8080:8080 --rm --tty -i --name container_name --volume $HOME/data:$HOME/app/data --shm-size 10G ubuntu:18.04

# remove all containers
docker container rm $(docker container ls -aq)

# force remove all images older than 12 hours/until 2020-08-04
docker image prune -a --force --filter "until=12h"
docker image prune -a --force --filter "until=2020-08-04T00:00:00"

# check docker resource usage
docker stats

# Execute command in a running container in an interactive terminal
docker exec -it <container name> <command>
```
More at: [Docker Official Cheatsheet](https://www.docker.com/sites/default/files/d8/2019-09/docker-cheat-sheet.pdf) and [DevTools](https://phase2.github.io/devtools/)

## Git 101

```bash
# setup local project with remote
git init
git add .
git commit -m "init commit"
git remote add origin <remote repo url>
git push --set-upstream origin master

# setup local branch with remote
git checkout -b cool-feature
git add .
git commit -m "branch init commit"
git push --set-upstream origin cool-feature

# change remote for your local repo, useful when synching your fork
git remote set-url origin <repo url>

# https://www.atlassian.com/git/tutorials/undoing-changes
# https://stackoverflow.com/a/24569160

# git reset
# undo 2 commits and unstage files
git reset HEAD~2 # git reset --mixed HEAD~2

# undo 2 commits and leave changes staged
git reset --soft HEAD~2

# undo 2 commits and delete all changes
git reset --hard HEAD~2

# git revert
#Undo last 2 commits with new commit (without altering history)
git revert HEAD~2

# git checkout
# move around to a branch/commit
git checkout <branch> or git checkout <commit-SHA>

# remove staged file from index
git rm <file-name>

# revert merge
git log #check the commit hash and parent id
# revert working tree to commit-hash on parent branch 1
git revert <commit-hash> -m 1

# move commits to another existing branch
git checkout existingbranch
git merge master
git checkout master
git reset --hard HEAD~3 # Go back 3 commits. You *will* lose uncommitted work.
git checkout existingbranch

# move commits to another new branch
git branch newbranch      # Create a new branch, saving the desired commits
git reset --hard HEAD~3   # Move master back by 3 commits or put a commit hash
git checkout newbranch    # Go to the new branch that still has the desired commits
```

[Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials)

## make Basics

Make `target` for dependencies `dep1` and `dep2` using recipe specified by `command`
```
target: dep1 dep2
	command arguments

dep%: subdep1 subdep2
	command arguments
```
* [Metaprogamming - MIT Missing Semester](https://missing.csail.mit.edu/2020/metaprogramming/)
* [`make` Standard Targets](https://www.gnu.org/software/make/manual/html_node/Standard-Targets.html#Standard-Targets)
