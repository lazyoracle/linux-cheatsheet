# Collection of frequently used Linux commands

## List directory contents with size

```bash
# works in bash
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