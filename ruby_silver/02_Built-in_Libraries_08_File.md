A File is an abstraction of any file object accessible by the program and is closely associated with class IO

### Read/Write Mode
Read/Write Modes for Existing File

|------|-----------|----------|----------|----------|-----------|
| R/W  | Initial   |          | Initial  |          | Initial   |
| Mode | Truncate? |  Read    | Read Pos |  Write   | Write Pos |
|------|-----------|----------|----------|----------|-----------|
| 'r'  |    No     | Anywhere |    0     |   Error  |     -     |
| 'w'  |    Yes    |   Error  |    -     | Anywhere |     0     |
| 'a'  |    No     |   Error  |    -     | End only |    End    |
| 'r+' |    No     | Anywhere |    0     | Anywhere |     0     |
| 'w+' |    Yes    | Anywhere |    0     | Anywhere |     0     |
| 'a+' |    No     | Anywhere |   End    | End only |    End    |
|------|-----------|----------|----------|----------|-----------|

Read/Write Modes for \File To Be Created

|------|----------|----------|----------|-----------|
| R/W  |          | Initial  |          | Initial   |
| Mode |  Read    | Read Pos |  Write   | Write Pos |
|------|----------|----------|----------|-----------|
| 'w'  |   Error  |    -     | Anywhere |     0     |
| 'a'  |   Error  |    -     | End only |     0     |
| 'w+' | Anywhere |    0     | Anywhere |     0     |
| 'a+' | Anywhere |    0     | End only |    End    |
| 'r+' | Anywhere |    0     | Anywhere |     0     |
|------|----------|----------|----------|-----------|

- w: open write only mode, content will be removed when opened or seeked
- w+: open read/write mode, content will be removed when opened or seeked
- a: open write only mode, content will be added to the end of the file when opened
- a+: open read/write mode, content will be added to the end of the file when opened, you can change the file position with **seek**
- r+: open read/write mode, you can change the file position with **seek**
- r: is default mode, open read only mode

### Creating
#### new
#### open
#### link
#### mkfifo
#### symlink

### Querying
#### Paths
- absolute_path
- basename
- dirname
- extname
- path
- size

#### Times
- atime
- ctime
- mtime
- birthtime

#### Types
- blockdev?
- chardev?
- directory?
- executable?
- executable_real?
- file?
- grpowned?
- owned?
- pipe?
- socket?
- symlink?
- writable?
- writable_real?

#### Contents
- empty?
- size

#### Settings
- chmode
- chown
- lchmod
- lchown
- lutime
- rename

#### Other
- truncate
- unlink



