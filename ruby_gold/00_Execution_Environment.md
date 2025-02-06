### Pre-defined Variables
- **$0**: The name of the file being executed.
- $*: Command-line arguments passed to the script as an array.
- $$: The process ID of the Ruby script.
- $?: The exit status of the last executed child process.
- $: or $LOAD_PATH: An array of paths that Ruby searches for required files.
- $" or $LOADED_FEATURES: An array of files that have already been required.

- $stdin: Standard input stream (default is keyboard input).
- $stdout: Standard output stream (default is the terminal).
- $stderr: Standard error output stream.

- $~: The last match object.
- $&: The entire matched string.
- $1, $2, ...: Capture groups from the last match.
- $+: The last capture group from the last match.
- $': The string after the last match.
- `$``: The string before the last match.

- $DEBUG: A boolean indicating if the Ruby interpreter is running in debug mode (-d flag).
- $VERBOSE: Controls warnings. true for verbose mode, nil to suppress warnings.
- $-w: Alias for $VERBOSE.

- $LOAD_PATH: Paths Ruby will search when requiring files.
- $FILENAME: The current file being read by a DATA segment.

### Pre-defined Constants
- RUBY_VERSION: The version of Ruby (e.g., "3.2.0").
- RUBY_PLATFORM: The platform Ruby is running on (e.g., "x86_64-linux").
- RUBY_ENGINE: The Ruby engine in use (e.g., "ruby", "jruby").
- RUBY_RELEASE_DATE: Release date of the Ruby version.
- RUBY_DESCRIPTION: A string containing Ruby version, engine, platform, and patchlevel.

- __FILE__: The current file's name.
- __LINE__: The current line number.
- __ENCODING__: The encoding of the current file (e.g., UTF-8).
- __END__: The end of the script, after the DATA section.

- ARGV: The array of command-line arguments passed to the script (similar to $*).
- DATA: A file-like object representing the data section of the script (after __END__).
- ENV: A hash-like object containing environment variables.

### Flags
- -0[octal]       specify record separator (\0, if no argument)
- -a              autosplit mode with -n or -p (splits $_ into $F)
- -c              check syntax only
- -Cdirectory     cd to directory before executing your script
- -d              set debugging flags (set $DEBUG to true)
- -e 'command'    one line of script. Several -e's allowed. Omit [programfile]
- -Eex[:in]       specify the default external and internal character encodings
- -Fpattern       split() pattern for autosplit (-a)
- -i[extension]   edit ARGV files in place (make backup if extension supplied)
- -Idirectory     specify $LOAD_PATH directory (may be used more than once)
- -l              enable line ending processing
- -n              assume 'while gets(); ... end' loop around your script
- -p              assume loop like -n but print line also like sed
- -rlibrary       require the library before executing your script
- -s              enable some switch parsing for switches after script name
- -S              look for the script using PATH environment variable
- -v              print the version number, then turn on verbose mode
-  -w              turn warnings on for your script
-  -W[level=2|:category]     set warning level; 0=silence, 1=medium, 2=verbose
-  -x[directory]   strip off text before #!ruby line and perhaps cd to directory
-  --jit           enable JIT for the platform, same as --yjit
-  --yjit          enable in-process JIT compiler
-  --rjit          enable pure-Ruby JIT compiler (experimental)
-  -h              show this message, --help for more info
