# Collection of frequently used Linux commands

Handy list of oft-used Linux commands that I will never remember. Not intended to be an all-purpose cheatsheet, highly specific to my limited usage.

- [List directory contents with size](#list-directory-contents-with-size)
- [Count number of items in directory](#count-number-of-items-in-directory)
- [Using rsync](#using-rsync)
- [Monitor GPU usage](#monitor-gpu-usage)

### List directory contents with size

```bash
# works in bash
du -sch * .[!.]* | sort -rh
```

### Count number of items in directory

```bash
ls <directory path> -1 | wc -l
```

### Using rsync

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

### Monitor GPU usage

```bash
# Check GPU usage and update every 1 s
watch -n1 | nvidia-smi
```