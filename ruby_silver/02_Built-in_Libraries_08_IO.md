### Public class methods

- binread
- binwrite
- copy_stream
- foreach
- new
- open
- pipe
- popen
- read
Opens the stream, reads and returns some or all of its content, and closes the stream; returns nil if no bytes were read.

When called from class IO (but not subclasses of IO), this method has potential security vulnerabilities if called with untrusted input; see Command Injection.

The first argument must be a string that is the path to a file.

```ruby
```

- readlines
- select
- sysopen
- try_convert
- write



### Public instance methods

- close
- each

- eof?
Returns true if the stream is at the end of the file.

- gets
- read
- readline
- rewind
- seek
- to_i
- to_io
- to_path


### Constant
- SEEK_CUR
Seek from the current position.
- SEEK_END
Seek from the end of the file.
- SEEK_SET
Seek from the beginning of the file.
