## File and Directory Permissions

## Steps
1. Create a directory /devops_workspace
2. Create a file called project_notes.txt
3. Set the permissions
     - The owner can read and write
     - The group can only read
     - Others have no access

## Commands Used
1. Create directory
```
mkdir -p /devops_workspace
```
The `mkdir -p`  command ensures the directory is created only if it does not exist.
2. Create file

```
touch project_notes.txt
```
3. Set permission
```
chmod 640 /devops_workspace/project_notes.txt
```
4. Verify permissions
```
ls -l /devops_workspace/project_notes.txt
```

## Output
```
-rw-r----- 1 user group 0 Feb 9 12:00 /devops_workspace/project_notes.txt
```

| Symbol            | Meaning  |
| :----------------:| :------: |
| -                 |  File type (- means a regular file, d would mean a directory)   |
| rw-               |   Owner (user) permissions: Read (r), Write (w), No Execute (-)   |
| r--               |  Group permissions: Read (r), No Write (-), No Execute (-)   |
| ---               |  Others (everyone else): No Read (-), No Write (-), No Execute (-)   |



