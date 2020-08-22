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
